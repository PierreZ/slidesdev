---
theme: slidev-theme-pz
title: "Testing: Prevention vs Discovery"
---

# Testing: Prevention vs Discovery 🔬

Pierre Zemb — Clever Cloud

---

# $ whoami 👋

- Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷
- Distributed systems — dev + on-call
- Open source: foundationdb-rs, moonpool 🦀
- "Le chat noir" 🐈‍⬛ — always on-call when the weird stuff happens

---

# One of my most "wait what" bugs 🤯

**A pretty tough network partition on a 70+ machine Apache Hadoop cluster**

1. Network partition + disk full on journal nodes 💥
2. 70-machine cluster goes belly-up
3. Can't self-heal, reboot... **NullPointerException at startup**

---

# The HDFS Incident — aftermath

4. Bug was known. Patched in newer HDFS version.
5. Emergency: backport patch, recompile, redeploy on 70 machines at 3am 🔥

**The question:** Why does a NullPointerException happen during *recovery*?

Rolling out an untested patch on a distributed system under fire is an... unpleasant experience.

---

# 🤖 LLMs generate code faster than ever

**But who catches the bugs they introduce?**

*Amazon, March 2026*

> "Trend of incidents" with "high blast radius" and "Gen-AI assisted changes"
> — Amazon internal briefing, reported by the Financial Times

Junior and mid-level engineers now require senior sign-off for any AI-assisted changes.

---

# 🤖 LLMs still struggle with regressions

*SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases — Chen et al., 2026*

> "Most models achieve a zero-regression rate below 0.25"

> "Current LLMs still struggle to sustain code quality over extended evolution, particularly in controlling regressions."

**More code, faster. Same blind spots. More potential bugs.** 😬

---

# Dev vs Prod 🚗

**Learning to drive ≠ Driving in Paris**

- 📝 Dev = passing the theory exam
- 🏎️ Prod = driving in Paris rush hour
- Recovery paths, split brain, cascading failures — none of this exists on localhost

How can we cultivate a production-oriented culture without assigning everyone a pager?

---

# Your system 🖥️

<div class="flex justify-center mt-12">
  <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">Your System</div>
</div>

---

# Your system interacts with two things

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">👤 Your Users</div>
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">🌍 The World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Can we improve user-related code? 🤔

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your Users</div>
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🌍 The World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Let's test our users 😱

**E-commerce API example:**

| Dimension | Values |
|-----------|--------|
| 👤 User type | Guest, Registered, Premium, Business |
| 💳 Payment | Credit Card, PayPal, Bank Transfer |
| 🚚 Shipping | Standard, Express, Pickup |
| 🎁 Promotion | None, Percentage, Fixed |
| 📦 Inventory | In stock, Low stock, Backorder |
| 💱 Currency | EUR, USD, GBP |

**648** test combinations just for the happy path 😱

---

# This is why E2E testing is hard 🤯

- Add one option per dimension → **4,000+ tests**
- Real project: 300 lines of feature code, **10,000 lines of tests**, 1.5 months of work
- We cannot test what we don't know

> "You test what you imagine. Bugs hide in combinations you didn't."

Tests can't assure bug absence — they mainly check for regressions.

---

# Can our code handle the world? 🌍

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">👤 Your Users</div>
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# "Your replicas protect you" 🛡️

*Redundancy Does Not Imply Fault Tolerance — Ganesan et al., FAST 2017*

> "A single file-system fault can cause catastrophic outcomes such as data loss, corruption, and unavailability"

Across 8 production systems: Redis, ZooKeeper, Cassandra, Kafka, MongoDB, RethinkDB, CockroachDB, LogCabin.

---

# "Your replicas protect you" — Kafka 💥

One corrupted log entry on the leader →

- Leader ignores it
- Instructs followers to do the same
- Followers hit fatal assertion
- **Entire cluster unavailable + data loss**

All problems observed at R=1 **persist at R=3**.

---

# "Your disks are reliable" 💾

*Disks Lie — TigerBeetle Safety Docs*

- 0.031% of SSDs per year: silent corruption
- 1.4% of enterprise HDDs per year: silent corruption
- Misdirected I/O: measurable rates across all disk types

---

# "Your disks are reliable" — ZooKeeper 💾

- Recovered in only **2%** of disk corruption cases
- 30% of block corruption → data loss or unavailability

*Protocol-Aware Recovery — Alagappan et al., FAST 2018*

- ext4 silently returns corrupted data to applications
- ZooKeeper's Truncate recovery: **causes silent data loss**

> "Very few papers address disk failures."

---

# "You'd notice a network partition" 🌐

*An Analysis of Network-Partitioning Failures — Alquraan et al., OSDI 2018*

- **80%** of partition failures have catastrophic impact
- **27%** lead to data loss
- **90%** are silent — no log, no alert, nothing 🤫

---

# "You'd notice a network partition" — it gets worse 🌐

- **21%** cause permanent damage that persists after the partition heals
- **83%** need 3+ events to manifest — **the sequential luck problem**

---

# "Your error handling catches it" ⚠️

*Simple Testing Can Prevent Most Critical Failures — Yuan et al., OSDI 2014*

- **92%** of catastrophic failures = incorrect handling of non-fatal errors
- **35%** of error handlers are empty, have a log, or `TODO`/`FIXME`
- **77%** reproducible by a unit test — **if someone had thought to write it**

---

# "Your error handling catches it" — concurrency bugs ⚠️

*TaxDC — Leesatapornwongsa et al., ASPLOS 2016*

> "More than three quarters of the bugs involve background protocols"

Not the user-facing foreground protocols developers typically test.

Triggered by: untimely message delivery, untimely faults or reboots, or combinations of both.

---

# "Your retries save you" 🔄

*Metastable Failures in the Wild — Huang et al., OSDI 2022*

The system enters a bad state that **persists even after the trigger is removed**.

> "Retries sustain more than 50% of metastable incidents"

---

# "Your retries save you" — the numbers 🔄

> "Changes meant to improve the common case — fast paths, caches, retries, failover, load balancing, autoscaling — all make the failure state less resource-efficient"

- 22 incidents from 11 organizations
- Outages: **1.5 to 73.5 hours** ⏰
- Universally observed from hyperscalers to small companies

**Your retry logic, your circuit breakers, your autoscaling — they can sustain the failure.**

---

# The Thesis 📜

**Passing tests don't mean your system is reliable.**
**They mean your system can handle the scenarios you imagined.**

What about the ones you didn't?

---

# The scenarios that page you at 3am 📟

The machine that dies halfway through a write. The retry storm nobody reproduced in CI. The network partition at exactly the wrong moment.

**No test was written for them.**

> "The very reason tests are needed — humans can't enumerate all control flow branches — is what makes it impossible for humans to write comprehensive tests."
> — Will Wilson

---

# Prevention vs Discovery ⚖️

|  | Prevention | Discovery |
|--|-----------|-----------|
| **Question** | "Did we break what used to work?" | "What else is broken?" |
| **Method** | Regression tests, CI, code review | Simulation, fault injection, PBT |
| **Finds** | Known bugs returning | Unknown bugs lurking |
| **Scales** | Linearly with effort | Multiplicatively with compute |
| **Coverage** | What you imagined | What nobody imagined |

The industry does 99% prevention, ~0% discovery.

---
layout: section
---

# ACT 2: THE SOLUTION 🛠️

---

# What developers want from tests ✅

- ⚡ **Fast** — not 10 min CI, not waiting for containers
- 🔍 **Debuggable** — not "works on my machine"
- 🏗️ **Full system** — not just isolated units
- 💪 **Robust** — no flaky tests, no `sleep()` in tests

> Who has written a `sleep()` in a test? 🙋

---

# We must test the worst 🦹

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your Users,<br/>but worse</div>
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The World,<br/>but worse</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

If your system survives this, it's **overqualified for production** 🦸

---

# How to simulate our users? 🤔

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulated Users</div>
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🌍 The World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Simulating users: randomized input 🎲

```java
enum UserType { GUEST, LOGGED_IN, PREMIUM, BUSINESS }
enum PaymentMethod { CARD, PAYPAL, APPLE_PAY, GIFT_CARD, BANK_TRANSFER }

// …
Random rand = new Random(); // random seed
UserType user = pickRandom(rand, UserType.values());
PaymentMethod payment = pickRandom(rand, PaymentMethod.values());
```

---

# Simulating users: checking the state ✅

```java
// move from hardcoded test case (prevention)
assertFalse(new User(GUEST).canUse(SAVED_CARD));

// to a test property (discovery)
assertEquals(user.isAuthenticated(), user.canUse(SAVED_CARD));
```

Properties look like specs. They compile as code. They work for ALL random inputs.

---

# Yep, it's "just" property-based testing 🧪

- 🐍 Python: [Hypothesis](https://hypothesis.readthedocs.io/en/latest/)
- ☕ Java: [jqwik](https://jqwik.net/)
- 🦀 Rust: [proptest](https://docs.rs/proptest/latest/proptest/)
- λ Haskell: QuickCheck

**Instead of writing N tests, write a test GENERATOR.**

Add a feature? Add one enum value, not 100 tests.

---

# From tests to test generators 🏭

```java
// Instead of 648 individual test cases...
enum UserType { GUEST, REGISTERED, PREMIUM, BUSINESS }
enum PaymentMethod { CREDIT_CARD, PAYPAL, BANK_TRANSFER }

// ...write one generator
UserType user = randomEnum(UserType.class);
PaymentMethod payment = randomEnum(PaymentMethod.class);
// ... run the scenario
```

A test generator run long enough will eventually output all possible tests you could have written.

---

# Guided exploration beats brute force 🎮

**Pure random inputs don't work.**

Random button presses on Super Mario Bros: Mario dies in the first 5 seconds. Every time. 💀

**Guided exploration beats the game.** 🏆

Smart randomness — stateful, correlated, guided — finds bugs that brute force never will.

---

# 🎮 Antithesis beating Super Mario Bros

<div class="flex justify-center items-center mt-12">
  <div class="px-12 py-8 border-2 border-dashed border-current rounded-lg text-center opacity-70">
    <p class="text-xl mb-2">🎬 VIDEO</p>
    <p>Antithesis beating Super Mario Bros with guided random exploration</p>
  </div>
</div>

---

# How to simulate the world? 🌍

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">👤 Your Users</div>
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😈 Simulated World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Simulating the World 🌐😈

We need to inject chaos on our world:

- 🕰️ **Time**: delays, timeouts, retries, race conditions
- 🌐 **Network**: latency, failure, disconnection, partitions
- 💾 **Storage**: corruption, slow disks, full disks, misdirected I/O
- ⚙️ **External APIs**: timeouts, rate limits, partial failures
- 📊 **Load**: 10 users vs 10,000 users

To inject, you need to **control everything**.

---

# Fakes: Simulating the World 🎭

> "The system under test shouldn't even be able to tell whether it is interacting with a real implementation or a fake."
> — Software Engineering at Google, Ch. 13

A **fake** = working in-memory implementation. Stateful. Correct behavior. Injectable failures. Zero I/O. ⚡

---

# Fakes vs Mocks vs Testcontainers 🎭

- 🃏 **Mocks** leak implementation details — 5 lines of `when().thenReturn()` that break when you refactor
- 🐳 **Testcontainers** are heavy, slow, non-deterministic, can't inject specific faults
- ✅ **Fakes** verify state, not calls. They survive refactoring. They run in milliseconds.

---

# Fakes are easier than you think 🗺️

Google's SWE Book: `FakeFileSystem` backed by a `HashMap<String, String>`. That's it.

- 📁 File system? **A HashMap.**
- ☁️ S3? **A HashMap.**
- 🌐 DNS? **A HashMap.**
- 🗄️ Database? **A HashMap.**
- 📡 Network? **A buffer.**

> A fake with 80% of features is better than no fake at all.

---

# Choose your boundary 🎯

| What you control | Fake boundary |
|---|---|
| **External deps** (you don't own PostgreSQL) | Your service layer — `UserRepository`, `S3Client` |
| **Everything** (all code in simulation) | OS primitives — network, disk I/O, clock |

Don't fake PostgreSQL. Fake your access to it.

---

# Model real failures, not documented ones 📖

*Jepsen: MariaDB Galera Cluster 12.1.2*

MariaDB claims "no lost transactions" and "between Serializable and Repeatable Read."

Jepsen found — even in **healthy clusters** with zero faults:

- 💀 **Lost committed transactions**
- 💀 **Lost Updates**
- 💀 **Stale Reads**

**Weaker than Read Uncommitted.** Your fake should simulate what your DB *actually does*.

---

# Generate enough work 💪

- Write workloads that combine operations — full workflows, not just CRUD
- Generate valid AND invalid inputs
- Combine adversarial users with a hostile world — at scale

**Process vs Workload separation:**

- 🖥️ **Process** = system under test (crashes, reboots, loses state)
- 📋 **Workload** = test driver (never crashes, judges correctness)

---

# Teach a computer to test 🤖

**Give a computer a test, it finds a bug. Teach a computer to test, it finds bugs forever.**

<div class="flex justify-center mt-6">
  <div class="flex items-center gap-4 text-lg">
    <div class="px-4 py-2 border-2 border-current rounded">🧪 Properties</div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">🎲 Randomness</div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">💪 Enough work</div>
    <span class="text-2xl">=</span>
    <div class="px-6 py-3 border-2 rounded font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">⚙️ DST</div>
  </div>
</div>

---

# Making it deterministic 🔧

**Three sources of non-determinism to eliminate:**

1. 🧵 **Thread scheduling** → single-threaded execution
2. 💾 **Real I/O** (network, disk, time) → simulated I/O
3. 🎲 **Random number generation** → seeded PRNG

```
u64 seed → entire execution determined
Same seed = same decisions = same bugs. Every time.
```

A failing seed is a time machine ⏪ — rewind, inspect, experiment.

---

# The Provider Pattern 🏗️

**Abstract I/O behind traits, implement twice:**

<div class="flex justify-center mt-4">
  <div class="border-2 border-current rounded-lg overflow-hidden w-4/5">
    <div class="px-4 py-2 border-b-2 border-current text-center font-bold">🖥️ Your Application (same code, always)</div>
    <div class="px-4 py-2 border-b-2 border-current text-center text-sm">Provider Traits: TimeProvider · NetworkProvider · StorageProvider · RandomProvider</div>
    <div class="grid grid-cols-2">
      <div class="px-4 py-3 border-r-2 border-current">
        <div class="font-bold mb-1">🏭 Production</div>
        <div class="text-sm">Real OS · Real TCP · Real fs · OS RNG</div>
      </div>
      <div class="px-4 py-3">
        <div class="font-bold mb-1">🧪 Simulation</div>
        <div class="text-sm">Event queue · In-memory network · In-memory fs · Seeded ChaCha8</div>
      </div>
    </div>
  </div>
</div>

Same code runs in both — no `#[cfg(test)]`, no conditional compilation.

---

# Simulating all the things 🎯

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulated Users</div>
      <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😈 Simulated World</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# DST: The ultimate LLM feedback loop 🤖🔁

> "The most important thing to get great results out of Claude Code — give Claude a way to verify its work."
> — Boris Cherny, creator of Claude Code

**DST IS that feedback loop.**

<div class="flex justify-center mt-4">
  <div class="flex items-center gap-3 text-base">
    <div class="px-3 py-2 border-2 border-current rounded">🤖 LLM writes code</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">🧪 Simulation finds bug</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">🔍 LLM reads seed</div>
    <span>→</span>
    <div class="px-3 py-2 border-2 border-current rounded">🔧 LLM fixes</div>
    <span>→ 🔁</span>
  </div>
</div>

---

# 🤖 Claude fixing moonpool with DST

<div class="flex justify-center items-center mt-12">
  <div class="px-12 py-8 border-2 border-dashed border-current rounded-lg text-center opacity-70">
    <p class="text-xl mb-2">📸 SCREENSHOT</p>
    <p>Claude fixing moonpool using deterministic replay</p>
  </div>
</div>

---
layout: section
---

# ACT 3: IN PRACTICE 🚀

---

# Who does deterministic simulation? 🌍

**This isn't theoretical.**

| Project | What | Scale |
|---------|------|-------|
| **FoundationDB** 🍎 | The pioneer. Simulator before database. | Trillions of CPU-hours |
| **TigerBeetle** 🐯 | Financial transactions DB | VOPR on 1024 cores |
| **Antithesis** 🔬 | Deterministic hypervisor | Bugs in 2-3 weeks |
| **Dropbox** 📦 | Sync engine | Full simulation |
| **WarpStream** 🌊 | Kafka-compatible streaming | Full SaaS under DST |
| **Clever Cloud** ☁️ | Materia distributed DB | Team of 5 🦀 |

---

# Clever Cloud / Materia ☁️🦀

- 80 employees, **team of 5** (including 2 junior apprentices!)
- Building a **distributed multi-model multi-tenant database** on FoundationDB
- Only team in the world running Rust code inside FDB's simulation 🚀
- **Simulation-driven development**: write the workload first, then implement

---

# Clever Cloud / Materia — discoveries 🔍

**Concrete discovery:** simulation found that certain index types failed when a `delete all` operation passed through — an edge case nobody imagined 💡

**On-prem business case:** Clever Cloud deploys on-prem where they don't control hardware quality. Simulation ensures software works on degraded networks and disks.

> "We would never have succeeded without simulation." 🦸

---

# Materia simulation runs 🖥️

<div class="flex justify-center items-center mt-12">
  <div class="px-12 py-8 border-2 border-dashed border-current rounded-lg text-center opacity-70">
    <p class="text-xl mb-2">📸 SCREENSHOTS</p>
    <p>Materia simulation runs — workloads, fault injection, results</p>
  </div>
</div>

---

# foundationdb-rs: Discovery at scale 🦀

- **~15 million downloads**, solo maintainer
- Used by Apache OpenDAL, SurrealDB, and others
- ~130 unsafe blocks requiring careful FFI management

**BindingTester**: randomized, seeded operations that explore the API surface continuously 🔄

- Runs **hourly on GitHub Actions** — ~219 days of continuous exploration per month
- Tests across multiple OS, FDB versions, Rust compiler versions

---

# Leader Election: LLM + Simulation 🤖⚡

- Claude Code implemented a full ballot-based leader election on FoundationDB. **Autonomously.**
- It generated 13 machine-checkable invariants to verify its own work ✅
- Simulation threw network partitions, process crashes, and clock skew 💥
- **The LLM proposes, simulation disposes.**

---

# Simulation is not a silver bullet ⚠️

- 📊 **Performance is invisible** — you still need a perf/bench farm
- 🧩 **Your model can be wrong** — if you don't simulate it, you won't find it
- 🎲 **Rare bugs need smart exploration** — brute force isn't enough
- ⏰ **Bug-finding latency** — a bug can hide in a seed for months

---
layout: section
---

# CONCLUSION 🏁

---

# Testing must evolve 🧬

Remember the HDFS incident? Network partition + disk full + restart = NullPointerException.

That exact combination? **It's a seed in a simulation.** Found in seconds. Fixed before production. **No 3am wake-up call.** 😴

Your tests check what you imagined. Simulation discovers what you didn't.

---

# The spectrum of adoption 📈

**Start anywhere. Each level adds value.**

| Level | What to do | What you get |
|-------|-----------|-------------|
| **1** 🧪 | Property-based testing | Explore user code paths |
| **2** 🎭 | Fakes | Fast, deterministic tests |
| **3** 😈 | Fault-injectable fakes | Discover edge cases |
| **4** ⚙️ | Seed-driven DST | Reproducible bugs, autonomous discovery |

---

# Resources 📚

**Blog posts:**
[Testing: Prevention vs Discovery](https://pierrezemb.fr/posts/testing-prevention-vs-discovery/) · [Simulation-Driven Development](https://pierrezemb.fr/posts/simulation-driven-development/) · [Diving into FDB Simulation](https://pierrezemb.fr/posts/diving-into-foundationdb-simulation/) · [Why Fakes Beat Mocks](https://pierrezemb.fr/posts/why-fakes-beat-mocks-and-test-containers/) · [Deterministic Simulation from Scratch (4 parts)](https://pierrezemb.fr/posts/deterministic-simulation-from-scratch-stage-1/) · [Simulating Leader Election on FDB](https://pierrezemb.fr/posts/simulating-leader-election-on-foundationdb/)

**Projects:**
[moonpool](https://github.com/PierreZ/moonpool) · [foundationdb-rs](https://github.com/PierreZ/foundationdb-rs)

**References:**
[Antithesis](https://antithesis.com/) · [TigerBeetle Safety](https://docs.tigerbeetle.com/concepts/safety/) · [Jepsen](https://jepsen.io/) · [SWE Book Ch. 13](https://abseil.io/resources/swe-book/html/ch13.html)

**Papers:** Yuan OSDI'14 · Alquraan OSDI'18 · Alagappan FAST'18 · Ganesan FAST'17 · TaxDC ASPLOS'16 · Huang OSDI'22 · SWE-CI 2026

---
layout: end
---

# Thank you! 🙏

**Testing: Prevention vs Discovery**

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · [pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬
