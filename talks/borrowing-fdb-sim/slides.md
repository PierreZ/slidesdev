---
theme: slidev-theme-pz
title: "Borrowing FoundationDB's Simulator for Layer Development"
layout: cover
themeConfig:
  accent: '#BB77FF'
---

# Borrowing FoundationDB's Simulator 
## for Layer Development 🧪

Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷

---
layout: two-cols
---

::title::
# From HBase trauma to FoundationDB 🏗️

::default::

### 😱 The pain

- **270-machine HBase cluster**
- Every network flap was a mess
  - Recovery = running `hbck`
  - Deleting shards/HFiles to restore consistency

::right::

<div class="opacity-0">

### 🔍 The discovery

- **FoundationDB**: simulation-tested, rock-solid
- **Deterministic simulation**
  - Network partitions, crashes, reboots
- **Millions of runs per night**
  - Every bug reproducible by seed

</div>

---
layout: two-cols
---

::title::
# From HBase trauma to FoundationDB 🏗️

::default::

### 😱 The pain

- **270-machine HBase cluster**
- Every network flap was a mess
  - Recovery = running `hbck`
  - Deleting shards/HFiles to restore consistency


::right::

### 🔍 The discovery

- **FoundationDB**: simulation-tested, rock-solid
- **Deterministic simulation**
  - Network partitions, crashes, reboots
- **Millions of runs per night**
  - Every bug reproducible by seed

---

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---
layout: two-cols
---

::title::
# I was scared 😰

::default::

- 🚀 Building **Serverless databases** on top of FDB
  - Virtualized databases for:
    - KV, KMS, ETCD, workflow engine...
- 👤 Team went from **1** to **6 engineers**
  - Almost none with dist-sys background
- 😰 How do we build **confidence** to ship all of this?

::right::

<div class="flex justify-center h-full items-center">
  <div class="flex flex-col items-center gap-4">
    <div class="px-6 py-3 border-2 rounded-lg font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">😰 Our Layer — untested</div>
    <div class="text-2xl">↕</div>
    <div class="px-6 py-3 border-2 border-current rounded-lg font-bold opacity-40">🛡️ FDB Core — battle-tested</div>
  </div>
</div>

---
layout: center 
---

# Can we hack our way into FDB's simulation framework? 🤔

---

# What if we could hack the simulator? 🔧

- 📖 Stumbled across FDB's [external C++ workload API](https://apple.github.io/foundationdb/client-testing.html)
- 🧙 Convinced our C++ wizard, built a **PoC in a weekend**
  - It worked!
- 📦 `fdbserver` loads a `.so` at runtime
  - calls `setup()` / `start()` / `check()` under full chaos
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

# At first: boring bugs 🪳

- 🧪 First workloads were **simple** — just verifying reads and writes
- 🔁 Across every layer, simulation found the **same thing**
  - Bad error encapsulation blocking retries
- 😅 Useful, but not very exciting

---

# Then we added real complexity 🔥

- 🧪 Richer workloads: **business logic under chaos**
- 💥 Simulation started finding **real bugs**:
  - 💥 Data corruption during reindexation across index types
  - 🗜️ ETCD compaction deleting live data
  - 📊 Query planner picking
    - wrong indexes
    - wrong data
  - 👑 Dual leader election under clock skew
  - ...

---

# The real win: a mindset shift 🧠

> "We stopped asking 'how do I **test** this?' — we started asking 'how do I **generate tests forever**?'"

- 🔀 From **prevention** to **discovery** — "what else is broken that we don't know about?"
- 🏗️ New features are **designed to be simulatable** from day one
- 🔄 Every new feature **compounds** — add an enum variant, and all existing chaos applies

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

---

# Simulation runs everywhere, all the time 🚀

- 📊 Query planner, 🗳️ leader election, ⚙️ workflow engine, 🔄 indexing...
- 🗃️ **KV**, 🔐 **KMS**, 🔌 **ETCD**, 🦎 **Materia Dyn**, 🏢 internal services
- 💻 Engineers run **a few seeds locally**
- 🔁 CI runs **more iterations** on every push
- ☁️ Cloud runs simulation **continuously**

---

# Contributing back to FoundationDB itself 🔄

- 😱 C++ external workload API = **ABI nightmare**
  - FDB 7.3 switched GCC → Clang
  - `std::string` layout mismatch → **segfaults**
- 🛠️ Our fix: contributed a [**pure C API** upstream](https://github.com/apple/foundationdb/pull/11288)
  - Stable ABI, no compiler coupling — just like `fdb_c.h`
- ⏱️ Added [`delay()` API](https://github.com/apple/foundationdb/pull/12357) to simulate time-dependent behavior

---
layout: end
---

# Try it on your FDB layers 🎁

[crates.io/crates/foundationdb](https://crates.io/crates/foundationdb)

Pierre Zemb — [pierrezemb.fr](https://pierrezemb.fr) · Questions? 💬
