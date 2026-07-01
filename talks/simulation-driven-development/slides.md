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

Pierre Zemb, Staff Engineer @ Clever Cloud

Sunny Tech 2026 🦩

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
<img src="https://pierrezemb.fr/images/myself.jpg" class="w-40 h-40 rounded-lg object-cover" />
</div>

---

# Also me

<div class="absolute inset-0 grid place-items-center">
  <img src="https://media1.tenor.com/m/MYZgsN2TDJAAAAAC/this-is.gif" class="rounded-lg max-h-72" />
</div>

---

# One of my most "wait what" bugs 🤯

- 🌐 A tough network partition hit a **70+ node Apache Hadoop cluster**
- 🐢 The cluster could not restart
- ☕ A **`NullPointerException` at startup**, caused by its faulty state
- 🩹 Patched in a newer HDFS: we **backported** the patch and redeployed the jar 😱
- ⏰ You hit the bug at the **worst moment**, during recovery
- 🔥 Rolling out an untested patch on a distributed system under fire is an **unpleasant** experience

---

# So, what went wrong? 🔍

- 🚗 **Production environments ≠ Development environments**
  - 📝 Like the knowledge test vs the driving test
- 🌍 The code didn't account for real-world complexity, like our "not-common" NPE
  - 👤 Users will do weird things
  - 🔧 Systems will be under pressure
  - 💥 And things will **break** if we're not ready

---

# It's 2026, and AI broke "good enough" 🤖

Before LLMs, we trusted **correctness** because we:

- 🧠 understood it
- ✍️ wrote it
- 🧪 tried it
- 📟 got paged by it

AI broke the first three. The only feedback left is the **worst** one: the pager.

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

| Dimension | Options | Count |
|---|---|---|
| 👤 User type | Guest, Logged-in, Premium, **Business** | 4 |
| 💳 Payment | Card, PayPal, Apple Pay, Gift Card, **Bank Transfer** | 5 |
| 🚚 Delivery | Standard, Express, Pickup, **Same-Day** | 4 |
| 🏷️ Promotion | Yes, No, **Expired promo** | 3 |
| 📦 Inventory | In stock, Low, Out, **Preorder** | 4 |
| 💱 Currency | USD, EUR, GBP, **JPY** | 4 |

<div class="text-center mt-6 text-lg font-mono">4 x 5 x 4 x 3 x 4 x 4 = <span class="font-bold" style="color: var(--theme-accent);">3,840</span> test cases 💀</div>

---

# You can't test what you don't know 🙈

- 🧠 Example-based tests only cover the cases you **already imagined**
- 🪤 The nasty bugs hide in the **combination you never tried**
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

Now the other side: **the world** 😈

---

# The world is worse than your tests 📚

You do not have to believe me. Believe the literature:

| You believe... | The research says otherwise |
|---|---|
| 🌐 "The network is reliable" | [Network-Partitioning Failures, OSDI '18](https://www.usenix.org/system/files/osdi18-alquraan.pdf) |
| 💾 "My data is safe on disk" | [Data Corruption in the Storage Stack, FAST '08](https://www.usenix.org/legacy/events/fast08/tech/full_papers/bairavasundaram/bairavasundaram.pdf) |
| 💾 "fsync means it is saved" | [Can Applications Recover from fsync Failures?, ATC '20](https://www.usenix.org/system/files/atc20-rebello.pdf) |
| 🤝 "Consensus recovers from crashes" | [Protocol-Aware Recovery, FAST '18](https://www.usenix.org/system/files/conference/fast18/fast18-alagappan.pdf) |
| 🛡️ "3 replicas means I am safe" | [Redundancy Does Not Imply Fault Tolerance, FAST '17](https://www.usenix.org/system/files/conference/fast17/fast17-ganesan.pdf) |
| ⚠️ "We handle all our errors" | [Simple Testing Can Prevent Most Critical Failures, OSDI '14](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf) |
| 🧵 "We have no concurrency bugs" | [TaxDC, ASPLOS '16](https://ucare.cs.uchicago.edu/pdf/asplos16-TaxDC.pdf) |
| 🔄 "We just retry on failure" | [Metastable Failures in the Wild, OSDI '22](https://www.usenix.org/system/files/osdi22-huang-lexiang.pdf) |
| 📖 "Our documentation must be right" | [Jepsen: MariaDB Galera](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2) |

---

# Let's test the worst 😈

<div class="grid grid-cols-[1fr_auto] gap-10 items-center">
<div>

- ✅ To ensure software **behaves correctly**
- 🔍 To surface **hidden edge cases**
- 🕳️ To uncover the **unknown unknowns**

</div>
<div class="flex flex-col items-center gap-4">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↕</div>
  <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 Your users, but worse</div>
  <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The world, but worse</div>
</div>
</div>

---

# Two ways to be the worst 😈

<div class="grid grid-cols-2 gap-12 mt-10 text-center">
  <div class="flex flex-col items-center gap-3">
    <div class="px-6 py-3 border-2 rounded-full font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 The worst users</div>
    <div class="text-3xl opacity-40">↓</div>
    <div class="px-6 py-3 border-2 border-current rounded-lg font-semibold">🎰 Random workloads</div>
  </div>
  <div class="flex flex-col items-center gap-3">
    <div class="px-6 py-3 border-2 rounded-full font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌍 The worst world</div>
    <div class="text-3xl opacity-40">↓</div>
    <div class="px-6 py-3 border-2 border-current rounded-lg font-semibold">🎭 Fakes</div>
  </div>
</div>

---

# Don't write tests, write a generator 🎰

```java
enum UserType { GUEST, LOGGED_IN, PREMIUM, BUSINESS }
enum PaymentMethod { CARD, PAYPAL, APPLE_PAY,
                     GIFT_CARD, BANK_TRANSFER }

Random rand = new Random(seed);

UserType user = pickRandom(rand, UserType.values());
PaymentMethod payment = pickRandom(rand, PaymentMethod.values());
// add a feature? add an enum value, not 648 tests
```

Iterate enough seeds and you **cover the cardinality**, for free.

---

# Don't write assertions, write properties ✅

```java
// from a hardcoded test case:
assertFalse(new User(GUEST).canUse(SAVED_CARD));

// to a property that holds for ANY generated input:
assertEquals(user.isAuthenticated(), user.canUse(SAVED_CARD));
```

* A property reads like a **spec**, not a test case. It holds for **any** generated input
  * (Yes it's like Property-based testing, but applied to integration tests)

---

# The worst world → fakes 🌪️

<div class="flex items-center justify-center gap-10 mb-6">
  <div class="px-8 py-8 border-2 border-current rounded-lg text-center font-semibold opacity-40">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↔</div>
  <div class="flex flex-col gap-4">
    <div class="px-6 py-2 border-2 border-current rounded-full text-center opacity-40">🤖 Simulated users</div>
    <div class="px-6 py-2 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🌪️ Simulated world</div>
  </div>
</div>

- 💥 The world breaks time, network, disk, infra, dependencies: to test the worst, you must **control all of it**
- 🎭 The lightest way in: a **fake**, an in-memory stand-in you can make as cruel as you want

---

# An app on a bus we don't own 📨

<div class="flex items-center justify-center mt-12">
  <div class="px-6 py-8 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🖥️ Order service<br/><span class="text-sm font-normal">our code, must behave</span></div>
  <div class="flex flex-col items-center px-4">
    <div class="text-sm font-bold" style="color: var(--theme-accent);">🎭 MessageBus</div>
    <div class="text-3xl" style="color: var(--theme-accent);">⇄</div>
    <div class="text-xs opacity-70 text-center">publish · poll<br/>fake here ✂️</div>
  </div>
  <div class="flex items-center gap-3 px-5 py-5 border-2 border-dashed border-current rounded-lg opacity-50">
    <div class="text-center text-sm">🔌 Kafka<br/>client</div>
    <div class="text-lg">→</div>
    <div class="text-center text-sm">📨 Kafka<br/>broker</div>
    <div class="text-lg">→</div>
    <div class="flex flex-col gap-1 text-sm text-left">
      <div>💳 Payment</div>
      <div>📦 Shipping</div>
    </div>
  </div>
</div>

We own the service. We don't control the **driver** or the **broker** past it. The seam between them is where our **fake** lives, an autonomous in-memory stand-in for the bus.

---

# A fake bus is just a list of messages 🎭

```java
interface MessageBus {
    Future<Void> publish(String topic, Message msg);
    List<Message> poll(String topic);
}

// production: talks to Kafka
class KafkaBus implements MessageBus { /* driver */ }

// simulation: a fake bus is just a list of messages
// held by a singleton
class FakeBus implements MessageBus {
    Map<String, List<Message>> queues = new HashMap<>();
}
```

Same interface, two implementations: your code **can't tell the difference**. Keep the fake honest with one **contract test** run against both.

---

# Make the fake fight back 😈

```java
class FakeBus implements MessageBus {
    Map<String, List<Message>> queues = new HashMap<>();
    Random rng;

    public Future<Void> publish(String topic, Message msg) {
        sleep(rng.nextInt(500));                            // random lag
        float r = rng.nextFloat();
        if (r < 0.15) return failed(new ConnectionLost());  // never happened
        queues.get(topic).add(msg);                         // committed...
        if (r < 0.30) return failed(new ConnectionLost());  // ...ack lost!
        return completed();
    }
}
```

**Your system should still behave correctly!**

---


# Be worse than production 😈


- 🩺 [Jepsen's MariaDB Galera Cluster 12.1.2 report](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2)

> “Data is consistent across all nodes at all times”

> Even in healthy clusters, MariaDB Galera Cluster exhibited Lost Update and Stale Reads

- 😈 So your fake should be **worse**: 50% stale reads, not 0.1%
- 🛡️ Survive the fake, and you **survive production**


---

# Bundle it: that's DST 🎲

<div class="flex items-center justify-center gap-10 mt-8">
  <div class="px-10 py-10 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl" style="color: var(--theme-accent);">↔</div>
  <div class="flex flex-col gap-4">
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎰 Worst users: random workloads</div>
    <div class="px-6 py-3 border-2 rounded-full text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎭 Worst world: fakes that fight back</div>
  </div>
</div>

Drive both from one **seed** on a deterministic loop, and every failure **replays exactly**.

That bundle is **Deterministic Simulation Testing**. The whole point is to **discover what you don't know**.

---

# Who actually does this? 🧬

- 🧬 A growing club, mostly **distributed databases**:
  - Clever Cloud, Antithesis, TigerBeetle
  - Turso, RisingWave, Dropbox
  - sled, Kafka KRaft, AWS

---

# How far can DST go? 🍄

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🤖 **Antithesis** does autonomous testing: it hunts bugs for you
- 🍄 From pure random input, it **beats Super Mario Bros (1985)**
- 🧱 ...and finds a bug where **Mario clips through a wall**

</div>
<img src="/mario.png" class="w-96 rounded shadow" />
</div>

*[Testing a Single-Node, Single Threaded, Distributed System Written in 1985](https://www.youtube.com/watch?v=m3HwXlQPCEU&t=8s) — Will Wilson*

---

# So we're building a database 🗄️

<div class="grid grid-cols-[1fr_auto] gap-8 items-center">
<div>

- 🏢 At **Clever Cloud** we build **Materia**, a multi-tenant, multi-model distributed database
  - KV, ETCD, KMS, workflows, leader election...
- 🎥 Built on FoundationDB, which has a simulation framework that we hacked:
  - [Borrowing FDB's simulator, BugBash 2026](https://www.youtube.com/watch?v=tTxY8IbT88A&t=330s)
  - [Distributed DBs with FDB & Rust, Sunny Tech 2024](https://www.youtube.com/watch?v=Q_8CRjf3M24&t=2234s)
  - [FoundationDB intro, Sunny Tech 2023](https://www.youtube.com/watch?v=cChMz4m8w5A&t=2s)

</div>
<img src="/materia.webp" class="w-96 rounded shadow" />
</div>

---


<img src="/materia-sim-single.png" class="w-full rounded shadow" />


---


<img src="/materia-sim-triple.png" class="w-full rounded shadow" />


---

# It found bugs everywhere 🔥


- 📊 query execution returning **wrong data**
- 🗺️ query planner picking the **wrong index**
- 💥 **corruption** during reindexing
- 👑 **dual leader election** under clock skew
- 🗜️ ETCD compaction **deleting live data**
- ...


---

# Simulation-driven development 🔁

<div class="grid grid-cols-[1fr_auto] gap-6 items-center">
<div>

- 💡 Simulation surfaces what you **don't know to look for**
- 🏗️ Now every layer we build is **simulation-first**: we default to **discovery**
  - 🔁 Run it in a **loop**: in CI, and on a cloud fleet
  - ☁️ **30 min of sim is about 24h** of chaos testing
  - 🐞 A faulty seed **replays locally**, deterministically
- 🦸 It feels like super-powers, because we **trust our software**

</div>
<img src="/materia-sim-ci.png" class="w-72 rounded shadow" />
</div>

---

# Not a silver bullet 🙅

- 📈 **Performance is invisible** in sim: you still need a perf farm
- 🕳️ **Verified fakes**: you must [ensure your fake matches the real thing](https://pierrezemb.fr/posts/designing-fakes-that-prove-correctness/)
- ⏳ **Bug-finding has latency**: a bug can hide in a seed for months

---

# You don't trust Claude, you trust the simulator 🔁

<div class="grid grid-cols-2 gap-8 place-items-center">
  <img src="/boris-cherny-tweet.png" class="rounded shadow w-full" />
  <img src="/claude-moonpool.png" class="rounded shadow w-full" />
</div>

---


# The age of correctness has started 🔬

Expressing correctness was a **niche craft** for decades: types, contracts, property-based testing, fuzzing, formal methods, simulation.

AI broke "good enough", so **all of them are about to grow**. Property-based testing already is:

<div class="grid place-items-center mt-2">
  <img src="https://pierrezemb.fr/images/bugbash-2026/property-based-testing.png" class="rounded-lg max-h-64" />
</div>

---

# Invest in correctness, now 🌅

- 🔭 Simulation is **one tool** in that rising tide, pick whichever fits you
- 🛡️ We have a **duty**: ship correct software, not crap
- ⏰ Don't wait for the 3am page, **invest in correctness today**

---

# How to adapt DST 📈

**Start anywhere. Each level adds value.**

| Level | What to do | What you get |
|---|---|---|
| **1** 🎰 | Random workload generation | Test unusual combinations |
| **2** ✅ | Property-based testing | Flush out your system spec |
| **3** 🎭 | Fakes | Fast, deterministic tests |
| **4** 😈 | Fault-injectable fakes | Discover edge cases |
| **5** 🎲 | Seed-driven DST | Reproducible bugs, autonomous discovery |

---
layout: end
---

# Thank you! 🙏

any questions?

Everything is on [pierrezemb.fr](https://pierrezemb.fr/) 📝

[My own DST in Rust 🦀](https://pierrezemb.fr)

---