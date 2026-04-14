---
theme: slidev-theme-pz
title: "Borrowing FoundationDB's Simulator for Layer Development"
layout: cover
themeConfig:
  accent: '#BB77FF'
  secondary: '#FF8775'
---

# Borrowing FoundationDB's Simulator
## for Layer Development 🧪

Pierre Zemb — Staff Engineer @ Clever Cloud 🇫🇷

Maintainer of [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs)

---

# From HBase pain to FDB layers 🏗️

- 👨‍💻 Software engineer who learned **dist-sys pitfalls the hard way**
- 😱 Years of **Apache HBase trauma** (270 machines, **2.5M w/s**, **6.5M r/s**)
  - Network split during region split → **inconsistent cluster**
  - Repair tool (`hbck`) moved partitioned data out of the keyspace to make it "consistent" 🙃
- 🔍 I discovered **FoundationDB**, a distributed key-value store
  - For developers, a toolbox to write distributed systems
  - For operators, a highly scalable / production-ready system

---

# FDB's simulation framework 🔬

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

- 🎭 Every failable interaction behind **one interface**: network, disk, time, randomness
- 💥 Swap the implementation for **fakes** → inject failures at will
- 🎲 Deterministic → same seed = **same bugs, every time**

---

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---

# So, we started building 😰

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-stretch w-[28rem]">
    <div class="px-4 py-3 border-2 rounded-lg mb-2" style="border-color: var(--theme-accent); color: var(--theme-accent);">
      <div class="font-bold text-center">Materia Framework</div>
      <div class="text-xs text-center mt-1 opacity-80">multi-tenancy · registration · schema · indexing · query planner · quota</div>
    </div>
    <div class="px-4 py-3 border-2 border-current rounded-lg text-center font-bold opacity-40">🛡️ FoundationDB — battle-tested</div>
  </div>
</div>

---

# Team grew from 1 to 6 🚀

<div class="flex justify-center mt-8">
  <div class="flex flex-col items-stretch w-[28rem]">
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

<div class="text-center mt-12 text-xl">How do we test this? 🙃</div>

---
layout: center
---

# Can we inject our code inside FoundationDB? 🤔

---

# ExternalWorkload API 🔌

<div class="flex justify-center mt-4">
  <div class="border-2 border-current rounded-lg w-[28rem] p-4">
    <div class="text-center font-bold text-sm mb-3">FDB Simulator (<code>fdbserver -r simulation</code>)</div>
    <div class="flex flex-col items-stretch">
      <!-- ExternalWorkload API - highlighted -->
      <div class="px-6 py-3 border-2 rounded-t-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold">ExternalWorkload API</div>
        <div class="text-xs mt-1 opacity-80">loads a shared object at runtime</div>
      </div>
      <!-- FDB cluster under simulation -->
      <div class="px-4 py-3 border-2 border-t-0 border-current rounded-b-lg text-center opacity-40">
        <div class="font-bold">FDB Cluster — under full simulation 💥</div>
        <div class="text-xs mt-1 flex justify-center gap-3 flex-wrap">
          <span>🔌 partitions</span>
          <span>💾 disk failures</span>
          <span>⏰ clock skew</span>
          <span>💀 process kills</span>
        </div>
      </div>
    </div>
  </div>
</div>

Your code talks to FDB normally — but FDB runs with **fakes underneath**

---

# Getting Rust inside the simulator 🦀

<div class="flex justify-center mt-4">
  <div class="border-2 border-current rounded-lg w-[28rem] p-4">
    <div class="text-center font-bold text-sm mb-3">FDB Simulator (<code>fdbserver -r simulation</code>)</div>
    <div class="flex flex-col items-stretch">
      <!-- ExternalWorkload containing Rust code -->
      <div class="px-4 py-3 border-2 rounded-t-lg" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold text-center text-sm mb-2">ExternalWorkload API</div>
        <div class="px-4 py-2 border-2 rounded-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
          <div class="font-bold">🦀 Rust code</div>
          <div class="text-xs mt-1 opacity-80">Materia Framework + data access layer</div>
        </div>
      </div>
      <!-- FDB cluster under simulation -->
      <div class="px-4 py-3 border-2 border-t-0 border-current rounded-b-lg text-center opacity-40">
        <div class="font-bold">FDB Cluster — under full simulation 💥</div>
        <div class="text-xs mt-1 flex justify-center gap-3 flex-wrap">
          <span>🔌 partitions</span>
          <span>💾 disk failures</span>
          <span>⏰ clock skew</span>
          <span>💀 process kills</span>
        </div>
      </div>
    </div>
  </div>
</div>

---

# At first: boring bugs 🪳

- 🧪 Simple workloads on the **Materia framework**
  - KV integrity checks, statistics up-to-date...
- 🤷 Essentially just testing **FDB's transactional promises**
  - The team was not convinced of the benefits, until...

---

# Then workloads got richer 🔥

- 🧪 As workloads got **richer**, simulation found bugs everywhere:
  - 📊 Query execution returning wrong data
  - 🗺️ Query planning picking the wrong index
  - 💥 Data corruption during reindexation
  - 👑 Dual leader election under clock skew
  - 🗜️ ETCD compaction deleting live data
  - ⏰ Bad TTL handling in Redis-shim
  - ...

---

# Simulation finds unknown unknowns 🔮

- 🧠 Once each team member:
  - Found a bug in **their own code**, or
  - Contributed a **low-level piece of engineering** quickly
- 💡 They understood the **power of simulation**
  - simulation finds **unknown unknowns**
- 🏗️ All our work is now **simulation-first**

---

# Contributing back to FoundationDB 🦀

- 🦀 **First language bindings** (outside of C++) to ship simulation support
  - open-sourced in foundationdb-rs
- 🔁 Full circle: from **happy users** to **upstream contributors**
  - 🛠️ Contributed a [**pure C API**](https://github.com/apple/foundationdb/pull/11288) upstream — no more C++ ABI nightmares
  - ⏱️ Added [`delay()` API](https://github.com/apple/foundationdb/pull/12357) for time-dependent simulation

---
layout: end
---

# Try it on your FDB layers 🎁

[crates.io/crates/foundationdb](https://crates.io/crates/foundationdb)

[crates.io/crates/foundationdb-simulation](https://crates.io/crates/foundationdb-simulation)

[pierrezemb.fr](https://pierrezemb.fr)

Questions? 💬

