---
theme: slidev-theme-pz
title: "Borrowing FoundationDB's Simulator for Layer Development"
layout: cover
themeConfig:
  accent: '#007AFF'
---

# Borrowing FoundationDB's Simulator
## for Layer Development 🧪

Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷

Maintainer of [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)

---

# From HBase pain to FDB layers 🏗️

- 😱 After years of **HBase trauma** (270-machine cluster, manual recovery, deleting shards...)
- 🔍 I discovered **FoundationDB** — rock-solid, simulation-tested
- 🚀 Now building **serverless databases** on top of FDB at Clever Cloud
  - KV, KMS, ETCD, workflow engine...
- 👤 Team grew from **1** to **6 engineers** — almost none with dist-sys background

---

# The gap 😰

<div class="flex justify-center mt-12">
  <div class="flex flex-col items-center gap-4">
    <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😰 Our Layer — untested</div>
    <div class="text-2xl">↕</div>
    <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🛡️ FDB Core — battle-tested</div>
  </div>
</div>

How do we build **confidence** to ship?

---

# How FDB achieves this 🔬

<div class="flex justify-center mt-8 gap-12">
  <div class="flex flex-col items-center gap-2">
    <div class="text-sm font-bold opacity-60">Production</div>
    <div class="flex flex-col items-stretch w-48">
      <div class="px-4 py-3 border-2 border-current rounded-t-lg text-center font-bold">FDB Code</div>
      <div class="px-4 py-3 border-2 border-t-0 border-current rounded-b-lg text-center opacity-60">Net2 (real network)</div>
    </div>
  </div>
  <div class="flex flex-col items-center gap-2">
    <div class="text-sm font-bold opacity-60">Simulation</div>
    <div class="flex flex-col items-stretch w-48">
      <div class="px-4 py-3 border-2 border-current rounded-t-lg text-center font-bold">FDB Code</div>
      <div class="px-4 py-3 border-2 border-t-0 rounded-b-lg text-center font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">Sim2 (fake everything)</div>
    </div>
  </div>
</div>

**Same code** in both. Swap the interface. Add a seed → **deterministic execution**.

Same seed = same bugs. **Every time.**

---

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---
layout: center
---

# I want that for my Rust layers! 🤩

---

# Getting Rust inside the simulator 🦀

<div class="flex justify-center mt-4">
  <div class="flex flex-col items-stretch w-[32rem]">
    <div class="px-4 py-2 border-2 border-current rounded-t-lg text-center font-bold text-sm">FDB Simulator (<code>fdbserver -r simulation</code>)</div>
    <div class="px-4 py-3 border-2 border-t-0 text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
      <div class="font-bold">🦀 Our Rust Workload (.so)</div>
      <div class="text-sm">setup() · start() · check()</div>
    </div>
    <div class="text-center py-1 text-sm opacity-60">↓ FDB Client API</div>
    <div class="px-4 py-3 border-2 border-current text-center opacity-40">
      <div class="font-bold">🛡️ FDB Cluster (real code)</div>
      <div class="text-sm">with fakes: Sim2 network, simulated disk & time</div>
    </div>
    <div class="px-4 py-2 border-2 border-t-0 border-current rounded-b-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">💥 chaos happens here</div>
  </div>
</div>

Our Rust code compiles to a `.so` — talks to FDB normally, but FDB runs with **fakes underneath**.

---

# The interface 🔧

```rust
pub trait RustWorkload {
    /// Populate the database
    async fn setup(&mut self, db: SimDatabase);
    /// Run the actual test under chaos
    async fn start(&mut self, db: SimDatabase);
    /// Verify invariants — did our data survive?
    async fn check(&mut self, db: SimDatabase);
}
```

Open-sourced in the [`foundationdb-simulation`](https://github.com/foundationdb-rs/foundationdb-rs) crate.

---

# At first: boring bugs 🪳

- 🧪 First workloads were **simple** — just verifying reads and writes
- 🔁 Across every layer, simulation found the **same thing**
  - Our wrappers swallowed errors instead of propagating them
- 😅 Useful, but not very exciting

---

# Then we added real complexity 🔥

- 🧪 Richer workloads: **business logic under chaos**
- 💥 Simulation started finding **real bugs**:
  - 💥 Data corruption during reindexation across index types
  - 🗜️ ETCD compaction deleting live data
  - 📊 Query planner picking wrong indexes
  - 👑 Dual leader election under clock skew
  - ...

---

# The real win: a mindset shift 🧠

> "We stopped asking 'how do I **test** this?' and started asking 'how do I **break** this?'"

- 🏗️ New features are **designed to be simulatable** from day one
- 🔄 Every new feature **compounds** — add an enum variant, and all existing chaos applies

---

# How we run it 🔄

- 💻 **Dev**: a few seeds locally during development
- 🔁 **CI**: more iterations on every push
- ☁️ **Cloud**: simulation runs **continuously**
- ⏱️ **30 minutes** of simulation = **24 hours** of chaos testing
- 🎲 Faulty seed? **Replay it locally**, deterministically

---

# Contributing back to FoundationDB 🦀

- 🦀 One of **two teams in the world** running Rust inside FDB's simulator
- 🛠️ Contributed a [**pure C API**](https://github.com/apple/foundationdb/pull/11288) upstream — no more C++ ABI nightmares
- ⏱️ Added [`delay()` API](https://github.com/apple/foundationdb/pull/12357) for time-dependent simulation
- 🔁 Full circle: from **happy users** to **upstream contributors**

---
layout: end
---

# Try it on your FDB layers 🎁

[crates.io/crates/foundationdb](https://crates.io/crates/foundationdb)

[pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬
