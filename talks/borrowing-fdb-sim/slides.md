---
theme: slidev-theme-pz
title: "Borrowing FoundationDB's Simulator for Layer Development"
layout: cover
themeConfig:
  accent: '#BB77FF'
---

# Borrowing FoundationDB's Simulator 
## for Layer Development 🧪

Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷 · Maintainer of [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)

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
    <div class="px-6 py-3 border-2 border-current rounded-lg font-bold opacity-40">🛡️ FDB Core — battle-tested</div>
    <div class="text-2xl">↕</div>
    <div class="px-6 py-3 border-2 rounded-lg font-bold" style="border-color: var(--theme-accent); color: var(--theme-accent);">😰 Our Layer — untested</div>
  </div>
</div>

---
layout: center 
---

# Can we hack our way into FDB's simulation framework? 🔧

---

# What if we could hack the simulator? 🔧

- 📖 Stumbled across FDB's [external workload API](https://apple.github.io/foundationdb/client-testing.html)
- 🧙 Convinced our C++ wizard, built a **PoC in a weekend**
  - It worked!
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

# Boring bugs? 🪳

* **90%** of initial bugs caught by simulation: **bad error encapsulation blocking retries**
  - 🔁 FDB transactions need proper retry loops
    - Our wrappers swallowed errors

---

# Boring bugs? 🪳

* **90%** of initial bugs caught by simulation: **bad error encapsulation blocking retries**
  - 🔁 FDB transactions need proper retry loops
    - Our wrappers swallowed errors
* But the **10% left** were **real bugs!**:
  - 🔀 Non-idempotent writes replayed on `MaybeCommitted`
  - 💥 Data corruption during reindexation across index types
  - 🗜️ ETCD compaction deleting live data
  - 📊 Query planner picking wrong indexes
  - 👯 Two workers grabbing the same task
  - 👑 Dual leader election under clock skew
  - ...

---

# The real win: a mindset shift 🧠

> "We stopped asking 'how do I **test** this?' — we started asking 'how do I **generate tests forever**?'"

- 🔀 From **prevention** to **discovery**
  - "What else is broken that we don't know about?"
- 🎲 Randomize everything
  - Inputs, operation sequences, failure timing
- 🧩 Encode invariants as code
  - The simulator checks them for you
- 🏗️ **Simulation as a first-class citizen**
  - New features are designed to be simulatable from day one
- 🔄 Every new feature **compounds**
  - Add an enum variant, write invariants, and all existing chaos applies automatically

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

Claude Code autonomously implemented **leader election** (13 invariants), **workflow engine**, **index types**
  - All validated by simulation.

---
layout: two-cols
---

::title::
# Every layer is simulation-driven now 🚀

::default::

### 🧪 Materia Framework

- 📊 Query planner correctness
- 🗳️ Leader election
- 💰 Quota enforcement
- ⚙️ Workflow engine
- 🔄 Indexing consistency

::right::

### 📦 Products

- 🗃️ **KV**
- 🔐 **KMS**
- 🔌 **ETCD**
- 🦎 **Materia Dyn**
- 🏢 **Internal services**
- ...

---
layout: two-cols
---

::title::
# Simulation runs continuously 🔄

::default::

- 💻 Engineers run a **few seeds locally** during development
- 🔁 CI runs **more iterations** on every push
- ☁️ Cloud runs simulation **continuously**
- ⏱️ **30 minutes** of simulation
  - = **24 hours** of chaos testing
- 🎲 Faulty seed found?
  - **Replay it locally**, deterministically

::right::

<img src="/materia-sim-ci.png" class="rounded shadow" />

---
layout: end
---

# Try it on your FDB layers 🎁

The `foundationdb-simulation` crate is **open source**

Approaching **16 million downloads** for the bindings.

[github.com/foundationdb-rs/foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)

[pierrezemb.fr](https://pierrezemb.fr)

Pierre Zemb — [@PierreZ](https://x.com/PierreZ) · Questions? 💬
