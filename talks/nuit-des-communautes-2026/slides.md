---
theme: slidev-theme-pz
title: "My new job as a software engineer"
themeConfig:
  accent: '#8A3FC9'
  secondary: '#B85E1F'
---

# My community, since 2011 🤝

- 🎂 Born as **FinistJUG** in December 2011, with Antonio Goncalves on stage
- 🌙 **130+ evenings** since, on Java, cloud, DevOps, web, and whatever a member wants to share
- 👥 Three organizers: Horacio, Stéphanie, and me. Talks are given by **you**
- 📅 Next evening: **[PLACEHOLDER: date and venue]**

<div class="mt-8 text-center text-xl">
<a href="https://finistdevs.org">finistdevs.org</a>, bring a colleague.
</div>

---
layout: cover
class: nuit-cover
---

# My new job as<br>a software engineer 🚀

Pierre Zemb, FinistDevs

La Nuit des Communautés Bretonne #3, Brest, 17 September 2026 🌙

<style>
.nuit-cover {
  background: #1B1D34 !important;
  background-image: linear-gradient(160deg, rgba(220,150,82,0.35) 0%, rgba(27,29,52,0) 45%, rgba(223,157,247,0.35) 100%) !important;
  color: #ffffff !important;
}
.nuit-cover h1 {
  background: linear-gradient(90deg, #f2b27a, #e59df7);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent !important;
}
.nuit-cover p {
  color: #d9d9e6 !important;
}
</style>

---

# $ whoami 👋

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🛠️ Staff engineer at Clever Cloud, working around **distributed systems**
  - 📟 Building, contributing, debugging, **getting paged**
- 🦀 Maintaining [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs) and a few other Rust libraries
- 🤝 Co-organizing **FinistDevs**, right here in Brest
- 🏸 Squash player

</div>
<img src="https://pierrezemb.fr/images/myself.jpg" class="w-40 h-40 rounded-lg object-cover" />
</div>

---

# 2010: I coded for the puzzle ☕

<div class="flex items-center justify-center gap-8 mt-6">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🧑‍💻 Me</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">💥 Segfault</div>
</div>

<div class="mt-8">

- 🎓 Engineering school in Brest. I came for **electronics**, I found **C**
- 🌙 A night, a coffee, and in the morning **something exists** that did not
- 🧠 Six months on pointers. The compiler and valgrind were the first things that **told me I was wrong**

</div>

<div class="mt-6 text-center text-xl">
I coded <strong>to code</strong>. The puzzle was the reward.
</div>

---

# Why distributed systems? 🌐

Two internships, two apprenticeships, all in **software infrastructure**: Arkea, then OVHcloud. At Arkea: Hadoop, Pig, and machines that had to agree.

- 🧗 They are **hard**, so they are interesting
- 🌍 They are **everywhere**: your bank, your phone, the ticket you scanned tonight
- 📖 Because they are hard, there is **a lot to learn** from them

<div class="mt-10 text-center text-xl">
One machine was a puzzle. <strong>Seventy machines was a calling.</strong>
</div>

---

# 2017: then I got a pager 📟

<div class="flex items-center justify-center gap-8 mt-4">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🧑‍💻 Me</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">📟 Pager, 3 a.m.</div>
</div>

<div class="mt-6">

OVH Metrics, full time since 2016: **800 servers**, 1.8M points per second. On-call was our test suite.

- 📟 **[PZ: what on-call taught you, first lesson, your words]**
- 🔭 **[PZ: second lesson]**
- 🧯 **[PZ: third lesson]**

</div>

<div class="mt-6 text-center text-xl">
My job became <strong>finding out I'm wrong before the users do</strong>.
</div>

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

<div class="mt-2 text-center text-lg" style="color: var(--theme-accent);">
Can we automate finding what we don't know in the code? 🤔
</div>

---

# A database whose simulator came first 🎲

<div class="flex items-center justify-center gap-8 mt-6">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🧑‍💻 Me</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulator</div>
</div>

<div class="mt-8">

- 🔧 My own cluster next: **250+ machines** of HBase, and a repair tool that fixed inconsistency by **moving data out of the keyspace** 💀. The pager tells the truth, too late
- 🍎 FoundationDB wrote its simulator **before the database**: partitions, crashes, swapped disks, from a single seed. **4,000 years** of bad Tuesdays a year
- 🔬 A **handful of companies in the world** work this way. The niche inside the niche

</div>

<div class="mt-6 text-center text-xl">
Same move, every job: <strong>compiler, pager, simulator</strong>. Each one faster than the last.
</div>
---

# The simulator became the tool 🎲

- 🏢 2021, Clever Cloud, Materia: a database built on FoundationDB by **1 to 6 people**, almost none with database internals. Six people at a French cloud provider, **doing what Apple does**
- 🎭 We cheated: our code runs **inside FoundationDB's simulator**. No simulator to write, the world underneath is fake and hostile
- 🔥 Every commit goes through partitions, crashes and clock skew. Bugs found **before anyone is paged**
- 🧑‍🎓 A newcomer shipped a deep feature in **one week**. The simulator taught him the rules nobody had written down

<div class="mt-8 text-center text-xl">
A simulator is not a test. <strong>It is the tool we write complicated software with.</strong>
</div>

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---
layout: center
---

# Then the LLMs showed up 🤖

**[PLACEHOLDER: your skeptic sentence about LLMs, to whom, when]**

---

# AI broke "good enough" 🤖

> "We said good enough because we wrote it, we understood it, we tried it. AI broke all three."

*Steve Klabnik, [BugBash 2026](https://pierrezemb.fr/posts/bugbash-2026/)*

Before LLMs, we trusted code because we:

- 🧠 understood it
- ✍️ wrote it
- 🧪 tried it
- 📟 got paged by it

<div class="mt-4 text-center text-lg">
AI broke the first three. The only feedback left is the <strong>worst</strong> one: the pager.
</div>

---

# Same model, two very different codebases 😬

- 🦜 My colleagues, same model, no simulator: code that reads well and **falls apart** under pressure
- 🎲 My code, same model, under the simulator: **very, very good**
- ⚡ I found out fast, and it was not the prompt, and not the model

<div class="mt-10 text-center text-xl">
<strong>It was the loop.</strong>
</div>

---

# You don't trust Claude, you trust the simulator 🎯

<div class="grid grid-cols-2 gap-8 place-items-center">
  <img src="/boris-cherny-tweet.png" class="rounded shadow w-full" />
  <img src="/claude-moonpool.png" class="rounded shadow w-full" />
</div>

---

# The writer changed, the loop did not 🔁

<div class="flex items-center justify-center gap-8 mt-10">
  <div class="px-8 py-6 border-2 border-dashed border-current rounded-lg text-center font-semibold">🤖 Claude</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulator</div>
</div>

<div class="mt-10 text-center text-xl">
Same loop as 2017. Same loop as Materia. <strong>Only the first box changed.</strong>
</div>

---


# I went to squash 🏸

- 🚀 **[PLACEHOLDER: month]**: I launch a big job, close the laptop, go play. When I come back, it is **done**. Since then the chores run at night: JDK bumps, coding style, library swaps
- 🔄 Then Materia, the entire database, rewritten in **four days**. **[PLACEHOLDER: when, from what to what]**
- 🎲 Not magic: **years of tests and simulation** said, at every step, whether it was still right
- 🗑️ Code became **disposable**. What survives a rewrite: the tests, the invariants, the seeds

<div class="mt-8 text-center text-lg">
Code is the cheap part. <strong>The loop is the capital.</strong> The pager made me build it long before AI.
</div>
---

# I used to craft everything by hand 🏭

<div class="border-2 border-dashed border-current rounded-lg h-64 grid place-items-center opacity-60 mt-2">
  drop <code>public/factorio-handcraft.png</code> here
</div>

<div class="mt-6 text-center text-xl">
Now I'm starting to build <strong>small factories</strong>.
</div>

---

# A factory needs its own inspector 🏭

- 🔧 In a workshop, I inspect **every gear** by hand
- 🏭 In a factory, the line **rejects the bad gear by itself**. Nobody looks
- 🔁 The loop is the inspector: compiler, tests, fakes, simulator. The cheapest one is the language that **says no the fastest**
- 🐌 Without it, an agent is a faster typist and **I am the bottleneck**, reading every diff

<div class="mt-8 text-center text-xl">
You cannot automate a factory <strong>that has no inspector</strong>.<br>
<span class="text-lg">Nothing new. The pager taught me that in 2017.</span>
</div>

---

# You own what ships under your name 📏

- 🧪 Throwaway code can be a **black box**. If it breaks, nothing burns
- 🏭 Production code written by an agent needs a **higher bar** than a human's: lint, tests, fuzzers every night, automated reviews
- 🔦 "Claude wrote it, I don't know how it works" means **"I can't debug it"**
- 🎯 On hard problems, **two thirds** of its assumptions are wrong. Debugging stayed mine

<div class="mt-8 text-center text-xl">
If you can't debug it, <strong>you can't own it</strong>.
</div>

---

# Reserved for Google. Built on my evenings 🏝️

- 📜 Paxos: machines agreeing while some crash. Lamport wrote it in **a few paragraphs**, the rest is folklore, reserved for **Google, Meta, AWS**
- 🤖 So I built one on my evenings: paros, **three months**, entirely by agents, never for production. That is the point
- 🗳️ Three nodes, **no leader, ever**. One node too busy with its clients to ever say hello to its peers
- 🎲 One line to fix. Found by a night of seeds, on code wrong **since day one**

<div class="mt-8 text-center text-lg">
Nobody paged. I did not find it. <strong>What was out of reach is now one person and a few evenings away.</strong>
</div>
---

# I don't know C++, I know which failure I want 🤷

- 🦀 I contribute to FoundationDB, in C++, and **I don't know C++**. I can read it. I know **which failure I want**
- ✅ Two contributions merged upstream, written with an agent
- 🔴 One PR stayed **red for [PLACEHOLDER: how long]**: my code was not **reboot-proof**
- 😅 **[PLACEHOLDER: how you found out, what you changed]**

<div class="mt-8 text-center text-lg">
The simulator treated me <strong>exactly the way it treats Claude</strong>.
</div>

---

# Not a silver bullet 🙅

- 🎯 **Two thirds wrong on hard problems.** The agent proposes, you still have to know when it is lying
- 🧱 **Some layers have no loop yet.** Kernel, network switches: no simulator, no fast feedback
- 🗑️ **You throw a lot away.** Whole subsystems, whole test suites, when the first draft was wrong

<div class="mt-8 text-center text-lg">
Every one of these is <strong>a loop nobody has built yet</strong>. That is the next ten years of work.
</div>

---

# Remember the pager? 📟

<div class="mt-10 text-center text-2xl leading-relaxed">

A partition, full disks, a reboot, a bug during recovery.

That exact combination is **a seed**.

Found at night, by a machine, <strong>while I play squash</strong>.

</div>

---

# The niche is the new standard 📈

**Every level was a niche technique. It is now the standard for anyone running agents. Start anywhere.**

| Level | What to do | What you get |
|---|---|---|
| **1** ✅ | Real tests, whatever your language | The agent has a loop at all |
| **2** 🎰 | Property-based testing | You test what you did not imagine |
| **3** 🎭 | Fakes, not mocks | Fast, deterministic tests of your dependencies |
| **4** 😈 | Fakes that fight back | Failures on every run |
| **5** 🎲 | Seed-driven simulation | Bugs found while you play squash |

---

# Same job, new era 🚀

- ☕ I still turn coffee into software. The machine does the night shift
- 📖 I still read code for the shape. Now it is **diffs and seeds**, a whole system in minutes
- 📐 I still write **what must always be true**, and build the loop that says "wrong" to whoever types
- 🧗 I still do it because it's hard. For the first time, **the hard part is all that is left**

<div class="mt-8 text-center text-xl">
The niche I lived in became the standard. My job did not change.<br><strong>The era did, and it got a lot more interesting.</strong>
</div>

---
layout: end
---

# Thank you! 🙏

any questions?

<div class="flex items-center justify-center gap-12 mt-6">
  <div class="flex flex-col gap-3 text-left text-lg">
    <div>🤝 Come talk to us at <strong>FinistDevs</strong>, the Brest dev meetup</div>
    <div>🔗 Slides and stories on <a href="https://pierrezemb.fr/">pierrezemb.fr</a></div>
    <div>🌊 <a href="https://github.com/PierreZ/moonpool">moonpool</a>, a simulator for distributed systems in Rust</div>
    <div>🏝️ <a href="https://github.com/PierreZ/paros">paros</a>, Paxos built inside it, 100% by agents</div>
  </div>
</div>
