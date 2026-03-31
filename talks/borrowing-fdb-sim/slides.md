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

<div class="flex justify-center mt-6">
  <div class="flex flex-col items-stretch w-[32rem]">
    <div class="flex gap-2 mb-2">
      <div class="flex-1 px-3 py-2 border-2 rounded-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">KV</div>
      <div class="flex-1 px-3 py-2 border-2 rounded-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">ETCD</div>
      <div class="flex-1 px-3 py-2 border-2 rounded-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">KMS</div>
      <div class="flex-1 px-3 py-2 border-2 rounded-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">Workflow</div>
      <div class="flex-1 px-3 py-2 border-2 rounded-lg text-center text-sm font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">...</div>
    </div>
    <div class="px-4 py-3 border-2 rounded-lg mb-2" style="border-color: var(--theme-accent); color: var(--theme-accent);">
      <div class="font-bold text-center">Materia Framework</div>
      <div class="text-xs text-center mt-1 opacity-80">multi-tenancy · registration · schema · indexing · query planner · quota</div>
    </div>
    <div class="px-4 py-3 border-2 border-current rounded-lg text-center font-bold opacity-40">🛡️ FoundationDB — battle-tested</div>
  </div>
</div>

All of this is **classically tested**. How do we build **confidence** to ship?

---

# How FDB achieves this 🔬

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-stretch w-[28rem]">
    <div class="px-6 py-4 border-2 border-current rounded-t-lg text-center font-bold text-lg">FDB Server Code</div>
    <div class="px-6 py-2 border-2 border-t-0 border-current text-center text-sm opacity-60">g_network interface</div>
    <div class="flex">
      <div class="flex-1 px-4 py-3 border-2 border-t-0 border-current rounded-bl-lg text-center opacity-40">
        <div class="font-bold">Net2</div>
        <div class="text-sm">production</div>
      </div>
      <div class="flex-1 px-4 py-3 border-2 border-t-0 border-l-0 rounded-br-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold">Sim2</div>
        <div class="text-sm">network, disk, time, crashes</div>
      </div>
    </div>
  </div>
</div>

One interface swap. **Same code. Everything else is faked.**

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
  <div class="border-2 border-current rounded-lg w-[32rem] p-4">
    <div class="text-center font-bold text-sm mb-3">FDB Simulator (<code>fdbserver -r simulation</code>)</div>
    <div class="px-4 py-3 border-2 rounded-lg mb-2" style="border-color: var(--theme-accent); color: var(--theme-accent);">
      <div class="font-bold text-center">🦀 .so Workload</div>
      <div class="text-xs text-center mt-1 opacity-80">Materia Framework + data access layer</div>
    </div>
    <div class="text-center py-1 text-sm opacity-60">↓ g_network / FDB Client API</div>
    <div class="flex mt-2">
      <div class="flex-1 px-4 py-2 border-2 border-current rounded-l-lg text-center opacity-40">
        <div class="font-bold text-sm">Net2</div>
        <div class="text-xs">disabled</div>
      </div>
      <div class="flex-1 px-4 py-2 border-2 border-l-0 rounded-r-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold text-sm">Sim2 💥</div>
        <div class="text-xs">network, disk, time, crashes</div>
      </div>
    </div>
  </div>
</div>

Compiles to a `.so` — talks to FDB normally, but FDB runs with **Sim2 underneath**.

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

<div class="text-xl mt-4 mb-6 pl-4 border-l-4" style="border-color: var(--theme-accent);">
"We stopped asking 'how do I <b style="color: var(--theme-accent);">test</b> this?' and started asking 'how do I <b style="color: var(--theme-accent);">break</b> this?'"
</div>

- 🏗️ New features are **designed to be simulatable** from day one
- 🔄 Every new feature **compounds** — add an enum variant, and all existing chaos applies
- 🤖 Works as a **feedback loop for LLMs** too — failing seed → deterministic replay → fix

---

# How we run it 🔄

- 💻 **Dev**: a few seeds locally during development
- 🔁 **CI**: more iterations on every push
- ☁️ **Cloud**: simulation runs **continuously**
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

---

# Bonus: How to structure the code 📦

```
my-project/
├── my-fdb-service/      # Core FDB operations — NO tokio
│                        # → this compiles into the .so
├── my-grpc-server/      # Production layer (tokio + tonic)
└── my-fdb-workloads/    # Simulation workloads
```

The key: isolate your FDB logic from your async runtime. The `.so` can't use tokio — it runs on FDB's Flow event loop.

---

# Bonus: The price of determinism 🎲

Simulation requires **deterministic execution**. Same seed = same bugs — but only if you eliminate all sources of non-determinism:

| ❌ Forbidden | ✅ Use instead |
|---|---|
| `HashMap`, `HashSet` | `BTreeMap`, `BTreeSet` |
| `rand::random()` | `context.rnd()` |
| `SystemTime::now()` | `context.now()` |
| `tokio::spawn()` | Simulation executor |
| Manual retry loops | `db.run()` |

---

# Bonus: The LLM feedback loop 🤖

<div class="flex justify-center items-center mt-8 gap-3">
  <div class="px-3 py-2 border-2 border-current rounded text-sm">🤖 LLM writes code</div>
  <div>→</div>
  <div class="px-3 py-2 border-2 border-current rounded text-sm">🧪 Sim finds bug</div>
  <div>→</div>
  <div class="px-3 py-2 border-2 border-current rounded text-sm">🔍 Reads replay</div>
  <div>→</div>
  <div class="px-3 py-2 border-2 border-current rounded text-sm">🔧 Fixes code</div>
  <div>→ 🔁</div>
</div>

Claude Code autonomously implemented **leader election** (13 invariants), **workflow engine**, **index types** — all validated by simulation.

---

# Bonus: The boring bug — `commit_unknown_result` 🔁

- FDB can return `commit_unknown_result` — your transaction **may or may not** have committed
- If your retry loop doesn't handle this → non-idempotent writes silently replayed
- We hit this **across every layer** — our wrappers swallowed the error
- Fix: enable `AutomaticIdempotency` in FDB 7.3 ([blog post](https://pierrezemb.fr/posts/automatic-txn-fdb-730/))

---

# Bonus: The C++ ABI nightmare 💀

- FDB 7.3 switched from **GCC → Clang**
- `std::string` layout mismatch between fdbserver and our `.so` → **segfaults**
- Every FDB release could silently break external workloads
- [Forum post](https://forums.foundationdb.org/t/externalworkload-segmentation-fault-in-7-3/4375) documenting the pain
- Our fix: contributed a [**pure C API**](https://github.com/apple/foundationdb/pull/11288) upstream
  - Stable ABI, no compiler coupling — just like `fdb_c.h`
