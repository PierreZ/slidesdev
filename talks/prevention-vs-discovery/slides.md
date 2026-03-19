---
theme: slidev-theme-pz
title: "Testing: Prevention vs Discovery"
---

# Testing: Prevention vs Discovery

Pierre Zemb — Clever Cloud

---

# Who Am I

- Pierre Zemb — Clever Cloud (French cloud provider)
- Distributed systems engineer — dev + on-call
- Open source: foundationdb-rs, moonpool
- "Le chat noir" — always on-call when the weird stuff happens

---

# The HDFS Incident

**Story: Network partition then NullPointerException on restart**

1. Normal day, then: network partition + disk full on journal nodes
2. 70-machine Hadoop cluster goes belly-up
3. Can't self-heal, reboot, **NullPointerException at startup**
4. Bug was known. Patched in newer HDFS version.
5. Emergency: backport patch, recompile, redeploy on 70 machines at 3am

**The question:** Why does a NullPointerException happen during *recovery*?

---

# The LLM Acceleration Problem

**LLMs generate code faster than ever — but who catches the bugs they introduce?**

- *Amazon, March 2026*
  > "Trend of incidents" with "high blast radius" and "Gen-AI assisted changes" — Amazon internal briefing, reported by the Financial Times

  Junior and mid-level engineers now require senior sign-off for any AI-assisted changes.

- *SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases — Chen et al., 2026*
  > "Current LLMs still struggle to sustain code quality over extended evolution, particularly in controlling regressions."

**More code, faster. Same blind spots. More potential bugs.**

---

# Dev vs Prod

**Learning to drive ≠ Driving in Paris**

- Dev = passing the theory exam
- Prod = driving in Paris rush hour
- The code didn't account for the complexity of the real world
- Recovery paths, split brain, cascading failures — none of this exists on localhost

---

# Your System Interacts With Two Things

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-4">
    <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">Code</div>
    <div class="text-2xl">↓</div>
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">Users</div>
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">World</div>
    </div>
  </div>
</div>

- **Users**: unpredictable behavior, edge cases, adversarial inputs
- **World**: machines die, networks partition, disks corrupt, clocks drift, dependencies fail

---

# The Combinatorial Explosion

**E-commerce API example:**

| Dimension | Values |
|-----------|--------|
| User type | Guest, Registered, Premium, Business |
| Payment | Credit Card, PayPal, Bank Transfer |
| Shipping | Standard, Express, Pickup |
| Promotion | None, Percentage, Fixed |
| Inventory | In stock, Low stock, Backorder |
| Currency | EUR, USD, GBP |

---

# The Combinatorial Explosion

- **648** test combinations to cover all paths
- Add one option per dimension → **4,000+ tests**
- Real project: 300 lines of feature code, **10,000 lines of tests**, 1.5 months of work

> "You test what you imagine. Bugs hide in combinations you didn't."

LLMs generate code fast but don't imagine edge cases either. More code, same blind spots, more potential bugs.

---

# "Your replicas protect you"

*Redundancy Does Not Imply Fault Tolerance — Ganesan et al., FAST 2017*

> "A single file-system fault can cause catastrophic outcomes such as data loss, corruption, and unavailability"

Across 8 production systems: Redis, ZooKeeper, Cassandra, Kafka, MongoDB, RethinkDB, CockroachDB, LogCabin.

**Kafka example**: One corrupted log entry on the leader → leader ignores it → instructs followers to do the same → followers hit fatal assertion → **entire cluster unavailable + data loss**

All problems observed at R=1 **persist at R=3**.

---

# "Your disks are reliable"

*Disks Lie — TigerBeetle Safety Docs*

- 0.031% of SSDs per year: silent corruption
- 1.4% of enterprise HDDs per year: silent corruption
- Misdirected I/O: measurable rates across all disk types

*ZooKeeper Disk Corruption*

- Recovered in only **2%** of disk corruption cases
- 30% of block corruption leads to data loss or unavailability

---

# "Your disks are reliable" (cont.)

*Protocol-Aware Recovery for Consensus-Based Storage — Alagappan et al., FAST 2018*

- ext4 silently returns corrupted data to applications
- ZooKeeper's Truncate recovery: **causes silent data loss**

> "All consensus research papers assume the network can break, but disk/storage must be resilient. Very few papers address disk failures."

---

# "You'd notice a network partition"

*An Analysis of Network-Partitioning Failures in Cloud Systems — Alquraan et al., OSDI 2018*

- **80%** of partition failures have catastrophic impact
- **27%** lead to data loss
- **90%** are silent — no log, no alert, nothing
- **21%** cause permanent damage that persists after the partition heals
- **83%** need 3+ events to manifest — **the sequential luck problem**

---

# "Your error handling catches it"

*Simple Testing Can Prevent Most Critical Failures — Yuan et al., OSDI 2014*

- **92%** of catastrophic failures = incorrect handling of non-fatal errors
- **35%** of error handlers are empty, contain only a log statement, or have `TODO`/`FIXME`
- **77%** reproducible by a unit test — **if someone had thought to write it**
- **98%** manifest on ≤ 3 nodes

---

# "Your error handling catches it" (cont.)

*TaxDC: A Taxonomy of Non-Deterministic Concurrency Bugs — Leesatapornwongsa et al., ASPLOS 2016*

> "More than three quarters of the bugs involve background protocols" — not the user-facing foreground protocols developers typically test

Triggered by: untimely message delivery, untimely faults or reboots, or combinations of both.

---

# "Your retries save you"

*Metastable Failures in the Wild — Huang et al., OSDI 2022*

The system enters a bad state that persists even after the trigger is removed.

> "Retries sustain more than 50% of metastable incidents"

> "Changes meant to improve the common case — fast paths, caches, retries, failover, load balancing, autoscaling — all make the failure state less resource-efficient"

- 22 incidents studied from 11 organizations
- Outages: **1.5 to 73.5 hours**
- Universally observed from hyperscalers to small companies

**Your retry logic, your circuit breakers, your autoscaling — they can sustain the failure.**

---

# The Thesis

**Passing tests don't mean your system is reliable.**
**They mean your system can handle the scenarios you imagined.**

What about the ones you didn't?

The machine that dies halfway through a write. The retry storm nobody thought to reproduce in CI. The network partition that shows up at exactly the wrong moment.

**Those are the scenarios that page you at 3am. And no test was written for them.**

> "The very reason tests are needed — humans can't enumerate all control flow branches — is what makes it impossible for humans to write comprehensive tests."
> — Will Wilson

---

# Prevention vs Discovery

|  | Prevention | Discovery |
|--|-----------|-----------|
| **Question** | "Did we break what used to work?" | "What else is broken that we haven't found yet?" |
| **Method** | Regression tests, CI, code review | Simulation, fault injection, property-based testing |
| **Finds** | Known bugs returning | Unknown bugs lurking |
| **Scales** | Linearly with developer effort | Multiplicatively with compute |
| **Coverage** | What developers imagined | What nobody imagined |

Both are needed. The industry does 99% prevention, ~0% discovery.

---
layout: section
---

# ACT 2: THE SOLUTION

---

# What Developers Want From Tests

- **Fast** — not 10 min CI, not waiting for containers to spin up
- **Debuggable** — not "works on my machine", not forensic log analysis
- **Full system** — not just isolated units, but real topologies
- **Robust** — no flaky tests, no `sleep()` in tests

> Who has written a `sleep()` in a test to wait for something to happen?

---

# Strategy: Test the Worst Case

**"What if dev was *worse* than prod?"**

- Test the **worst version** of your users (adversarial inputs, race conditions, edge cases)
- Test the **worst version** of the world (partitions, disk corruption, clock drift, cascading failures)
- If your system survives this, it's **overqualified for production**

---
layout: two-cols
---

::title::

# Simulating Users: Properties, Not Test Cases

::default::

**Specific test case (prevention)**

```java
@Test void guestCannotUseSavedCards() {
    User guest = new User(UserType.GUEST);
    assertFalse(guest.canUseSavedCards());
}
```

::right::

**Property (discovery)**

```java
@Property void savedCardsRequireAuth(
    @ForAll UserType type,
    @ForAll PaymentMethod method
) {
    User user = createUser(type);
    if (!user.isAuthenticated()) {
        assertFalse(user.canUseSavedCards());
    }
}
```

---

# Properties Look Like Specs

- Properties compile as code
- They work for ALL random inputs
- LLMs generate properties from specs more reliably than exhaustive individual test cases

**Instead of testing specific cases, write invariants that hold for ALL inputs.**

---

# From Tests to Test Generators

**Instead of writing N tests, write a test GENERATOR**

```java
// Instead of 648 individual test cases...
enum UserType { GUEST, REGISTERED, PREMIUM, BUSINESS }
enum PaymentMethod { CREDIT_CARD, PAYPAL, BANK_TRANSFER }

// ...write one generator
UserType user = randomEnum(UserType.class);
PaymentMethod payment = randomEnum(PaymentMethod.class);
// ... run the scenario
```

- Add a feature? Add one enum value, not 100 tests
- A test generator run long enough will eventually output all possible tests
- Libraries: jqwik (Java), Hypothesis (Python), proptest (Rust), QuickCheck (Haskell)

---

# Guided Exploration Beats Brute Force

**Pure random inputs don't work.**

Random button presses on Super Mario Bros: Mario dies in the first 5 seconds. Every time.

**Guided exploration beats the game.**

The same principle applies to your systems: randomly dropping packets isn't a network partition. Randomly failing requests isn't a realistic workload.

Smart randomness — stateful, correlated, guided — finds bugs that brute force never will.

---

# Simulating the World: What Can Fail?

Your code interacts with a world that lies, breaks, and surprises you.

- **Network**: partitions, latency spikes, packet loss, connection resets
- **Databases**: stale reads, lost updates, connection drops mid-transaction
- **External APIs**: timeouts, rate limits, partial failures, unexpected payloads
- **Storage**: corruption, slow disks, full disks, misdirected I/O
- **Time**: clock drift, NTP jumps, leap seconds
- **DNS, Auth, Caches**: resolution failures, token expiry, thundering herd

---

# Fakes: Simulating the World

> "The system under test shouldn't even be able to tell whether it is interacting with a real implementation or a fake."
> — Software Engineering at Google, Ch. 13

- A **fake** is a working in-memory implementation of your dependency. Stateful. Correct behavior. Injectable failures. Zero I/O.
- **Mocks** leak implementation details — 5 lines of `when().thenReturn()` that break when you refactor.
- **Testcontainers** are heavy, slow, non-deterministic, and can't inject specific faults.
- **Fakes** verify state, not calls. They survive refactoring. They run in milliseconds.

---

# Fakes Are Easier Than You Think

Google's SWE Book uses a `FakeFileSystem` backed by a `HashMap<String, String>`. That's it.

- File system? **A HashMap.**
- S3? **A HashMap.**
- DNS? **A HashMap.**
- Database? **A HashMap.**
- Network? **A buffer.**

> "A fake might not need to have 100% of the functionality of its corresponding real implementation."
> — Software Engineering at Google, Ch. 13

A fake with 80% of features is better than no fake at all.

---

# Choose Your Boundary

**The boundary depends on what you control.**

| What you control | Fake boundary |
|---|---|
| **External dependencies** (you don't own PostgreSQL) | Your service layer — `UserRepository`, `S3Client`, `QueuePublisher` |
| **Everything** (all code in simulation) | OS primitives — network, disk I/O, clock |

Don't fake PostgreSQL. Fake your access to it.

The fake can only be autonomous at the boundary you fully control.

---

# Model Real Failures, Not Documented Ones

**Don't trust the docs. Study your dependency's ACTUAL behavior.**

*Jepsen: MariaDB Galera Cluster 12.1.2* — MariaDB claims "no lost transactions" and "between Serializable and Repeatable Read."

Jepsen found — even in **healthy clusters** with zero faults:

- **Lost committed transactions** — values acknowledged as committed disappeared
- **Lost Update** — concurrent read-modify-write produced wrong results
- **Stale Read** — committed writes invisible to later transactions

> "read-modify-write patterns, like those used in many ORMs, are likely unsafe"

**Weaker than Read Uncommitted.** Your database fake should simulate what your database *actually does*, not what the documentation promises.

---

# Generate Enough Work

**If you only drop one disk at a time, you'll never explore your system.**

- Write workloads that combine operations — not just insert/delete but full workflows
- Generate valid AND invalid inputs
- Combine adversarial users with a hostile world — at scale

**Process vs Workload separation:**

- **Process** = system under test (crashes, reboots, loses state)
- **Workload** = test driver (never crashes, judges correctness)

Your cluster crashes and reboots. Your test driver survives, records what happened, and judges if the data is still consistent.

---

# Teach a Computer to Test

**Give a computer a test, it finds a bug. Teach a computer to test, it finds bugs forever.**

<div class="flex justify-center mt-6">
  <div class="flex items-center gap-4 text-lg">
    <div class="px-4 py-2 border-2 border-current rounded">Properties<br/><span class="text-sm opacity-70">(what to check)</span></div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">Randomness<br/><span class="text-sm opacity-70">(fakes + chaos)</span></div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">Enough work<br/><span class="text-sm opacity-70">(workloads)</span></div>
    <span class="text-2xl">=</span>
    <div class="px-6 py-3 border-2 rounded font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">Deterministic Simulation<br/>Testing (DST)</div>
  </div>
</div>

---

# Making It Deterministic

**Three sources of non-determinism to eliminate:**

1. **Thread scheduling** → single-threaded execution
2. **Real I/O** (network, disk, time) → simulated I/O
3. **Random number generation** → seeded PRNG

```
u64 seed → entire execution determined
Same seed = same decisions = same bugs. Every time.
```

A failing seed is a time machine. Rewind, inspect, experiment — no production risk.

---

# The Provider Pattern

**Abstract I/O behind traits, implement twice:**

<div class="flex justify-center mt-4">
  <div class="border-2 border-current rounded-lg overflow-hidden w-4/5">
    <div class="px-4 py-2 border-b-2 border-current text-center font-bold">Your Application (same code, always)</div>
    <div class="px-4 py-2 border-b-2 border-current text-center text-sm">Provider Traits: TimeProvider · NetworkProvider · StorageProvider · RandomProvider</div>
    <div class="grid grid-cols-2">
      <div class="px-4 py-3 border-r-2 border-current">
        <div class="font-bold mb-1">Production</div>
        <div class="text-sm">Real OS · Real TCP · Real fs · OS RNG</div>
      </div>
      <div class="px-4 py-3">
        <div class="font-bold mb-1">Simulation</div>
        <div class="text-sm">Event queue · In-memory network · In-memory fs · Seeded ChaCha8</div>
      </div>
    </div>
  </div>
</div>

Same code runs in both — no `#[cfg(test)]`, no conditional compilation.

---

# DST: The Ultimate LLM Feedback Loop

> "The most important thing to get great results out of Claude Code — give Claude a way to verify its work."
> — Boris Cherny, creator of Claude Code

**DST IS that feedback loop.**

<div class="flex justify-center mt-4">
  <div class="flex items-center gap-3 text-base">
    <div class="px-3 py-2 border-2 border-current rounded">LLM writes code</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">Simulation finds bug</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">LLM reads failing seed</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">LLM fixes + re-runs</div>
    <span>→</span>
    <span>🔁</span>
  </div>
</div>

All autonomously. No human in the loop.

---
layout: section
---

# ACT 3: IN PRACTICE

---

# Who Does Deterministic Simulation?

**This isn't theoretical.**

| Project | What | Scale |
|---------|------|-------|
| **FoundationDB** (Apple) | The pioneer. Built simulator before database. | Trillions of CPU-hours simulated |
| **TigerBeetle** | Financial transactions database | VOPR on 1024 cores |
| **Antithesis** | Deterministic hypervisor — works on ANY software | Customers find bugs in 2-3 weeks |
| **Dropbox** | Sync engine | Full simulation of sync protocol |
| **WarpStream** | Kafka-compatible streaming | Full SaaS tested with simulation |
| **Clever Cloud** | Materia distributed database | Team of 5 including 2 juniors |

---

# Clever Cloud / Materia

- 80 employees, **team of 5** (including 2 junior apprentices)
- Building a **distributed multi-model multi-tenant database** on FoundationDB
- Only team in the world that can run Rust code directly inside FDB's simulation
- **Simulation-driven development**: write the workload first, then implement the feature
- Components under simulation: SDK, KMS, ETCD, workflow engine, leader election

---

# Clever Cloud / Materia — Discoveries

**Concrete discovery:** simulation found that certain index types failed when a `delete all` operation passed through — an edge case nobody imagined, despite multiple team members knowing the indexing system well.

**On-prem business case:** Clever Cloud deploys on-prem where they don't control hardware quality. Simulation ensures software works on degraded networks and disks.

> "We would never have succeeded without simulation."

---

# foundationdb-rs: Discovery at Scale

- **~15 million downloads**, solo maintainer
- Used by Apache OpenDAL, SurrealDB, and others
- ~130 unsafe blocks requiring careful FFI management

**BindingTester**: randomized, seeded operations that explore the API surface continuously — not tests someone wrote, but combinations the generator discovers.

- Compares Rust implementation against reference implementations
- Runs **hourly on GitHub Actions** — ~219 days of continuous exploration per month
- Tests across multiple OS, FDB versions, Rust compiler versions

---

# Leader Election: LLM + Simulation

- Claude Code implemented a full ballot-based leader election on FoundationDB. Autonomously.
- It generated 13 machine-checkable invariants to verify its own work.
- Simulation threw network partitions, process crashes, and clock skew at Claude's code.
- The LLM proposes, simulation disposes.

---

# Simulation Is Not a Silver Bullet

- **Performance is invisible** — simulation assumes infinite CPU speed. You still need a perf/bench farm.
- **Your model can be wrong** — if you don't simulate a failure type, you won't find it.
- **Production fakes need chaos too** — your production implementations need their own fault injection.
- **Rare bugs need smart exploration** — brute force isn't enough.
- **Bug-finding latency** — a bug can hide in a seed for months after the feature shipped.

---
layout: section
---

# CONCLUSION

---

# Testing Must Evolve

Remember the HDFS incident? Network partition + disk full + restart = NullPointerException. That exact combination? It's a seed in a simulation. Found in seconds. Fixed before production. **No 3am wake-up call.**

Your tests check what you imagined. Simulation discovers what you didn't.

LLMs generate code faster than ever. DST catches the bugs they introduce. Together: **autonomous discovery**.

**The tools exist. The techniques are proven. Testing must evolve from prevention to discovery.**

---

# The Spectrum of Adoption

**Start anywhere. Each level adds value.**

| Level | What to do | What you get |
|-------|-----------|-------------|
| **1** | Property-based testing | Simplify testing boilerplate, explore user code paths |
| **2** | Fakes | Fast, deterministic tests without external dependencies |
| **3** | Fault-injectable fakes | Discover edge cases, simulate production failures |
| **4** | Seed-driven deterministic simulation | Reproducible bugs, autonomous discovery |

---

# Resources

**Blog posts:**
[Testing: Prevention vs Discovery](https://pierrezemb.fr/posts/testing-prevention-vs-discovery/) · [Simulation-Driven Development](https://pierrezemb.fr/posts/simulation-driven-development/) · [Diving into FDB Simulation](https://pierrezemb.fr/posts/diving-into-foundationdb-simulation/) · [Why Fakes Beat Mocks](https://pierrezemb.fr/posts/why-fakes-beat-mocks-and-test-containers/) · [Deterministic Simulation from Scratch (4 parts)](https://pierrezemb.fr/posts/deterministic-simulation-from-scratch-stage-1/) · [Simulating Leader Election on FDB](https://pierrezemb.fr/posts/simulating-leader-election-on-foundationdb/)

**Projects:**
[moonpool](https://github.com/PierreZ/moonpool) — DST framework + book · [foundationdb-rs](https://github.com/PierreZ/foundationdb-rs) — Rust FDB bindings + simulation workloads

**References:**
[Antithesis](https://antithesis.com/) · [TigerBeetle Safety](https://docs.tigerbeetle.com/concepts/safety/) · [Jepsen](https://jepsen.io/) · [SWE Book Ch. 13](https://abseil.io/resources/swe-book/html/ch13.html)

**Papers:** Yuan OSDI'14 · Alquraan OSDI'18 · Alagappan FAST'18 · Ganesan FAST'17 · TaxDC ASPLOS'16 · Huang OSDI'22 · SWE-CI 2026

---
layout: end
---

# Thank you!

**Testing: Prevention vs Discovery**

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · [pierrezemb.fr](https://pierrezemb.fr)

Questions?
