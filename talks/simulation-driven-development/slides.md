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

On-call survivor, a.k.a. the **"black cat"** 🐈‍⬛

Sunny Tech

---

# $ whoami 👋

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🛠️ Staff engineer, I build and operate **distributed systems**
- 🐈‍⬛ On-call **"black cat"**: the weird incidents always find me
- 🦀 Maintainer of OSS Rust libraries ([foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs))
- 🤝 I help the local **FinistDevs** / JUG
- 🎾 Squash player (the sport with walls, not the git command)

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-40 h-40">
  🖼️ photo of Pierre (/pierre.png)
</div>
</div>

---

# One of my most "wait what" bugs 🤯

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🌐 A network partition hits an **~70-node HDFS cluster**
- 🐢 The cluster cannot agree on reality, it gets **stuck on its back**
- 🔁 In doubt, **reboot**. Except it will not come back up
- ☕ It greets us with a **`NullPointerException` at startup**
- 🩹 Known bug, patched upstream: we **backported** it, recompiled, and did a rolling redeploy on 70 machines 😱

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

- 🚗 **Prod is not Dev**. Dev is the written exam, prod is driving through Paris at rush hour
- 🌍 The code never modeled what **the world** actually does: partitions, recovery, pressure
- 🌙 I learned this in the fire of on-call, at **3am, alone**

---

# How do you teach 3am? 🧑‍🏫

- 🧑‍🏫 I now have **juniors and apprentices** on my team
- 🩹 On-call gave me production intuition. Those are **scars**, learned the hard way
- 🤔 I cannot just hand them the pager and say "good luck"
- ❓ The real question under everything: **will their code break production?**

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

<div class="ecom">

| Dimension | Options | Count |
|---|---|---|
| 👤 User type | Guest, Logged-in, Premium | 3 |
| 💳 Payment | Card, PayPal, Apple Pay, Gift Card | 4 |
| 🚚 Delivery | Standard, Express, Pickup | 3 |
| 🏷️ Promotion | Yes, No | 2 |
| 📦 Inventory | In stock, Low, Out | 3 |
| 💱 Currency | USD, EUR, GBP | 3 |

</div>

<div class="text-center mt-6 text-lg font-mono">3 x 4 x 3 x 2 x 3 x 3 = <span class="font-bold" style="color: var(--theme-accent);">648</span> happy-path test cases 😱</div>

<style>
.ecom table { font-size: 0.72rem; width: auto; margin: 0 auto; }
.ecom td, .ecom th { padding: 2px 16px; }
</style>

---

# Add a few features... 😱

<div class="ecom">

| Dimension | New option | Count |
|---|---|---|
| 👤 User type | + Business | 4 |
| 💳 Payment | + Bank Transfer | 5 |
| 🚚 Delivery | + Same-Day | 4 |
| 🏷️ Promotion | + Expired promo | 3 |
| 📦 Inventory | + Preorder | 4 |
| 💱 Currency | + JPY | 4 |

</div>

<div class="text-center mt-6 text-lg font-mono">4 x 5 x 4 x 3 x 4 x 4 = <span class="font-bold" style="color: var(--theme-accent);">3,840</span> test cases 💀</div>

One option each, and manual testing **cannot keep up** with feature velocity.

<style>
.ecom table { font-size: 0.72rem; width: auto; margin: 0 auto; }
.ecom td, .ecom th { padding: 2px 16px; }
</style>

---

# You can't test what you don't know 🙈

- 🧠 You only write tests for the cases you **already imagined**
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

Now let's torture the **other** side. 😈

---

# Failure is the normal case 🏭

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

> A **2000-machine** service means **more than 10 machine crashes every day**.

*John Wilkes, QCon London 2015*

- 💀 Disks, RAM, kernel and cluster upgrades, OOM kills
- 📊 At scale, failure is not an event, it is the **day-to-day job**

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-56 h-40">
  🖼️ datacenter aisle photo (/datacenter.png)
</div>
</div>

---

# "The network is reliable" 🌐

- 🤥 The **first fallacy** of distributed computing
- 🧪 Every microservice app **is** a distributed system: it talks over the network
- 🎓 So academia went and **studied what really happens**

---

# Network partitions are catastrophic 💀

*[An Analysis of Network-Partitioning Failures in Cloud Systems](https://www.usenix.org/conference/osdi18/presentation/alquraan), Alquraan et al., OSDI 2018*

<div class="grid grid-cols-[1fr_auto] gap-8 items-center mt-4">
<div>

> **80%** of studied failures had a **catastrophic** impact.
> **27%** caused **permanent data loss**.

Real bug reports from HBase, ZooKeeper, Kafka, and more.

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-56 h-40">
  🖼️ OSDI '18 paper tables 1 and 2 (/osdi-tables.png)
</div>
</div>

---

# ...and you won't even see it 🙈

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

> **90%** were **silent** (no logs, no errors).
> **21%** left the system **permanently broken**, even after the network healed.
> **83%** needed **3 or more events** to trigger.

🔁 Permanently broken until you reboot it. Sound familiar? (Hi, Hadoop. 👋)

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-52 h-40">
  🖼️ "I just rolled a new network config" meme (/network-config-meme.png)
</div>
</div>

---

# Your microservices are a distributed system 🕸️

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🧩 Front-end, back-end, and database: **state in all three**, coordinating over a network
- 🔪 "We replaced our monolith with microservices so every outage is a **murder mystery**"
- 📺 cite Bryan Cantrill, *Debugging Under Fire*

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-56 h-44">
  🖼️ "murder mystery microservices" tweet (/murder-mystery.png)
</div>
</div>

---
layout: two-cols
---

::title::

# SRE vs SWE 🌉

::default::

**🛟 The on-call engineer (SRE)**

- 📦 Treats your service as a **black box**
- 🌪️ Absorbs whatever prod throws: bad users, dead machines, partitions
- 🌙 Has to fix it at **3am**, alone

::right::

**🧑‍💻 The developer (SWE)**

- 🚀 Pushed to **ship the feature**
- ⏳ Rarely given time to write tests
- 🧪 Tests only **what they thought to test**

---

# And now an LLM writes the code 🤖

More code, faster, with the **same blind spots**:

> We said "good enough" because we wrote it, we understood it, we tried it. AI broke all three.

*Steve Klabnik, "Steel, Rust, and truth", BugBash 2026*

- 🏢 Amazon's Gen-AI changes, **"high blast radius"** incidents (FT, 2026)
- 📉 *SWE-CI (Chen et al., 2026): most models hold a zero-regression rate **below 0.25***

---

# Juniors and LLMs: the same question ❓

A junior's code and an LLM's code both fail Klabnik's three grounds **in the production sense**:

- ✍️ we wrote it, but we did not feel it break
- 🧠 we understood it, but not what prod does to it
- 🧪 we tried it, but only on the happy path

Neither has the scars. Both ask one thing: **will it break production?**

We need a way to find out, **before production does**.

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

- ⚡ **Fast and debuggable**
- 🌐 **Entire system** at once
- 🪨 **Robust** (no `sleep(5)` flakiness)

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

A property reads like a **spec**, not a test case.

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

Same idea, applied to **integration**, not just units.

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
- 🧱 ...and finds a bug where **Mario clips through a wall** (Will Wilson)

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
- 🔐 Writing a database is like crypto: a thousand ways to mess up
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

Deterministically cut the network. Reversing the close order finds **more** bugs 🤷.

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

Kill up to 10 machines, keep at least 3. The system must still **make progress**.

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

The Kubernetes-volume nightmare, on purpose.

---

# ...all at once, concurrently 🌀

```toml
[[test.workload]]
testName = 'Cycle'

[[test.workload]]
testName = 'RandomClogging'

[[test.workload]]
testName = 'Attrition'
```

All of it happens **concurrently**, driven by one seed.

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
- ☁️ **30 min of sim is about 24h** of absolute chaos
- 🐞 A faulty seed? **Replay it locally**, deterministically
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
4. 🔧 the loop fixes it

> You do not trust Claude. You trust the simulator. It finds what you never thought to test.

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-52 h-44">
  🖼️ feedback-loop / boris-cherny tweet (/feedback-loop.png)
</div>
</div>

---

# Claude, finding bugs under simulation 🔬

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🔎 On **moonpool**, Claude found a bug I did not know existed, replayed the seed, and fixed it, alone
- 🦀 It built the **foundationdb-rs** binding tester
- 🗳️ It implemented **leader election** with machine-checkable invariants under partitions, crashes, and clock skew

</div>
<div class="grid place-items-center border-2 border-dashed rounded-lg opacity-50 text-xs w-52 h-44">
  🖼️ Claude fixing moonpool screenshot (/claude-moonpool.png)
</div>
</div>

---

# Same loop, juniors and AI alike 🧑‍🏫🤖

- 🧑‍🏫 The feedback loop works for **juniors and agents** alike: both **discover what they don't know**
- ❓ DST answers **"will it break production?"** before production does
- 🔬 Klabnik closed his keynote with this:

> You've been taking this seriously for sixty years. [...] Every programmer is about to need what you already know.

---

# Not a silver bullet 🙅

- 📈 **Performance is invisible** in sim: you still need a perf farm
- 🗺️ Your **model can be wrong**: sim only tests the world you described
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

# Remember the NPE? 🔁

- 🌐 The partition, the recovery, the `NullPointerException` at startup
- 🎲 That exact combination is just a **seed** now
- ⚡ Found in **seconds**, fixed **before prod**, **no 3am page**

That is the whole point.

---
layout: end
---

# Thank you, questions? 🎁

Come say hi at the **Clever Cloud** booth 👋

[The companion blog post](https://pierrezemb.fr/posts/simulation-driven-development/) · [Why fakes beat mocks and testcontainers](https://pierrezemb.fr/posts/why-fakes-beat-mocks-and-testcontainers/)

[BugBash 2026: the correctness decade](https://pierrezemb.fr/posts/bugbash-2026/) · [Learn about DST](https://pierrezemb.fr/posts/learn-about-dst/)

[moonpool](https://github.com/PierreZ/moonpool) · [pierrezemb.fr](https://pierrezemb.fr) · @PierreZ
