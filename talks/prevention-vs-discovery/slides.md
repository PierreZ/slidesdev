---
theme: slidev-theme-pz
title: "Testing: Prevention vs Discovery"
themeConfig:
  accent: '#ce3942'
---

<img src="/title.png" class="mx-auto h-96" />

---

<img src="/1774372533908.jpg" class="mx-auto h-96" />

---

<img src="/finistai-next.png" class="mx-auto h-96" />

---
layout: cover
---

# Testing: Prevention vs Discovery 🔬
## rethinking testing in the age of LLMs 🤔

Pierre Zemb — Clever Cloud

---

# $ whoami 👋

- Pierre Zemb — Staff Engineer @ [Clever Cloud](https://clever.cloud) 🇫🇷
- Former ISEN student
- Specialized in distributed systems
  - Building, contributing, operating...
- OSS maintainer
- Co-leading the FinistDevs
- Squash player

---

# $ (also) whoami 👋

<img src="/thisisfine.gif" class="mx-auto" />

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

# Dev vs Prod 🚗

**Learning to drive ≠ Driving in real-life**

- 📝 Dev = passing the theory exam
- 🏎️ Prod = driving in Paris rush hour
- Recovery paths, split brain, cascading failures — none of this exists on localhost
- And more importantly:
  - **how can we test any fix?**
  - How can we **cultivate a production-oriented culture** for developers?

---
layout: two-cols
---

::title::

# 🤖 LLMs generate code faster than ever

**More code, faster. Same blind spots. More potential bugs.**

::default::

[*Amazon holds engineering meeting following AI-related outages, March 2026*](https://www.ft.com/content/7cab4ec7-4712-4137-b602-119a44f771de)

> "Trend of incidents" with "high blast radius" and "Gen-AI assisted changes"
> — Amazon internal briefing, reported by the Financial Times

Junior and mid-level engineers now require senior sign-off for any AI-assisted changes.

::right::

[*SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases via Continuous Integration — Chen et al., 2026*](https://arxiv.org/pdf/2603.03823)

> "Most models achieve a zero-regression rate below 0.25"

> "Current LLMs still struggle to sustain code quality over extended evolution, particularly in controlling regressions."

---

# Cheap code, expensive what? 💡

> "Code is becoming like fast food. Cheap, fast, everywhere."
> — [João Alves](https://world.hey.com/joaoqalves/when-software-becomes-fast-food-23147c9b)

- 🧠 **Knowing what to build** — specs, design, architecture
- 🔍 **Knowing if it works** — testing, verification, observability
- ⚖️ **Knowing when it breaks** — failure modes, edge cases, regressions

LLMs **amplify expertise, they don't replace it.**

---

# Let's focus on quality 🔍

The goal isn't faster code. It's **better software**.

We can generate code. But how do we verify it actually works?

---

# Your system interacts with two things 🖥️

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

- You test what you imagine. Bugs hide in combinations you didn't.
- Manual or naive testing strategies can't keep up

**We need a way to explore!**

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

# What can go wrong? 🌍

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

You probably handle the happy path. What about the other **3,839 combinations**?

---

# What do you believe about your system? 🤔

---

# "The network is reliable" 🌐

*[An Analysis of Network-Partitioning Failures](https://www.usenix.org/system/files/osdi18-alquraan.pdf) — Alquraan et al., OSDI 2018*

- **80%** of partition failures have catastrophic impact
- **27%** lead to data loss
- **90%** are silent — no log, no alert, nothing 🤫
- **21%** cause permanent damage that persists after the partition heals
- **83%** need 3+ events to manifest — **the sequential luck problem**

---

# "My data is safe on disk" 💾

* [*SSD Reliability*](https://www.usenix.org/system/files/fast20-maneas.pdf) FAST 2020
* [*Data Corruption in the Storage Stack*](https://www.usenix.org/legacy/events/fast08/tech/full_papers/bairavasundaram/bairavasundaram.pdf) FAST 2008

- Silent corruption
  - 0.031% of SSDs per year
  - 1.4% of enterprise HDDs per year

- Misdirected I/O
  - 0.023% of SSDs per year
  - 0.466% of Nearline HDDs per year


---

# "I called fsync, my data is safe" 💾

*[Can Applications Recover from fsync Failures?](https://www.usenix.org/system/files/atc20-rebello.pdf) — Rebello et al., ATC 2020*

> "Although applications use many failure-handling strategies, none are sufficient: fsync failures can cause catastrophic outcomes such as data loss and corruption."

> "All three file systems mark pages clean after fsync fails, rendering techniques such as application-level retry ineffective."

Tested on PostgreSQL, LMDB, LevelDB, SQLite, Redis — **none handle fsync failures correctly**.

---

# "Consensus will recover from crashes" 💾

*[Protocol-Aware Recovery for Consensus-Based Storage](https://www.usenix.org/system/files/conference/fast18/fast18-alagappan.pdf) — Alagappan et al., FAST 2018*

The researchers injected **2,401 corruption scenarios** into ZooKeeper:

| Scenario | Result |
|---|---|
| Targeted corruptions | Recovers in **46/2,401** cases (1.9%) 💀 |
| Random block corruptions | **~30%** end in data loss or unavailability |
| Block errors | **~50%** cluster unavailability — restarting loops forever |

**Why?** ZooKeeper truncates corrupted log entries. If the corrupted node then forms a majority with lagging nodes → **committed data is silently lost**.

---

# "I have 3 replicas, I'm safe" 🛡️

*[Redundancy Does Not Imply Fault Tolerance](https://www.usenix.org/system/files/conference/fast17/fast17-ganesan.pdf) — Ganesan et al., FAST 2017*

> "A single file-system fault can cause catastrophic outcomes such as data loss, corruption, and unavailability."

Tested on 8 systems: Redis, ZooKeeper, Cassandra, Kafka, MongoDB, RethinkDB, CockroachDB, LogCabin.

**Kafka:** one corrupted log entry on the leader → leader ignores it, instructs followers to do the same → followers hit fatal assertion → **entire cluster unavailable + data loss** 💥

All problems observed at R=1 **persist at R=3**.

---

# "We handle all our errors" ⚠️

*[Simple Testing Can Prevent Most Critical Failures](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf) — Yuan et al., OSDI 2014*

> "Almost all catastrophic failures (**92%**) are the result of incorrect handling of non-fatal errors explicitly signaled in software."

> "**35%** of the catastrophic failures are caused by trivial mistakes in error handling logic — ones that simply violate best programming practices."

> "A majority of the production failures (**77%**) can be reproduced by a unit test."

---

# "We don't have concurrency bugs" ⚠️

*[TaxDC: A Taxonomy of Non-Deterministic Concurrency Bugs](https://ucare.cs.uchicago.edu/pdf/asplos16-TaxDC.pdf) — Leesatapornwongsa et al., ASPLOS 2016*

> "More than three quarters of the bugs involve some background protocols" — not the foreground protocols developers typically test.

> "DC bugs are triggered mostly by untimely messages (**64%**) and sometimes by untimely faults/reboots (**32%**), and occasionally by a combination of both."

---

# "We just retry on failure" 🔄

*[Metastable Failures in the Wild](https://www.usenix.org/system/files/osdi22-huang-lexiang.pdf) — Huang et al., OSDI 2022*

> "The sustaining effect keeps the system in the metastable failure state even after the trigger is removed."

> "The most common sustaining effect is due to the retry policy, affecting more than **50%** of the studied incidents."

> "It naturally arises from the optimizations for the common case that lead to sustained work amplification."

22 incidents from 11 organizations — outages: **1.5 to 73.5 hours** ⏰ — at least **4 of 15 major AWS outages** in the last decade.

---

# "We follow the documentation" 📖

*[Jepsen: MariaDB Galera Cluster 12.1.2](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2)*

MariaDB claims: "Galera's tx_isolation is between Serializable and Repeatable Read."

Jepsen found — even in **healthy clusters** with zero faults:
- 💀 **Lost committed transactions**
- 💀 **Lost Updates**
- 💀 **Stale Reads**

> "It appears weaker than Read Uncommitted."

---

# We need to explore 🚀

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-center gap-6">
    <div class="grid grid-cols-2 gap-16">
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl text-center w-full">👤 <span style="color: var(--theme-accent);">Discover</span> user bugs?</div>
        <div class="text-2xl">↕</div>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl text-center w-full">🌍 <span style="color: var(--theme-accent);">Discover</span> infra bugs?</div>
        <div class="text-2xl">↕</div>
      </div>
    </div>
    <div class="px-12 py-6 border-2 border-current rounded-lg font-bold text-2xl">🖥️ Your System</div>
  </div>
</div>

LLMs excel at prevention — give them a spec, they write tests. But discovery requires different infrastructure.

---

# What developers want from tests ✅

- ⚡ **Fast** — not 10 min CI, not waiting for containers
- 🔍 **Debuggable** — not "works on my machine"
- 🏗️ **Full system** — not just isolated units
- 💪 **Robust** — no flaky tests, no `sleep()` in tests

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

If your system survives this, we can have more confidence it will **survive production** 🦸

Let's tackle these **one at a time**.

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

# Remember our e-commerce API? 🛒

**3,840 test cases** — and that was just the happy path.

Writing them by hand? Not an option.

What if we could **generate** them instead?

---

# From tests to test generators 🎲

```java
// ✅ One generator = all combinations
enum UserType { GUEST, LOGGED_IN, PREMIUM, BUSINESS }
enum PaymentMethod { CREDIT_CARD, PAYPAL, APPLE_PAY, GIFT_CARD, BANK_TRANSFER }
// …

Random rand = new Random();
UserType user = pickRandom(rand, UserType.values());
PaymentMethod payment = pickRandom(rand, PaymentMethod.values());
// …
checkout(user, payment, shipping, promo, stock, currency);
```

Add a feature? Add one enum value, not 100 tests.

---

# Properties, not test cases 🧪

```java {4}
// Don't assert specific values — assert relationships
for (int i = 0; i < 1000; i++) {
  UserType user = pickRandom(rand, UserType.values());
  assertEquals(user.isAuthenticated(), user.canUseSavedCards());
}
```

Properties look like specs. They compile as code. They hold for **all** inputs.

---

# Property-based testing 🧪

- 🐍 Python: [Hypothesis](https://hypothesis.readthedocs.io/en/latest/)
- ☕ Java: [jqwik](https://jqwik.net/)
- 🦀 Rust: [proptest](https://docs.rs/proptest/latest/proptest/)
- λ Haskell: QuickCheck

**The recipe:**
- 🎲 **Randomize inputs** — let the computer explore combinations you'd never write by hand
- 🧪 **Validate properties** — assert relationships that must hold for all inputs

---

# How to simulate the world? 🌍

Replace real dependencies with **fakes** — lightweight, in-memory implementations that actually hold state.

<div class="flex justify-center mt-4">
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

# Step 1: Identify fallible parts of your code 🔌

<div class="flex justify-center mt-4">
  <div class="flex flex-col items-stretch w-[28rem]">
    <div class="px-4 py-2 border-2 border-current rounded-t-lg text-center text-sm font-bold">Your Business Logic</div>
    <div class="relative px-4 py-2 border-2 border-x-current border-b-current text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">✂️ Service Layer — UserRepository, S3Client, ...<span class="absolute -right-28 top-1/2 -translate-y-1/2 text-xs opacity-60">← fake here</span></div>
    <div class="px-4 py-2 border-2 border-x-current border-b-current text-center text-sm opacity-40">Client Libraries (JDBC, SDK, HTTP,...)</div>
    <div class="relative px-4 py-2 border-2 border-x-current border-b-current text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">✂️ OS primitives — network, disk, clock<span class="absolute -right-28 top-1/2 -translate-y-1/2 text-xs opacity-60">← fake here</span></div>
    <div class="px-4 py-2 border-2 border-x-current border-b-current rounded-b-lg text-center text-sm opacity-40">Hardware</div>
  </div>
</div>

Don't fake PostgreSQL. Fake your **access** to it.

---

# Step 2: Define the boundary 🎭

```java
interface UserRepository {
  void save(User user);
  Optional<User> findById(long id);
  List<User> findAll();
}
```

Your code depends on this interface, not on PostgreSQL.

---
layout: two-cols
---

::title::

# Two implementations, one interface

::default::

🗄️ **Production**

```java
class PostgresUserRepository
    implements UserRepository {
  void save(User user) {
    jdbc.execute(
      "INSERT INTO users ...", user);
  }
}
```

::right::

🎭 **Simulation**

```java
class FakeUserRepository
    implements UserRepository {
  Map<Long, User> store = new HashMap<>();
  void save(User user) {
    store.put(user.id(), user);
  }
}
```

Same interface. One talks to Postgres. One lives in memory. **Your system can't tell the difference.**

---

# Step 3: Now break everything 💥

```java {5-9}
class FakeUserRepository implements UserRepository {
  Map<Long, User> store = new HashMap<>();
  Random rand;

  void save(User user) {
    if (rand.nextFloat() < 0.5)
      throw new StorageException("Connection lost");
    store.put(user.id(), user);
  }
}
```

Same fake from Step 1. Now it fights back.

---

# Be worse than production 😈

Remember MariaDB Galera? Jepsen found **stale reads in healthy clusters**.

Your fake should be **worse**: 50% stale reads, not 0.1%.

If your system survives this, it **survives production**.

---

# What if we combined both? 🧩

- 🎭 Fakes that **control** the world
- 💥 Chaos that **injects failures** everywhere

Run it all at once, with random inputs, checking properties...

---

# The price of determinism 🔧

Sources of non-determinism you must eliminate:

- 🧵 **Thread scheduling** → single-threaded cooperative execution
- 🎲 **Random numbers** → seeded PRNG
- 🕰️ **System time** → simulated clock
- 📋 **HashMap iteration** → deterministic data structures
- 🌐 **I/O** → simulated through fakes

The payoff:

```
u64 seed → entire execution determined
Same seed = same bugs. Every time.
```

A failing seed is a time machine ⏪

---

# Teach a computer to test 🤖

**Give a computer a test, it finds a bug. Teach a computer to test, it finds bugs forever.**

<div class="flex justify-center mt-6">
  <div class="flex items-center gap-4 text-lg">
    <div class="px-4 py-2 border-2 border-current rounded">🧪 Properties</div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">🎲 Randomness</div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">🎯 Determinism</div>
    <span class="text-2xl">+</span>
    <div class="px-4 py-2 border-2 border-current rounded">💪 Enough work</div>
    <span class="text-2xl">=</span>
    <div class="px-6 py-3 border-2 rounded font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">⚙️ DST</div>
  </div>
</div>

**Deterministic Simulation Testing (DST):**
Simulated users + simulated world + seeded determinism = reproducible bug discovery at scale.

---
layout: two-cols
---

::title::

# DST: The ultimate LLM feedback loop 🤖🔁

::default::

<img src="/boris-cherny-tweet.png" class="rounded shadow" />

DST as **a feedback loop** works as well for:
* junior engineers
* AI

because it lets them **discover what they don't know**.

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

# FoundationDB's simulation config 🔧

<img src="/fdb-sim-config-full.png" class="w-full" />

---

<img src="/fdb-sim-config-cycle.png" class="w-full" />

---

<img src="/fdb-sim-config-clogging.png" class="w-full" />

---

<img src="/fdb-sim-config-attrition.png" class="w-full" />

---

<img src="/fdb-sim-config-attrition-code.png" class="w-full" />

---

<img src="/fdb-sim-config-concurrent.png" class="w-full" />

---

<img src="/materia-sim-single.png" class="w-full" />

---

<img src="/materia-sim-triple.png" class="w-full" />

---
layout: two-cols
---

::title::

# Simulation runs continuously 🔄

::default::

- 💻 Engineers run a **few seeds locally** during development
- 🔁 CI runs **more iterations** on every push
- ☁️ Cloud runs simulation **continuously**
- ⏱️ **30 minutes** of simulation = **24 hours** of chaos testing
- 🎲 Faulty seed found? **Replay it locally**, deterministically

::right::

<img src="/materia-sim-ci.png" class="rounded shadow" />

---

# Claude Code + Simulation in action 🤖🔬

**Claude Code autonomously implemented real components under simulation:**

- 🦀 **foundationdb-rs** — binding tester with ~219 days of continuous exploration/month
- 🗳️ **Leader election** — generated **13 machine-checkable invariants**, verified under network partitions, process crashes, and clock skew
- 🏗️ **Materia** — implemented workflow engine, index types, query support — and found deep bugs through simulation along the way
- 🔍 **moonpool** — Developing a distributed system simulation framework

---

# What is the state of autonomous testing? 🎮

[Antithesis](https://antithesis.com/) is the leader in autonomous testing — their platform uses **guided random exploration** to find bugs in any software.

**Demo:** they beat Super Mario Bros using only random inputs — no human, no scripting. Pure guided exploration. 🏆 See [Testing a Single-Node, Single Threaded, Distributed System Written in 1985](https://www.youtube.com/watch?v=m3HwXlQPCEU) by Will Wilson.

<img src="/mario.png" class="h-40 mx-auto" />

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

LLMs generate code faster than ever. DST catches the bugs they introduce. Together: **autonomous discovery** 🤖🧪

The feedback loop works for junior engineers and AI alike — it lets them discover what they don't know.

**The tools exist. The techniques are proven. Testing must evolve from prevention to discovery.**

---

# The spectrum of adoption 📈

**Start anywhere. Each level adds value.**

| Level | What to do | What you get |
|-------|-----------|-------------|
| **1** 🎲 | Random workload generation | Test unusual combinations |
| **2** 🧪 | Property-based testing | Flush out your system spec |
| **3** 🎭 | Fakes | Fast, deterministic tests |
| **4** 😈 | Fault-injectable fakes | Discover edge cases |
| **5** ⚙️ | Seed-driven DST | Reproducible bugs, autonomous discovery |


---
layout: end
---

# Thank you! 🙏

**Testing: Prevention vs Discovery**

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · [pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬
