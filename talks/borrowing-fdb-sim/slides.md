---
theme: slidev-theme-pz
title: "Borrowing FoundationDB's Simulator for Layer Development"
layout: cover
themeConfig:
  accent: '#BB77FF'
---

# Borrowing FoundationDB's Simulator 🧪
## for Layer Development

Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷 · Maintainer of [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)

---

# From HBase trauma to Materia 🏗️

- 😱 Operated a **270-machine HBase cluster**. Never again.
- Found FDB: **simulation-tested**, rock-solid — never woke us up at 3 AM
- Built **Materia**: serverless multi-model database on FDB
  - Indexes, query planner, quotas, multi-tenant isolation
- 👥 Team of **6 engineers, only 1 with distributed systems background**
  - How do you let everyone contribute to a distributed database **safely**?

---

# FDB never woke us up 🛡️

FoundationDB tests itself brilliantly via **deterministic simulation**:

- 🌐 Network partitions
- 💥 Machine crashes & reboots
- 💾 Disk failures & corruption
- 🔀 Cluster reconfigurations

> "FoundationDB would not ship a release until it had run the equivalent of **a trillion combined CPU-hours** of simulation." — Will Wilson

---

# But our layer code... 🤔

FDB tests **FDB**. Not our code.

<div class="flex justify-center mt-6">
  <div class="flex flex-col items-center gap-4">
    <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🛡️ FDB Core — battle-tested under simulation</div>
    <div class="text-2xl">↕</div>
    <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😰 Our Layer — indexes, queries, quotas, KV, KMS, ETCD...</div>
  </div>
</div>

**What if we could run our Rust code inside FDB's actual simulator?**

---

# The architecture 🏗️

<div class="flex justify-center mt-4">
  <div class="flex flex-col items-center gap-3">
    <div class="px-6 py-3 border-2 border-current rounded-lg font-bold text-lg">fdbserver -r simulation</div>
    <div class="text-xl">↓ loads at runtime</div>
    <div class="px-6 py-3 border-2 rounded-lg font-bold text-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">libmyworkload.so (Rust cdylib)</div>
    <div class="text-xl">↓ calls</div>
    <div class="flex gap-4">
      <div class="px-4 py-2 border-2 border-current rounded text-sm">workloadCFactory()</div>
      <div class="text-xl">→</div>
      <div class="px-4 py-2 border-2 rounded text-sm" style="border-color: var(--theme-accent); color: var(--theme-accent);">RustWorkload</div>
    </div>
    <div class="text-xl">↓ lifecycle</div>
    <div class="flex gap-6">
      <div class="px-4 py-2 border-2 border-current rounded text-sm font-bold">setup()</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm font-bold">start()</div>
      <div class="px-4 py-2 border-2 border-current rounded text-sm font-bold">check()</div>
    </div>
  </div>
</div>

Same deterministic chaos. Same network partitions. Same machine crashes. **But now testing our layer code.**

---

# Hacking in: async swap + determinism 🔄

Rust's **swappable async runtime** is the unlock:

```rust
// Replace tokio with FDB's event loop as our executor
foundationdb::future::CUSTOM_EXECUTOR_HOOK
    .set(poll_pending_tasks)
    .unwrap();
```

The determinism tax — everything non-deterministic must go:

- 🎲 `rand` crate → `context.rnd()` (seeded by simulator)
- 🕰️ `SystemTime::now()` → `context.now()` (simulated clock)
- 📋 `HashMap` → `BTreeMap` (RandomState breaks reproducibility)

A failing seed is a **time machine** — same seed = same bugs, every time ⏪

---

# The workload contract 📝

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

```rust
pub struct WorkloadContext { /* ... */ }
impl WorkloadContext {
    fn rnd(&self) -> u32;        // deterministic random
    fn now(&self) -> f64;        // simulated time
    fn client_id(&self) -> i32;  // which simulated client
    fn trace(&self, ...);        // log to FDB trace files
}
```

---

# The LLM feedback loop 🤖🔁

DST as the ultimate **LLM validation loop**:

<div class="flex justify-center mt-4">
  <div class="flex flex-col items-center gap-1 text-lg">
    <div class="px-4 py-2 border-2 border-current rounded">🤖 LLM writes workload / layer code</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🧪 Simulation finds bug (failing seed)</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🔍 LLM reads deterministic replay</div>
    <div>↓</div>
    <div class="px-4 py-2 border-2 border-current rounded">🔧 LLM fixes code</div>
    <div>↓</div>
    <div>🔁</div>
  </div>
</div>

Claude Code autonomously implemented **leader election** (13 invariants), **workflow engine**, **index types** — all validated by simulation.

---

# Every layer is simulation-driven now 🚀

| Layer | What's tested |
|-------|--------------|
| 🗃️ **KV (Redis)** | Data integrity, index consistency |
| 🔐 **KMS** | Key management under failures |
| 🗳️ **Leader Election** | 13 invariants, clock skew, split-brain |
| 📊 **Query Planner** | Reference-model correctness |
| 💰 **Quotas** | Storage accounting under chaos |
| ⚙️ **Workflow Engine** | State machine transitions |

Team of 6 shipping with **production-level confidence**.

> "We would never have succeeded without simulation." 🦸

---
layout: two-cols
---

::title::

# Simulation runs continuously 🔄

::default::

- 💻 Engineers run **a few seeds locally** during development
- 🔁 CI runs **more iterations** on every PR
- ☁️ Cloud runs simulation **continuously**
- ⏱️ **30 minutes** of simulation = **24 hours** of chaos testing
- 🎲 Faulty seed found? **Replay it locally**, deterministically

::right::

<img src="/materia-sim-ci.png" class="rounded shadow" />

---

# Contributing back 🎁

We didn't just borrow — we contributed upstream:

- 🏗️ **FoundationDB core** — contributed the **pure C workload API** (FDB 7.4), replacing the fragile C++ ABI bridge
- 🦀 **foundationdb-rs** — the `foundationdb-simulation` crate is **open source**, approaching **15 million downloads** for the bindings
- 📖 Blog posts: *[Diving into FDB's Simulation](https://pierrezemb.fr/posts/diving-into-foundationdb-simulation/)*, *[Writing Workloads That Find Bugs](https://pierrezemb.fr/posts/writing-rust-fdb-workloads-that-find-bugs/)*

To our knowledge, **we're the first** to make this integration work — even Apple, Snowflake, and Datadog haven't integrated layer code into the simulator this way.

---

# Production bugs caught in CI, not at 3 AM 😴

<div class="flex justify-center mt-6">
  <div class="flex gap-12">
    <div class="flex flex-col items-center gap-4">
      <div class="text-4xl">😱</div>
      <div class="px-6 py-3 border-2 border-current rounded-lg text-center opacity-40">
        <div class="font-bold">Before</div>
        <div class="text-sm mt-1">Hope for the best</div>
        <div class="text-sm">3 AM pages</div>
        <div class="text-sm">Manual testing</div>
      </div>
    </div>
    <div class="text-4xl self-center">→</div>
    <div class="flex flex-col items-center gap-4">
      <div class="text-4xl">😴</div>
      <div class="px-6 py-3 border-2 rounded-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold">After</div>
        <div class="text-sm mt-1">Simulation on every PR</div>
        <div class="text-sm">Deterministic replay</div>
        <div class="text-sm">Ship with confidence</div>
      </div>
    </div>
  </div>
</div>

The `foundationdb-simulation` crate is **open source**. If you're building on FDB, you can borrow the simulator too.

- [github.com/foundationdb-rs/foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)
- [pierrezemb.fr](https://pierrezemb.fr)

---
layout: end
---

# Thank you! 🙏

**Borrowing FoundationDB's Simulator for Layer Development**

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · [pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬
