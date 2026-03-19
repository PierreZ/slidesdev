---
theme: slidev-theme-pz
title: "Testing: Prevention vs Discovery"
---

# Testing: Prevention vs Discovery 🔬
## rethinking testing in the age of LLMs 🤔

Pierre Zemb — Clever Cloud

---

# $ whoami 👋

- Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷
- Former ISEN student
- Specialized in distributed systems
  - Building,
  - contributing,
  - operating,
- OSS maintainer
- Squash player


---

# $ (also) whoami 👋

TODO black cat

---

# One of my most "wait what" bugs 🤯

**Story: Network partition then NullPointerException on restart**

1. Normal day, then: network partition + disk full on journal nodes (multiple cascading failures)
2. 70-machine Hadoop cluster goes belly-up
3. Can't self-heal, reboot, **NullPointerException at startup**
4. Bug was known. Patched in newer HDFS version.
5. Emergency: backport patch, recompile, redeploy on critical 70 machines cluster

**The question:** Why does a NullPointerException happen during *recovery*?

---
layout: two-cols
---

::title::

# 🤖 LLMs generate code faster than ever

**More code, faster. Same blind spots. More potential bugs.**

::default::

*Amazon, March 2026*

> "Trend of incidents" with "high blast radius" and "Gen-AI assisted changes"
> — Amazon internal briefing, reported by the Financial Times

Junior and mid-level engineers now require senior sign-off for any AI-assisted changes.

::right::

*SWE-CI — Chen et al., 2026*

> "Most models achieve a zero-regression rate below 0.25"

> "Current LLMs still struggle to sustain code quality over extended evolution, particularly in controlling regressions."

---

# Dev vs Prod 🚗

**Learning to drive ≠ Driving in Paris**

- 📝 Dev = passing the theory exam
- 🏎️ Prod = driving in Paris rush hour
- Recovery paths, split brain, cascading failures — none of this exists on localhost

How can we cultivate a production-oriented culture without assigning everyone a pager?

---

# Your system interacts with two things

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">👤 Your Users</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl">🌍 The World</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

- **Users**: unpredictable behavior, edge cases, adversarial inputs
- **World**: machines die, networks partition, disks corrupt, clocks drift, dependencies fail

---

# Can we improve user-related code? 🤔

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your Users</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🌍 The World</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Let's test our users 😱

Let's imagine the following e-commerce API that supports:

- 👤 User Type: Guest, Logged-in, Premium
- 💳 Payment Method: Credit Card, PayPal, Apple Pay, Gift Card
- 🚚 Delivery Option: Standard, Express, In-Store Pickup
- 🎁 Promotion Applied: Yes, No
- 📦 Inventory Status: In Stock, Low Stock, Out of Stock
- 💱 Currency: USD, EUR, GBP

To cover all possibilities, we need to write **3 × 4 × 3 × 2 × 3 × 3 = 648 unique test cases** 😱

(just to cover the happy path)

---

# This is why E2E testing is hard 🤯

Let's imagine the following e-commerce API that supports:

- 👤 User Type: Guest, Logged-in, Premium, **Business**
- 💳 Payment Method: Credit Card, PayPal, Apple Pay, Gift Card, **Bank Transfer**
- 🚚 Delivery Option: Standard, Express, In-Store Pickup, **Same-Day**
- 🎁 Promotion Applied: Yes, No, **Expired Promo**
- 📦 Inventory Status: In Stock, Low Stock, Out of Stock, **Preorder**
- 💱 Currency: USD, EUR, GBP, **JPY**

To cover all possibilities, we need to write **4 × 5 × 4 × 3 × 4 × 4 = 3,840 unique test cases** 😱

(just to cover the happy path)

---

# We can't test everything 🤯

- Every new feature or edge case **multiplies** the complexity
- Manual or naive testing strategies can't keep up
- Bugs hide in rare, high-dimensional combos
- You may spend days writing tests for a relatively small feature
- **We cannot know how our software will be used!**

> "You test what you imagine. Bugs hide in combinations you didn't."

---

# Can our code handle the world? 🌍

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">👤 Your Users</div>
        <div class="text-2xl opacity-40">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The World</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

--- 

# Can our code handle the world? 🌍

<div class="flex justify-center mt-6">
  <div class="flex flex-col items-center gap-4">
    <div class="flex gap-6 flex-wrap justify-center">
      <div class="px-4 py-2 border-2 border-current rounded text-sm">🌐 Network</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm">💾 Storage</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm">🕰️ Time</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm">🔐 Auth</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm">⚙️ APIs</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm">🗄️ Databases</div>
    </div>
    <div class="text-2xl">↕</div>
    <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🖥️ Your Application</div>
  </div>
</div>

Everything around your app can fail, lie, or slow down.

---

# "The network is reliable" 🌐

*[An Analysis of Network-Partitioning Failures](https://www.usenix.org/system/files/osdi18-alquraan.pdf) — Alquraan et al., OSDI 2018*

- **80%** of partition failures have catastrophic impact
- **27%** lead to data loss
- **90%** are silent — no log, no alert, nothing 🤫
- **21%** cause permanent damage that persists after the partition heals
- **83%** need 3+ events to manifest — **the sequential luck problem**

---

# "Your disks are reliable" 💾

- Silent corruption
  - 0.031% of SSDs per year
  - 1.4% of enterprise HDDs per year

- Misdirected I/O
  - 0.023% of SSDs per year
  - 0.466% of Nearline HDDs per year

Sources: [*SSD Reliability*](https://www.usenix.org/system/files/fast20-maneas.pdf) FAST 2020 · [*Data Corruption in the Storage Stack*](https://www.usenix.org/legacy/events/fast08/tech/full_papers/bairavasundaram/bairavasundaram.pdf) FAST 2008

---

# "fsync is reliable" 💾

*[Can Applications Recover from fsync Failures?](https://www.usenix.org/system/files/atc20-rebello.pdf) — Rebello et al., ATC 2020*

> "Although applications use many failure-handling strategies, none are sufficient: fsync failures can cause catastrophic outcomes such as data loss and corruption."

> "All three file systems mark pages clean after fsync fails, rendering techniques such as application-level retry ineffective."

Tested on PostgreSQL, LMDB, LevelDB, SQLite, Redis — **none handle fsync failures correctly**.

---

# "Your disks are reliable" — in practice 💾

*[Protocol-Aware Recovery for Consensus-Based Storage](https://www.usenix.org/system/files/conference/fast18/fast18-alagappan.pdf) — Alagappan et al., FAST 2018*

> "ext4 silently returns corrupted data if the underlying device block is corrupted."

> "None of the current approaches correctly recover from storage faults."

> "All current approaches lead to safety violation (e.g., data loss), low availability, or both."

ZooKeeper's Truncate recovery: a corrupted node truncates its log, forms a majority with lagging nodes → **silent data loss**

---

# "Your replicas protect you" 🛡️

*[Redundancy Does Not Imply Fault Tolerance](https://www.usenix.org/system/files/conference/fast17/fast17-ganesan.pdf) — Ganesan et al., FAST 2017*

> "A single file-system fault can cause catastrophic outcomes such as data loss, corruption, and unavailability."

Tested on 8 systems: Redis, ZooKeeper, Cassandra, Kafka, MongoDB, RethinkDB, CockroachDB, LogCabin.

**Kafka:** one corrupted log entry on the leader → leader ignores it, instructs followers to do the same → followers hit fatal assertion → **entire cluster unavailable + data loss** 💥

All problems observed at R=1 **persist at R=3**.

---

# "Your error handling catches it" ⚠️

*[Simple Testing Can Prevent Most Critical Failures](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf) — Yuan et al., OSDI 2014*

> "Almost all catastrophic failures (**92%**) are the result of incorrect handling of non-fatal errors explicitly signaled in software."

> "**35%** of the catastrophic failures are caused by trivial mistakes in error handling logic — ones that simply violate best programming practices."

> "A majority of the production failures (**77%**) can be reproduced by a unit test."

---

# "Your error handling catches it" — concurrency bugs ⚠️

*[TaxDC: A Taxonomy of Non-Deterministic Concurrency Bugs](https://ucare.cs.uchicago.edu/pdf/asplos16-TaxDC.pdf) — Leesatapornwongsa et al., ASPLOS 2016*

> "More than three quarters of the bugs involve some background protocols" — not the foreground protocols developers typically test.

> "DC bugs are triggered mostly by untimely messages (**64%**) and sometimes by untimely faults/reboots (**32%**), and occasionally by a combination of both."

---

# "Your retries save you" 🔄

*[Metastable Failures in the Wild](https://www.usenix.org/system/files/osdi22-huang-lexiang.pdf) — Huang et al., OSDI 2022*

> "The sustaining effect keeps the system in the metastable failure state even after the trigger is removed."

> "The most common sustaining effect is due to the retry policy, affecting more than **50%** of the studied incidents."

> "It naturally arises from the optimizations for the common case that lead to sustained work amplification."

22 incidents from 11 organizations — outages: **1.5 to 73.5 hours** ⏰ — at least **4 of 15 major AWS outages** in the last decade.

---

# "Your database does what the docs say" 📖

*[Jepsen: MariaDB Galera Cluster 12.1.2](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2)*

MariaDB claims: "Galera's tx_isolation is between Serializable and Repeatable Read."

Jepsen found — even in **healthy clusters** with zero faults:
- 💀 **Lost committed transactions**
- 💀 **Lost Updates**
- 💀 **Stale Reads**

> "It appears weaker than Read Uncommitted."

---

# The Thesis 📜

**Passing tests don't mean your system is reliable.**
**They mean your system can handle the scenarios you imagined.**

What about the ones you didn't?

> "The very reason tests are needed — humans can't enumerate all control flow branches — is what makes it impossible for humans to write comprehensive tests."

> — Will Wilson


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
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your Users,<br/>but worse</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The World,<br/>but worse</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

If your system survives this, it's **overqualified for production** 🦸

---

# How to simulate our users? 🤔

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulated Users</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🌍 The World</div>
        <div class="text-2xl opacity-40">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---
layout: two-cols
---

::title::

# Properties, not test cases 🧪

::default::

```java
// prevention: one hardcoded case
assertFalse(new User(GUEST).canUseSavedCards());
```

- Tests **one specific scenario**
- You write one test per case
- Breaks if requirements change

::right::

```java
// discovery: holds for ALL inputs
assertEquals(
  user.isAuthenticated(),
  user.canUseSavedCards()
);
```

Properties look like specs. They compile as code. They work for ALL random inputs.

---

# Property-based testing 🧪

- 🐍 Python: [Hypothesis](https://hypothesis.readthedocs.io/en/latest/)
- ☕ Java: [jqwik](https://jqwik.net/)
- 🦀 Rust: [proptest](https://docs.rs/proptest/latest/proptest/)
- λ Haskell: QuickCheck

**Instead of writing N tests, write a test GENERATOR.**

Add a feature? Add one enum value, not 100 tests. A test generator run long enough will eventually output all possible tests you could have written.

---

# Guided exploration beats brute force 🎮

[Antithesis](https://antithesis.com/) is the leader in autonomous testing — their platform uses **guided random exploration** to find bugs in any software.

**Demo:** they beat Super Mario Bros using only random inputs — no human, no scripting. Pure guided exploration. 🏆 See [Testing a Single-Node, Single Threaded, Distributed System Written in 1985](https://www.youtube.com/watch?v=m3HwXlQPCEU) by Will Wilson.

<img src="/mario.png" class="h-40 mx-auto" />

---

# How to simulate the world? 🌍

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">👤 Your Users</div>
        <div class="text-2xl opacity-40">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😈 Simulated World</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---

# Simulating the World 🌐😈

We need to inject chaos on our world:

- 🕰️ **Time**: delays, timeouts, retries, race conditions
- 🌐 **Network**: latency, failure, disconnection, partitions
- 💾 **Storage**: corruption, slow disks, full disks, misdirected I/O
- 🗄️ **Databases**: stale reads, lost updates, connection drops mid-transaction
- ⚙️ **External APIs**: timeouts, rate limits, partial failures
- 🔐 **DNS, Auth, Caches**: resolution failures, token expiry, thundering herd

To inject, you need to **control everything**.

---

# Fakes: Simulating the World 🎭

> "The system under test shouldn't even be able to tell whether it is interacting with a real implementation or a fake."

> — Software Engineering at Google, Ch. 13

A **fake** = working in-memory implementation. Stateful. Correct behavior. Injectable failures. Zero I/O. ⚡


---
layout: two-cols
---

::title::

# Fakes vs Mocks vs Testcontainers 🎭

::default::

🃏 **Mocks**

```java
when(repo.findById(1))
  .thenReturn(Optional.of(user));
```

- Verify **calls**, not state
- Break when you refactor
- Don't catch real bugs

::right::

✅ **Fakes**

```java
FakeUserRepo repo = new FakeUserRepo();
repo.save(user);
assertEquals(user, repo.findById(1));
```

- Verify **state**, not calls
- Survive refactoring
- Run in milliseconds

---

# 🐳 What about Testcontainers?

- Heavy, slow startup
- Non-deterministic (network, timing)
- Can't inject **specific** faults (corrupt this block, delay that packet)
- Great for integration smoke tests, **not for discovery**

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
layout: two-cols
---

::title::

# Choose your boundary — examples 🔌

::default::

**🗄️ Database** — you control everything

Swap OS primitives:
- 📡 TCP → in-memory channels
- 💾 ext4 → HashMap + fault injection
- 🕰️ System clock → simulated clock

Same binary, different I/O backends.

::right::

**☁️ Control plane** — you don't own the data service

Swap your service layer:
- 🗄️ `DataService` → `FakeDataService`
- 📡 `QueuePublisher` → `FakeQueue`
- 🔐 `AuthProvider` → `FakeAuth`

Same app code, fake dependencies.

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

# Simulating all the things 🎯

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="flex gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulated Users</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😈 Simulated World</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

---
layout: two-cols
---

::title::

# DST: The ultimate LLM feedback loop 🤖🔁

::default::

> "Probably the most important thing to get great results out of Claude Code — give Claude a way to verify its work. If Claude has that feedback loop, it will 2-3x the quality of the final result."
> — [Boris Cherny, creator of Claude Code](https://x.com/bcherny/status/2007179861115511237)

**DST IS that feedback loop.**

All autonomously. No human in the loop.

::right::

<div class="flex justify-center">
  <div class="flex flex-col items-center gap-1 text-sm">
    <div class="px-4 py-2 border-2 border-current rounded">🤖 LLM writes code</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🧪 Simulation finds bug</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🔍 LLM reads failing seed</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🔧 LLM fixes code</div>
    <div>↓</div>
    <div>🔁</div>
  </div>
</div>

---

# 🤖 Claude fixing software with DST

![Claude fixing moonpool using deterministic replay](https://pierrezemb.fr/images/testing-prevention-vs-discovery/claude-moonpool.png)

---

# Clever Cloud / Materia ☁️🦀

- ~90 employees — a **full cloud provider** with our own LB, Linux distro, and orchestrator
- Already have our hands full — then we decided to build a **distributed multi-model database** 🤯
  - Team grew from 1 to 6 persons
- The question isn't *why* — it's **how do you build something this hard with a small team?**
- The answer: **simulation-driven development** — write the workload first, then implement
  - All our stack is under simulation: KV, KMS, ETCD, workflow engine, leader election...

> "We would never have succeeded without simulation." 🦸


---

<img src="/materia-sim-single.png" class="w-full" />

---

<img src="/materia-sim-triple.png" class="w-full" />

---

# foundationdb-rs: Discovery at scale 🦀

- **~15 million downloads**, solo maintainer
- Used by Apache OpenDAL, SurrealDB, and others
- ~130 unsafe blocks requiring careful FFI management

**BindingTester**: randomized, seeded operations that explore the API surface continuously 🔄

- Compares Rust implementation against reference implementations
- Runs **hourly on GitHub Actions** — ~219 days of continuous exploration per month
- Tests across multiple OS, FDB versions, Rust compiler versions

---

# Simulation is not a silver bullet ⚠️

- 📊 **Performance is invisible** — you still need a perf/bench farm
- 🧩 **Your model can be wrong** — if you don't simulate it, you won't find it
- 🏭 **Production fakes need chaos too** — your production implementations need their own fault injection
- 🎲 **Rare bugs need smart exploration** — brute force isn't enough
- ⏰ **Bug-finding latency** — a bug can hide in a seed for months

---

# Testing must evolve 🧬

Remember the HDFS incident? Network partition + disk full + restart = NullPointerException.

That exact combination? **It's a seed in a simulation.** Found in seconds. Fixed before production. **No 3am wake-up call.** 😴

LLMs generate code faster than ever. DST catches the bugs they introduce. Together: **autonomous discovery**.

**The tools exist. The techniques are proven. Testing must evolve from prevention to discovery.**

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
layout: end
---

# Thank you! 🙏

**Testing: Prevention vs Discovery**

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · [pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬
