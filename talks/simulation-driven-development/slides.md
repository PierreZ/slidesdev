---
theme: slidev-theme-pz
title: "What if we embraced simulation-driven development?"
layout: cover
themeConfig:
  accent: '#d63f49'
  secondary: '#901f26'
---

<!-- Cover background image goes here (dark "developer in chaos" art). -->

# What if we embraced<br>simulation-driven development? 🎲

Pierre Zemb, Distributed-systems Engineer @ Clever Cloud 🇫🇷

On-call survivor 🔥

Sunny Tech

---

# $ whoami 👋

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🛠️ Staff engineer, working around **distributed systems**
- 🐞 Building, contributing, debugging, **paging**…
- 🦀 Maintaining several OSS libraries in Rust ([foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs))
- 🤝 Involved in local communities (**FinistDevs** / JUG)
- 🎾 Squash player

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-40 h-40">
  🖼️ photo of Pierre (/pierre.png)
</div>
</div>

---

<div class="grid place-items-center h-full">
  <div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-sm w-[36rem] h-80">
    🖼️ "this is fine" gif (/this-is-fine.gif)
  </div>
</div>

---

# One of my most "wait what" bugs 🤯

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🌐 A tough network partition hit a **70+ node Apache Hadoop cluster**
- 🐢 The cluster could not restart
- ☕ A **`NullPointerException` at startup**, caused by its faulty state
- 🩹 Patched in a newer HDFS: we **backported** the patch and redeployed the jar 😱
- ⏰ You hit the bug at the **worst moment**, during recovery
- 🔥 Rolling out an untested patch on a distributed system under fire is an **unpleasant** experience

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-48 h-48">
  🖼️ "days since last NullPointerException" sign (/days-since-npe.png)
</div>
</div>

---

# So, what went wrong? 🔍

<div class="flex justify-center my-8">
  <div class="px-12 py-10 border-2 border-current rounded-lg text-center font-semibold text-lg">🖥️ Your system</div>
</div>

- 🚗 **Prod is not Dev**, like the knowledge test vs the driving test
- 🌍 The code never modeled real-world complexity: partitions, recovery, pressure
- 💥 Users do weird things, systems run under pressure, things **break** if we're not ready
- 🌙 And you debug it paged at **3am**, everything down

---

# It's 2026, and AI broke "good enough" 🤖

An **LLM** writes a lot of the code now. More code, faster, with the **same blind spots**.

> We said "good enough" because we wrote it, we understood it, we tried it. AI broke all three.

<div class="flex justify-center gap-4 mt-6">
  <div class="px-6 py-4 border-2 rounded-lg text-center line-through" style="border-color: var(--theme-accent); color: var(--theme-accent);">✍️ we wrote it</div>
  <div class="px-6 py-4 border-2 rounded-lg text-center line-through" style="border-color: var(--theme-accent); color: var(--theme-accent);">🧠 we understood it</div>
  <div class="px-6 py-4 border-2 rounded-lg text-center line-through" style="border-color: var(--theme-accent); color: var(--theme-accent);">🧪 we tried it</div>
</div>

We did not write it. We do not understand it. Sometimes we do not even try it.

⚙️ Writing code is nearly free now, so the bottleneck becomes **making it correct**. Suddenly, everybody cares.

---

# How do you trust code you didn't write? 🤝

<div class="flex items-center justify-center gap-3 mt-8 text-lg font-semibold">
  <span>🎯 correctness</span><span class="opacity-40">→</span>
  <span>🤝 confidence</span><span class="opacity-40">→</span>
  <span>🧠 understanding</span><span class="opacity-40">→</span>
  <span style="color: var(--theme-accent);">👀 behaviors</span>
</div>

> We start wanting correctness. We realize we need confidence. Confidence requires understanding. Understanding lives in behaviors.

And confidence is **not a checkmark**: no tool hands you "correct", you build it.

---

# Understanding lives in behaviors 👀

A behavior is just a **sequence of states**: what your system actually does. There are two ways to witness them:

<div class="flex justify-center gap-6 mt-6">
  <div class="px-6 py-4 border-2 border-current rounded-lg text-center w-64">🌙 <b>Operate it</b><div class="text-sm opacity-70 mt-1">on-call, paged at 3am (how I learned HDFS)</div></div>
  <div class="px-6 py-4 border-2 rounded-lg text-center w-64" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌪️ <b>Simulate it</b><div class="text-sm opacity-80 mt-1">controlled, no pager</div></div>
</div>

Both lead to **understanding**. Simulation gets you there **before production**, at the speed agents demand.

---

# Your code meets two things 🌍

<div class="flex items-center justify-center gap-10 mt-12">
  <div class="px-10 py-12 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl opacity-40">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-8 py-3 border-2 border-current rounded-full text-center">👤 Your users</div>
    <div class="px-8 py-3 border-2 border-current rounded-full text-center">🌍 The world</div>
  </div>
</div>

Two sources of chaos. Let's start with the **users**.

---

# Can we test our users? 🛒

<div class="flex items-center justify-center gap-10 mt-8">
  <div class="px-8 py-10 border-2 border-current rounded-lg text-center font-semibold opacity-40">🖥️ Your system</div>
  <div class="text-3xl opacity-30">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-8 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your users</div>
    <div class="px-8 py-3 border-2 border-current rounded-full text-center opacity-40">🌍 The world</div>
  </div>
</div>

An e-commerce API, grown feature by feature. How many paths must we cover?

---

# One checkout, so many paths 🛒

| Dimension | Options | Count |
|---|---|---|
| 👤 User type | Guest, Logged-in, Premium | 3 |
| 💳 Payment | Card, PayPal, Apple Pay, Gift Card | 4 |
| 🚚 Delivery | Standard, Express, Pickup | 3 |
| 🏷️ Promotion | Yes, No | 2 |
| 📦 Inventory | In stock, Low, Out | 3 |
| 💱 Currency | USD, EUR, GBP | 3 |

<div class="text-center mt-6 text-lg font-mono">3 x 4 x 3 x 2 x 3 x 3 = <span class="font-bold" style="color: var(--theme-accent);">648</span> happy-path test cases 😱</div>

---

# Add a few features... 😱

| Dimension | New option | Count |
|---|---|---|
| 👤 User type | + Business | 4 |
| 💳 Payment | + Bank Transfer | 5 |
| 🚚 Delivery | + Same-Day | 4 |
| 🏷️ Promotion | + Expired promo | 3 |
| 📦 Inventory | + Preorder | 4 |
| 💱 Currency | + JPY | 4 |

<div class="text-center mt-6 text-lg font-mono">4 x 5 x 4 x 3 x 4 x 4 = <span class="font-bold" style="color: var(--theme-accent);">3,840</span> test cases 💀</div>

One option each, and manual testing **cannot keep up** with feature velocity.

---

# You can't test what you don't know 🙈

- 🧠 Example-based tests only cover the cases you **already imagined**
- 🪤 The nasty bugs hide in the **combination you never tried** (that one currency with no FX rate)
- 🛡️ Tests are a **regression net, not a proof** that bugs are absent

---

# Can our code handle the world? 🌍

<div class="flex items-center justify-center gap-10 mt-12">
  <div class="px-8 py-10 border-2 border-current rounded-lg text-center font-semibold opacity-40">🖥️ Your system</div>
  <div class="text-3xl opacity-30">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-8 py-3 border-2 border-current rounded-full text-center opacity-40">👤 Your users</div>
    <div class="px-8 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The world</div>
  </div>
</div>

Now the other side: **the world**. 😈

---

# The world is worse than your tests 📚

You do not have to believe me. Believe the literature:

| You believe... | The research says otherwise |
|---|---|
| 🌐 "The network is reliable" | [Network-Partitioning Failures, OSDI '18](https://www.usenix.org/system/files/osdi18-alquraan.pdf) (80% catastrophic, 90% silent) |
| 💾 "My data is safe on disk" | [Data Corruption in the Storage Stack, FAST '08](https://www.usenix.org/legacy/events/fast08/tech/full_papers/bairavasundaram/bairavasundaram.pdf) |
| 💾 "fsync means it is saved" | [Can Applications Recover from fsync Failures?, ATC '20](https://www.usenix.org/system/files/atc20-rebello.pdf) |
| 🤝 "Consensus recovers from crashes" | [Protocol-Aware Recovery, FAST '18](https://www.usenix.org/system/files/conference/fast18/fast18-alagappan.pdf) |
| 🛡️ "3 replicas means I am safe" | [Redundancy Does Not Imply Fault Tolerance, FAST '17](https://www.usenix.org/system/files/conference/fast17/fast17-ganesan.pdf) |
| ⚠️ "We handle all our errors" | [Simple Testing Can Prevent Most Critical Failures, OSDI '14](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf) |
| 🧵 "We have no concurrency bugs" | [TaxDC, ASPLOS '16](https://ucare.cs.uchicago.edu/pdf/asplos16-TaxDC.pdf) |
| 🔄 "We just retry on failure" | [Metastable Failures in the Wild, OSDI '22](https://www.usenix.org/system/files/osdi22-huang-lexiang.pdf) (retries sustain >50% of incidents) |
| 📖 "We follow the documentation" | [Jepsen: MariaDB Galera](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2) (stale reads in healthy clusters) |

---

# Let's test the worst 😈

<div class="flex items-center justify-center gap-10 mt-10">
  <div class="px-8 py-10 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your users, but worse</div>
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The world, but worse</div>
  </div>
</div>

Surface the hidden edge cases. **Uncover the unknown unknowns.** If it survives this, it survives prod.

---

# What do we want from a test? ✅

| What we want | How |
|---|---|
| ⚡ Fast and debuggable | Single thread |
| 🌐 Entire system | Single binary |
| 🪨 Robust | Randomized |

---

# What do we want from a test? ✅

| What we want | How | The DST piece |
|---|---|---|
| ⚡ Fast and debuggable | Single thread | **Deterministic event loop** |
| 🌐 Entire system | Single binary | **Network simulation** |
| 🪨 Robust | Randomized | **Entropy injection** |

Put together, that is **Deterministic Simulation Testing**.

---

# Simulating all the things 🎲

<div class="flex items-center justify-center gap-10 mt-12">
  <div class="px-10 py-12 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🤖 Simulated users</div>
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌪️ Simulated world</div>
  </div>
</div>

Two jobs: **simulate the users**, **simulate the world**.

---

# Don't write tests, write a generator 🎰

<div class="grid grid-cols-[1fr_auto] gap-6 items-center">
<div>

```java {7-9}
enum UserType { GUEST, LOGGED_IN, PREMIUM, BUSINESS }
enum PaymentMethod { CARD, PAYPAL, APPLE_PAY,
                     GIFT_CARD, BANK_TRANSFER }

Random rand = new Random(seed);

UserType user = pickRandom(rand, UserType.values());
PaymentMethod payment =
    pickRandom(rand, PaymentMethod.values());
// add a feature? add an enum value, not 648 tests
```

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-44 h-44">
  🖼️ "write several tests vs write a generator" meme (/generator-meme.png)
</div>
</div>

Iterate enough seeds and you **cover the cardinality**, for free.

---

# Don't write assertions, write properties ✅

<div class="grid grid-cols-[1fr_auto] gap-6 items-center">
<div>

```java {5}
// from a hardcoded test case:
assertFalse(new User(GUEST).canUse(SAVED_CARD));

// to a property that holds for ANY generated input:
assertEquals(user.isAuthenticated(), user.canUse(SAVED_CARD));
```

A property reads like a **spec**, not a test case. It holds for **any** generated input, so it doubles as your safety net.

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-44 h-44">
  🖼️ reaction meme (/property-meme.png)
</div>
</div>

---

# It's "just" property-based testing 🧪

- 🐍 **Hypothesis** (Python)
- ☕ **jqwik** (Java)
- 🦀 **proptest** (Rust)
- λ **QuickCheck** (Haskell)

Same idea, applied to **integration**, not just units. It used to be for experts. **Now it's for everyone.**

---

# Simulating the world 🌪️

<div class="flex items-center justify-center gap-10 mb-6">
  <div class="px-8 py-8 border-2 border-current rounded-lg text-center font-semibold opacity-40">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↔</div>
  <div class="flex flex-col gap-4">
    <div class="px-6 py-2 border-2 border-current rounded-full text-center opacity-40">🤖 Simulated users</div>
    <div class="px-6 py-2 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌪️ Simulated world</div>
  </div>
</div>

- 💥 Break Time, Network, Infra, Dependencies, and simulate Load, so you must **control everything**
- 🧰 Three options: DST from day 1, discrete-event-sim libs ([madsim](https://github.com/madsim-rs/madsim), [turmoil](https://github.com/tokio-rs/turmoil), SimPy), or **fakes**

---

# Your testcontainers can't page you 🐳

CI spins up Kafka, Postgres, Redis in Docker. 4 minutes to start. All green. None can produce the partial failure that pages you at **3am**.

| | Mocks | Testcontainers | Fakes |
|---|---|---|---|
| **State** | scripted | real | in-memory |
| **Partial failures** | manual | impossible | built-in |
| **Speed** | µs | minutes | µs |
| **Deterministic** | yes | no | yes |

Testcontainers give you **up or down**. Real failures are **partial**.

---

# Write a fake instead 🎭

A **fake** is a lightweight, in-memory implementation that **holds state**.

Fakes are about **ownership**: what do you own, and where is the fallible boundary it leans on?

<div class="flex flex-col items-stretch w-[34rem] mx-auto mt-6 text-sm">
  <div class="px-4 py-2 border-2 border-current rounded-t-lg text-center">Your business logic</div>
  <div class="px-4 py-2 border-2 border-t-0 text-center font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">Your access trait (fake here)</div>
  <div class="px-4 py-2 border-2 border-t-0 border-current text-center opacity-50">Driver you don't own (JDBC, SDK, HTTP)</div>
  <div class="px-4 py-2 border-2 border-t-0 border-current rounded-b-lg text-center opacity-50">Network and disk</div>
</div>

**Don't fake PostgreSQL. Fake your access to it.**

---

# Same interface, two implementations 🔀

```java {6-9}
interface UserRepository {
    void save(User user);
    Optional<User> findById(long id);
}

// production: talks to Postgres
class PostgresUserRepository implements UserRepository { /* JDBC */ }

// simulation: lives in memory
class FakeUserRepository implements UserRepository {
    Map<Long, User> store = new HashMap<>();
}
```

Your system **can't tell the difference**. Keep the fake honest with a contract test run against both (a verified fake).

---

# Make the fake fight back 😈

```java {3-5}
public Optional<User> findById(long id) {
    float r = rng.nextFloat();
    if (r < 0.10) throw new ConnectionLost();        // loud
    if (r < 0.20) return Optional.empty();           // silent: write vanished
    if (r < 0.25) return sleepForever();             // silent: hangs forever
    if (r < 0.45) return stale.get(id);              // silent: stale read
    return Optional.ofNullable(store.get(id));
}
```

Loud failures are easy. The dangerous ones are **silent**. Your code thinks everything is fine.

---

# Be worse than production 😈

- 🩺 Jepsen found a **healthy** Galera cluster serving **stale reads**, with zero faults injected
- 😈 So your fake should be **worse**: 50% stale reads, not 0.1%
- 🛡️ Survive the fake, and you **survive production**

---

# Who actually does this? 🧬

- 🧬 A growing club, mostly **distributed databases**:
  - Clever Cloud, Antithesis, TigerBeetle, Turso, RisingWave, Dropbox, sled, Kafka KRaft, AWS
- 🏗️ Heavy users of sim and fakes: **AWS** (15+ years), **Google** Fauxmaster, **Microsoft** CrystalNet (50+ bugs), **Oxide** (500k+ lines of Rust)

---

# How far can DST go? 🍄

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🤖 **Antithesis** does autonomous testing: it hunts bugs for you
- 🍄 From pure random input, it **beats Super Mario Bros (1985)**
- 🧱 ...and finds a bug where **Mario clips through a wall**

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-56 h-44">
  🖼️ Super Mario / Antithesis screenshot (/mario.png)
</div>
</div>

---

# So we're building a database 🗄️

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🏢 Clever Cloud is a **hoster**: own machines, BGP, Linux distro, load balancer, VM orchestrator
- 🗄️ ...and now a **multi-tenant, multi-model distributed database**, thanks to DST
- 🔐 Writing a database is like crypto: a thousand ways to get it wrong
- 🦾 Team went from **1 to 5 + 2 apprentices**, shipping distributed-DB code

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-52 h-40">
  🖼️ Materia logo (/materia.png)
</div>
</div>

---

# A simulation test, end to end 🧪

```toml {1,2}
[[test]]
testTitle = 'Clogged'

[[test.workload]]
testName = 'Cycle'
transactionsPerSecond = 1000.0

[[test.workload]]
testName = 'RandomClogging'

[[test.workload]]
testName = 'Attrition'
machinesToKill = 10
machinesToLeave = 3
```

---

# A linked list under chaos 🔗

```toml {4-6}
[[test]]
testTitle = 'Clogged'

[[test.workload]]
testName = 'Cycle'
transactionsPerSecond = 1000.0

[[test.workload]]
testName = 'RandomClogging'

[[test.workload]]
testName = 'Attrition'
machinesToKill = 10
machinesToLeave = 3
```

Objects point in a ring, reshuffle the pointers every transaction. Integrity holds when the last still points to the first.

---

# Random clogging 🔌

```toml {8,9}
[[test]]
testTitle = 'Clogged'

[[test.workload]]
testName = 'Cycle'
transactionsPerSecond = 1000.0

[[test.workload]]
testName = 'RandomClogging'

[[test.workload]]
testName = 'Attrition'
machinesToKill = 10
machinesToLeave = 3
```

Deterministically cut the network. Reversing the close order surfaces **more** bugs.

---

# Attrition 💀

```toml {11-13}
[[test]]
testTitle = 'Clogged'

[[test.workload]]
testName = 'Cycle'
transactionsPerSecond = 1000.0

[[test.workload]]
testName = 'RandomClogging'

[[test.workload]]
testName = 'Attrition'
machinesToKill = 10
machinesToLeave = 3
```

Kill up to 10 machines, keep at least 3. The system must still **make progress**. All workloads run **concurrently**, driven by one seed.

---

# Swap the disks 🤯

```cpp
bool swap = killType == Reboot
    && BUGGIFY_WITH_PROB(0.75)
    && sameRack(a, b);

if (swap) {
    // two dead machines in the same rack:
    // each reboots with the OTHER's disks
    std::swap(a.folders, b.folders);
}
```

We reproduce the Kubernetes-volume nightmare on purpose.

---

# One datacenter, one seed 🎲

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

One seed drives the whole cluster and even flips feature flags (TSS). **6s real, about 393s simulated**: we fast-forward the idle time.

---

# Change the seed, change the world 🌍

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

A different seed gives six datacenters, TSS off, a different topology. The **same test**.

---

# Simulation-driven development 🔁

<div class="grid grid-cols-[1fr_auto] gap-6 items-center">
<div>

- 🔁 Run it in a **loop**: in CI, and on a cloud fleet
- ☁️ **30 min of sim is about 24h** of chaos testing
- 🐞 A faulty seed **replays locally**, deterministically
- 🦸 It feels like super-powers, because we **trust our software**

</div>
<img src="/materia-sim-ci.png" class="w-72 rounded shadow" />
</div>

---

# You don't trust Claude, you trust the simulator 🤖🔁

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

The loop closes the same way for an agent as for a human:

1. 🤖 the agent writes code
2. 🌪️ the simulator beats it up
3. 🎲 a faulty seed reproduces the bug
4. 🔧 the agent reads the seed and fixes it
5. 🔁 rerun the **whole** suite, so the fix adds no new bug

On **moonpool**, Claude found a bug I did not know existed and fixed it from the seed, alone. The same loop serves a **junior**, **me**, and an **agent** alike.

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-52 h-44">
  🖼️ Claude fixing moonpool screenshot (/claude-moonpool.png)
</div>
</div>

---

# Not a silver bullet 🙅

- 📈 **Performance is invisible** in sim: you still need a perf farm
- 🕳️ **Meaning leaks**: a proof still has a gap to reality. Sim tests the **model you described**, not the full world
- 🧱 **Hard to retrofit** onto a system not built for determinism
- ⏳ **Bug-finding has latency**: a bug can hide in a seed for months

---

# Where to start 📈

| Level | Tactic | What you gain |
|---|---|---|
| 1 | 🎲 Random workload generation | Test the combinations you forgot |
| 2 | 🧪 Property-based testing | Flush out the spec |
| 3 | 🎭 Fakes | Fast, deterministic tests |
| 4 | 😈 Fault-injectable fakes | Discover edge cases |
| 5 | ⚙️ Seed-driven DST | Reproducible bugs, autonomous discovery |

You can start at level 1 **on Monday**.

---

# Our moment to shine 🌅

- 🌊 The masses are flooding into correctness. Winning looks like the world **co-opting your thing**.
- 🔁 We used to have a problem: **nobody** cared about correctness. Now **everybody** does. A better problem.
- 🤝 They are hungry for what this community already knows. **It is our moment to teach.**

---

# Remember the NPE? 🔁

- 🌐 The partition, the recovery, the `NullPointerException` at startup
- 🎲 That exact combination is just a **seed** now
- ⚡ Found in **seconds**, fixed **before prod**, **no 3am page**

That is the whole point.

---
layout: end
---

# The correctness decade is here 🔬

The techniques are ready. The world is finally asking. Come build with us. 🎁

[The companion blog post](https://pierrezemb.fr/posts/simulation-driven-development/) · [Why fakes beat mocks and testcontainers](https://pierrezemb.fr/posts/why-fakes-beat-mocks-and-testcontainers/)

[BugBash 2026: the correctness decade](https://pierrezemb.fr/posts/bugbash-2026/) · [Learn about DST](https://pierrezemb.fr/posts/learn-about-dst/)

[moonpool](https://github.com/PierreZ/moonpool) · [pierrezemb.fr](https://pierrezemb.fr) · @PierreZ · Questions? 💬
