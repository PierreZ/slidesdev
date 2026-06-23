---
theme: slidev-theme-pz
title: "What if we embraced simulation-driven development?"
layout: cover
themeConfig:
  accent: '#d63f49'
  secondary: '#901f26'
---

# What if we embraced<br>simulation-driven development? 🎲

Pierre Zemb — Distributed-systems Engineer @ Clever Cloud 🇫🇷

On-call survivor, a.k.a. the **"black cat"** 🐈‍⬛

[pierrezemb.fr](https://pierrezemb.fr)

---

# The "black cat" of on-call 🐈‍⬛

- 🐈‍⬛ Whenever something **weird** breaks, somehow **I'm the one on call**
- 🌙 Not the huge outages — the *"I've never seen this alert before"* ones
- 🔥 I'm a **developer** who learned ops **the hard way**: in the fire of on-call
- 🧑‍🏫 Now I have juniors & apprentices — how do I pass on that intuition?

---

# A war story: Hadoop won't reboot 💥

- 🌐 A network blip hits a **~70-machine HDFS cluster**
- 🐢 Nodes can't agree on reality → the cluster ends up **stuck like a turtle on its back**
- 🔁 In doubt? **Reboot.** Except… it won't come back up
- ☕ And it greets us with a classic: **`NullPointerException` at startup**

---

# An NPE… during recovery? 🤔

- 🔎 Known bug, already **patched upstream** in newer HDFS
- 🛠️ So we recompiled and did a **rolling redeploy across 70 machines** 😱 (it worked — barely)
- 🤯 But why crash at the **worst possible moment** — *mid-recovery*, while the system is already broken?
- 💡 Because the code never modeled **what the world actually does**

---

# Dev is the driving test. Prod is Paris. 🚗

- 📝 Dev is passing the written exam
- 🚗 Prod is driving through **Paris at rush hour**
- 🌍 The code didn't account for the **complexity of the world**: splits, recoveries, pressure
- 😈 Things break in production — *especially* when you're not ready

---

# Your code meets two things 🌍

<div class="flex justify-center items-center gap-4 mt-10">
  <div class="px-5 py-6 border-2 border-current rounded-lg text-center w-44">
    <div class="text-3xl">👤</div>
    <div class="font-bold mt-2">Users</div>
    <div class="text-xs opacity-70 mt-1">doing weird things</div>
  </div>
  <div class="text-2xl opacity-50">↔</div>
  <div class="px-6 py-6 border-2 rounded-lg text-center w-48 font-bold text-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">
    <div class="text-3xl">🧩</div>
    <div class="mt-2">Your code</div>
  </div>
  <div class="text-2xl opacity-50">↔</div>
  <div class="px-5 py-6 border-2 border-current rounded-lg text-center w-44">
    <div class="text-3xl">🌍</div>
    <div class="font-bold mt-2">The world</div>
    <div class="text-xs opacity-70 mt-1">machines, network, k8s</div>
  </div>
</div>

Could I have caught that NPE in dev? And how do I harden the rest? 🤔

---

# "I'll just write more tests" 🧪

- 🗣️ The universal developer answer to *"how do I improve quality?"*
- ✅ Fair enough — let's start with the **users**
- 🛒 Imagine an **e-commerce API**, grown feature by feature over months

---

# An e-commerce API 🛒

<div class="grid grid-cols-3 gap-3 mt-8 text-center">
  <div class="px-4 py-3 border-2 border-current rounded-lg">👤 User type</div>
  <div class="px-4 py-3 border-2 border-current rounded-lg">💳 Payment</div>
  <div class="px-4 py-3 border-2 border-current rounded-lg">🚚 Delivery</div>
  <div class="px-4 py-3 border-2 border-current rounded-lg">🏷️ Promotion</div>
  <div class="px-4 py-3 border-2 border-current rounded-lg">📦 Inventory</div>
  <div class="px-4 py-3 border-2 border-current rounded-lg">💱 Currency</div>
</div>

Forget unit tests — how do we verify the **paths a user actually takes**? 🛤️

---

# One happy path = one test ✅

<div class="flex justify-center flex-wrap gap-2 mt-10 text-sm">
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">👤 guest</span>
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">💳 PayPal</span>
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">🚚 express</span>
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">🏷️ no promo</span>
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">📦 in-stock</span>
  <span class="px-3 py-2 border-2 rounded-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">💶 EUR</span>
</div>

**One** path validated. Now do every other combination… 😅

---

# Cover everything = 648 tests 😱

<div class="flex justify-center mt-10">
  <div class="text-2xl font-mono">
    3 <span class="opacity-50">×</span> 4 <span class="opacity-50">×</span> 3 <span class="opacity-50">×</span> 2 <span class="opacity-50">×</span> 3 <span class="opacity-50">×</span> 3 <span class="opacity-50">=</span> <span class="font-bold" style="color: var(--theme-accent);">648</span>
  </div>
</div>

- ✍️ **648 tests** just to check the happy paths — and that's *before* any failure case
- ➕ Add **one** feature everywhere → **~4000 tests** 💀
- 🐌 Naive/manual testing **cannot keep up** with feature velocity

---

# You can't test what you don't know 🙈

- 🧠 You only write tests for the parts you **already imagined**
- 🪤 The nasty bugs hide in the **combination you never tried** (that one currency, no FX rate…)
- 🛡️ So tests are a **regression net — not a proof of absence of bugs**

---

# The world breaks, constantly 🌍

<div class="flex justify-center items-center gap-4 mt-10">
  <div class="px-5 py-6 border-2 border-current rounded-lg text-center w-44 opacity-40">
    <div class="text-3xl">👤</div>
    <div class="font-bold mt-2">Users</div>
  </div>
  <div class="text-2xl opacity-30">↔</div>
  <div class="px-6 py-6 border-2 border-current rounded-lg text-center w-48 font-bold text-lg opacity-40">
    <div class="text-3xl">🧩</div>
    <div class="mt-2">Your code</div>
  </div>
  <div class="text-2xl" style="color: var(--theme-accent);">↔</div>
  <div class="px-5 py-6 border-2 rounded-lg text-center w-44 font-bold text-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">
    <div class="text-3xl">🌍</div>
    <div class="mt-2">The world</div>
    <div class="text-xs opacity-80 mt-1">machines, network, k8s</div>
  </div>
</div>

Now let's torture the **other** side. 😈

---

# Failure is the normal case 📉

> A **2000-machine** service means **>10 machines crashing every single day**.

*Google production engineering keynote, ~2015*

- 💀 Disk failures, bad RAM, kernel & cluster upgrades, OOM kills…
- 📊 At scale, **failure isn't an event — it's the day-to-day job**

---

# "The network is reliable" 🌐

- 🤥 The **first fallacy** of distributed computing
- 🧪 Every microservice app **is** a distributed system — it talks over the network
- 🎓 So academia went and **studied what really happens**

---

# Network partitions are catastrophic 💀

*[An Analysis of Network-Partitioning Failures in Cloud Systems](https://www.usenix.org/conference/osdi18/presentation/alquraan) — Alquraan et al., OSDI 2018*

> **80%** of studied failures had a **catastrophic** impact.
> **27%** caused **permanent data loss**.

They studied real bug reports from HBase, ZooKeeper, Kafka, and more. 😱

---

# …and you won't even see it 🙈

> **90%** were **silent** — no logs, no errors.
> **21%** left the system **permanently broken**, even after the network healed.
> **83%** needed **≥3 events** to trigger.

🔁 Permanently broken until you reboot it… *sound familiar?* (Hi, Hadoop. 👋)

---
layout: two-cols
---

::title::

# Two worlds, one system 🌉

::default::

**🛟 The on-call engineer (SRE)**

- 📦 Treats your service as a **black box**
- 🌪️ Must absorb **whatever** prod throws: bad users, dead machines, splits
- 🌙 Has to fix it at **3am**, alone

::right::

**🧑‍💻 The developer**

- 🚀 Pushed to **ship the feature**
- ⏳ Rarely given time to write tests
- 🧪 Tests only **what they imagined**

---
layout: center
---

# What if dev were *worse* than prod? 😈

---

# What I want from a test ⚡

- 🏃 **Fast** — feedback in seconds, not 10 minutes
- 🐞 **Debuggable** — I debug enough in production already
- 🌐 **Whole-system** — complex distributed topologies, on my laptop
- 🪨 **Robust** — no `sleep(5)` flakiness that breaks on every change

---

# The recipe 🎲

- 🧵 **Single-threaded** + control **all I/O** ⇒ everything becomes **deterministic** (one event loop)
- 📦 **One self-contained binary** ⇒ spawn whole topologies in a single process
- 🎰 **Randomness** ⇒ inject entropy and **shake the system** (chaos testing)
- ✨ Put together, that's **deterministic simulation**

---

# Simulate both sides 🔁

<div class="flex justify-center items-center gap-4 mt-10">
  <div class="px-5 py-6 border-2 rounded-lg text-center w-44 font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">
    <div class="text-3xl">👤</div>
    <div class="mt-2">Users</div>
    <div class="text-xs opacity-80 mt-1">generated</div>
  </div>
  <div class="text-2xl" style="color: var(--theme-accent);">↔</div>
  <div class="px-6 py-6 border-2 border-current rounded-lg text-center w-48 font-bold text-lg">
    <div class="text-3xl">🧩</div>
    <div class="mt-2">Your code</div>
  </div>
  <div class="text-2xl" style="color: var(--theme-accent);">↔</div>
  <div class="px-5 py-6 border-2 rounded-lg text-center w-44 font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">
    <div class="text-3xl">🌍</div>
    <div class="mt-2">The world</div>
    <div class="text-xs opacity-80 mt-1">chaos-injected</div>
  </div>
</div>

Two jobs: **simulate the users**, **simulate the world**.

---

# Don't write tests — write a generator 🎰

```rust {2-6,9-10}
fn random_checkout(rng: &mut Rng) -> Checkout {
    let user     = rng.choice(&[Guest, Premium, Business]);
    let payment  = rng.choice(&[Card, PayPal, Wire, Crypto]);
    let delivery = rng.choice(&[Standard, Express, Pickup]);
    let promo    = rng.bool();
    let currency = rng.choice(&[EUR, USD, GBP]);
    // add a feature? add an enum arm — not 648 new tests
    Checkout { user, payment, delivery, promo, currency }
}
```

Iterate enough seeds and you **cover the cardinality** — for free. 🎲

---

# Don't write assertions — write properties ✅

```rust {1-4}
// Holds for ANY generated input, not one hand-picked case:
fn property(checkout: &Checkout, result: &Outcome) {
    if checkout.user.is_authenticated() {
        assert!(result.allows_saved_card());
    }
}
```

- 📜 A property reads like a **spec**, not a test case
- 🔬 This is **property-based testing** — [proptest](https://github.com/proptest-rs/proptest) · [Hypothesis](https://hypothesis.works/) · [fast-check](https://github.com/dubzzz/fast-check) · QuickCheck
- 🆙 Same idea, but applied to **integration**, not just units

---

# Simulate the world 🌪️

- ⏰ Break **time** · 🔌 break the **network** · 💀 break the **infra** · 📦 break your **dependencies** · 🏋️ simulate **load**
- 🎛️ The corollary: you must **control the entire system**
- 🎯 You decide that the **42nd network call takes 500ms** — because the seed said so
- 🧵 …including scheduling of every async task

---

# No determinism? Inject chaos into mocks 🎭

```rust {3-6}
async fn get(&self, url: &str) -> Result<Response> {
    let r = self.rng.f64();
    if r < 0.05 { return Err(Status(500));         } // random failure
    if r < 0.15 { self.clock.sleep(rng.ms(800)).await; } // random latency
    self.inner.get(url).await
}
```

- 🪄 You own the mock → you can **inject failure** right there
- 📚 Or use discrete-event sim: [madsim](https://github.com/madsim-rs/madsim) · [turmoil](https://github.com/tokio-rs/turmoil) · SimPy

---

# Who actually does this? 🧬

- 🧬 A small club — mostly **distributed databases**:
  - 🪨 FoundationDB · 🐯 TigerBeetle · 🧪 Antithesis · ☁️ Clever Cloud
- 🤔 If they trust their database to it, maybe it's worth a look

---

# Beating Super Mario to find bugs 🍄

- 🤖 **Antithesis** does *autonomous* testing — it hunts bugs for you
- 🍄 From pure random input, it **beats Super Mario Bros (1985)**…
- 🧱 …and finds a bug where **Mario clips through a wall**
- 🚀 Deterministic simulation as a genuinely **novel** testing strategy (video in the links!)

---

# We decided to build a database 🗄️

- 🏢 Clever Cloud: ~**80 people**, a **hoster** — our machines, BGP, Linux distro, load balancer, VM orchestrator
- 🗄️ …and now a **distributed, multi-model, multi-tenant database**
- 🔐 Writing a database is like crypto: **a thousand ways to mess up**
- 🦾 DST is the power tool: started at **1 dev → now 5 + 2 apprentices** shipping distributed-DB code, thanks to the **feedback loop**

---

# A simulation test, end to end 🧪

- 🔗 **Linked-list integrity** — objects point in a ring; reshuffle the pointers every transaction; **1000 txn/s**
- 🔌 **Random clogging** — deterministically cut the network *(closing connections in reverse finds more bugs 🤷)*
- 💀 **Attrition** — kill up to **10 machines**, keep **≥3 alive** (the system must still make progress)
- 🤯 **C++ chaos** — two dead machines in the same rack → **swap their disks** (the k8s-volume nightmare)

---

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---

# One seed decides everything 🎲

- 🎲 A single random **seed** drives the **entire** cluster timing
- 🚩 It even flips **feature flags** (*"enable TSS? sure, let's test it"*)
- ⏱️ **6s real ≈ 396s simulated** — when the cluster idles, we **fast-forward time**
- 🌌 So we test **hours** of outage in **seconds** — change the seed, change the world 🌍

---

# A fleet hunting bugs ☁️

- 🔁 We run the simulation **in a loop** — and **inside CI** (touch Materia's core → N rounds = integration-test++)
- ☁️ As a hoster, we run **many** sims: **30 min ≈ 24h** of absolute chaos
- 🛸 Next up: **bitflips / cosmic rays** — flip a bit in RAM or on disk and watch

---

<img src="/materia-sim-ci.png" class="w-full rounded shadow" />

---

# Deterministic replay = a superpower 🦸

- 🤖 The bot reports a **seed + commit** — and because it's deterministic, I **replay it locally**
- 🐞 That monster partition + data-corruption combo becomes a **debuggable integration test**
- 🛡️ We've **tested the worst world and the worst users** → we ship to prod **without fear** 🚀

---

# Not a silver bullet 🙅

- 🧱 **Hard to retrofit** onto a system not built for determinism
- 🎛️ You must **own all I/O** — even task scheduling
- 🪜 Can't go full DST? Fall back to **mocks + randomness** and still find real bugs

---

# 2025: what about AI? 🤖

- 🔁 DST gives a coding agent a **feedback loop**
- 🐛 The agent writes a test, gets a **seed that hits a "prod" bug**, and **self-corrects**
- 🧑‍🏫 The loop helps **juniors and AI alike** — both **discover what they don't know**

---

# Takeaways 🎁

- 🎯 Catch bugs **before** prod — spare your on-call engineers the **3am hell**
- 🦾 **Developer superpowers** — build robust, resilient systems in a container-heavy world
- 🌉 **Bridge dev & ops** — you can't hand everyone the pager, but you can give them **prod intuition**

---
layout: end
---

# Try simulation on your systems 🎲

[Blog post + the Super Mario video 🍄](https://pierrezemb.fr)

[crates.io/crates/foundationdb-simulation](https://crates.io/crates/foundationdb-simulation)

[pierrezemb.fr](https://pierrezemb.fr) — and come say hi at the **Clever Cloud** booth 👋

Questions? 💬 — catch me in the hallway
