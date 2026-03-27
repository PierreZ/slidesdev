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

# From HBase trauma to FoundationDB 🏗️

- 😱 Operated a **270-machine HBase cluster**. Never again.
- 🔍 Found FoundationDB: **simulation-tested**, rock-solid
- 🚀 Decided to build **Materia** on top — a serverless multi-model database
  - Indexes, query planner, quotas, multi-tenant isolation

---

# I was scared 😰

- 👤 Started alone, team grew to **6 engineers** — almost none with distributed systems background
- 🛡️ FDB tests **FDB** brilliantly — network partitions, crashes, disk failures, reconfigurations
- 😰 But our **layer code** has **zero simulation coverage**

<div class="flex justify-center mt-6">
  <div class="flex flex-col items-center gap-4">
    <div class="px-8 py-4 border-2 border-current rounded-lg font-bold text-xl opacity-40">🛡️ FDB Core — battle-tested under simulation</div>
    <div class="text-2xl">↕</div>
    <div class="px-8 py-4 border-2 rounded-lg font-bold text-xl" style="border-color: var(--theme-accent); color: var(--theme-accent);">😰 Our Layer — indexes, queries, quotas, KV, KMS, ETCD...</div>
  </div>
</div>

---

# What if we could hack the simulator? 🔧

- 📖 Stumbled across FDB's [external workload API](https://apple.github.io/foundationdb/client-testing.html)
- 🧙 Convinced our C++ wizard, built a **PoC in a weekend** — it worked!
- 📦 `fdbserver` loads a `.so` at runtime → calls `setup()` / `start()` / `check()` under full chaos
- 🎉 Open-sourced it in the [`foundationdb-simulation`](https://github.com/foundationdb-rs/foundationdb-rs) crate

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

---

# The determinism tax 🎲

Everything non-deterministic must go:

- 🎲 `rand` crate → `context.rnd()` (seeded by simulator)
- 🕰️ `SystemTime::now()` → `context.now()` (simulated clock)
- 📋 `HashMap` → `BTreeMap` (RandomState breaks reproducibility)

```rust
pub struct WorkloadContext { /* ... */ }
impl WorkloadContext {
    fn rnd(&self) -> u32;        // deterministic random
    fn now(&self) -> f64;        // simulated time
    fn client_id(&self) -> i32;  // which simulated client
    fn trace(&self, ...);        // log to FDB trace files
}
```

A failing seed is a **time machine** — same seed = same bugs, every time ⏪

---

# The ugly truth 🪳

**90%** of initial bugs caught by simulation: **bad error encapsulation blocking retries**

- 🔁 FDB transactions need proper retry loops — our wrappers swallowed errors
- 🔍 Simulation catches what **code review misses**
- 🤷 Most bugs are boring. That's the point.

> Simulation doesn't find *clever* bugs first. It finds the *dumb* ones you shipped anyway.

---

# The real win: a mindset shift 🧠

- 🐳 Initial setup: **20GB Docker images** matching FDB's C++ build env — nobody ran it locally
- 💥 C++ ABI broke between FDB 7.1→7.3 (GCC→Clang switch)
- 🎁 We contributed a **pure C API** upstream (FDB 7.4) — no more Docker
- 🚀 Once devs could **build and run locally**, everything changed

They stopped writing simplistic integration tests. They started writing **exploration strategies** — randomizing inputs, relying on the simulator to find edge cases.

> 🔍 Example: reindexation bugs across index types — code worked for 3 common types, rare ones **completely broken**. No review caught them.

---

# The LLM feedback loop 🤖

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

> "We would never have succeeded without simulation." 🦸

---
layout: end
---

# Try it on your FDB layers 🎁

The `foundationdb-simulation` crate is **open source** — approaching **15 million downloads** for the bindings.

- [github.com/foundationdb-rs/foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)
- [pierrezemb.fr](https://pierrezemb.fr)

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · Questions? 💬
