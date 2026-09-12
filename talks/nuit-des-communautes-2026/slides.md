---
theme: slidev-theme-pz
title: "My new job as a software engineer"
themeConfig:
  accent: '#1E6FD9'
  secondary: '#134A94'
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
---

# My new job as<br>a software engineer 🚀

Pierre Zemb, FinistDevs

Nuit des Communautés 2026, Brest 🌙

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

- 🌙 A night, a coffee, and in the morning **something exists** that did not
- 🧠 Six months to understand pointers. The compiler and valgrind were the first things that **told me I was wrong**

</div>

<div class="mt-6 text-center text-xl">
I coded <strong>to code</strong>. The puzzle was the reward.
</div>

---

# Why distributed systems? 🌐

- 🧗 They are **hard**, so they are interesting
- 🌍 They are **everywhere**: your bank, your phone, the ticket you scanned tonight
- 📖 Because they are hard, there is **a lot to learn** from them

<div class="mt-10 text-center text-xl">
One machine was a puzzle. <strong>Seventy machines was a calling.</strong>
</div>

---

# 2015: then I got a pager 📟

OVH Metrics: **800 servers**, 1.8M points per second. On-call was our test suite.

- 🌐 A violent network partition hits a **70-node Apache Hadoop cluster**
- 🐢 The cluster tries to heal, fills its disks, and **flips on its back like a turtle**
- ☕ We reboot. **`NullPointerException` at startup**, on all 70 machines
- 🩹 Known bug, fixed upstream. Backport, recompile, redeploy, **under fire** 😱
- ⏰ It hit us at the **worst moment**: during recovery

---

# So, what went wrong? 🤔

<div class="flex items-center justify-center gap-8 mt-6">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🧑‍💻 Me</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">📟 Pager, 3 a.m.</div>
</div>

<div class="mt-8">

- ☀️ The code was right **on a sunny day**
- 🌧️ Nobody had asked it what happens **during recovery**

</div>

<div class="mt-6 text-center text-xl">
My job became <strong>finding out I'm wrong before the users do</strong>.
</div>

---

# What the pager taught me 🎓

- 🤷 Mostly that I **did not know a lot of things**. Every page was a combination I had never imagined
- 🔭 But it taught me some: **observability**, because you cannot fix what you cannot see
- 🧯 And **errors**, because the sunny-day path is the easy half of the code

<div class="mt-8 text-center text-lg">
Tests check what I imagined. <strong>The pager found the rest.</strong>
</div>

<div class="mt-6 text-center text-xl" style="color: var(--theme-accent);">
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

</div>

<div class="mt-6 text-center text-xl">
Same move, every job: <strong>compiler, pager, simulator</strong>. Each one faster than the last.
</div>
---

# My team was not convinced 🏢

- 🗄️ Clever Cloud, Materia: a database built on FoundationDB, **simulation-first**
- 🥱 At first, boring bugs. Then each engineer found a bug **in their own code**
- 🧑‍🎓 A newcomer shipped a deep feature in **one week**, edge cases included. The simulator taught him the rules nobody had written down

<div class="mt-10 text-center text-xl">
Not test-driven development. <strong>Simulation-driven development.</strong>
</div>

---

# You don't trust Claude, you trust the simulator 🎯

<div class="flex items-center justify-center gap-8 mt-6">
  <div class="px-8 py-6 border-2 border-dashed border-current rounded-lg text-center font-semibold">🤖 Claude</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulator</div>
</div>

<div class="mt-8">

- 🦜 **[PLACEHOLDER: your skeptic sentence about LLMs, to whom, when]**
- ⚡ Then ten minutes with Windsurf on a stuck contribution: it read the code **for the shape**, the way it took me ten years to learn
- 🎲 Same model on Materia: outside the simulator, a parrot. Inside, **very, very good**

</div>

<div class="mt-6 text-center text-xl">
It was not the model. <strong>It was the loop.</strong> The writer changed, the loop did not.
</div>
---

# I went to squash 🏸

- 🚀 **[PLACEHOLDER: month]**: I launch a big job, close the laptop, go play. When I come back, it is **done**
- 🌙 Since then the chores nobody wants run at night: **JDK bumps, coding style, library swaps**
- 🔄 Then Materia, the entire database, rewritten in **four days**. **[PLACEHOLDER: when, from what to what]**
- 🎲 Not magic: **years of tests and simulation** said, at every step, whether it was still right

<div class="mt-8 text-center text-lg">
Code is the cheap part. <strong>The loop is the capital.</strong> The pager made me build it long before AI.
</div>
---

# Pick the language that talks back ⚡

- ⌨️ When typing no longer costs anything, pick the language for the **end product**
- 🦀 Rust pays twice: early, precise feedback, **for me and for the agent**
- 🔁 Compiler, Clippy, tests: a loop that answers **in seconds**, not at 3 a.m.

<div class="mt-10 text-center text-xl">
The best language for an agent is the one that <strong>says no the fastest</strong>.
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
layout: two-cols
---

::title::

# Someone wrote the factory manual 📜

::default::

**The manual says** *([Frontier engineering](https://kiro.dev/topics/frontier-engineering/))*

- 🔁 Give agents a **fast feedback loop**
- 📏 Hold AI output to **human standards**
- 🚧 Trust the **boundaries**, not the agent
- 🗑️ Code is disposable, **boundary tests are not**

::right::

**The pager said**

- 📟 2015: the pager was our test suite
- 🔧 2018: the repair tool that moved data out
- 🎲 2021: a simulator you trust, a writer you don't
- 🎯 Invariants and seeds outlive every rewrite

<div class="mt-6 text-center text-lg">
Nothing new. <strong>Just named.</strong>
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

# The bug I never looked for 🏝️

- 📜 Paxos: machines agreeing while some crash. Lamport wrote it in **a few paragraphs**, the rest is folklore, reserved for **Google, Meta, AWS**
- 🤖 So I built one on my evenings: paros, **three months**, entirely by agents, never for production. That is the point
- 🗳️ Three nodes, **no leader, ever**. One node too busy with its clients to ever say hello to its peers
- 🎲 One line to fix. Found by a night of seeds, on code wrong **since day one**

<div class="mt-8 text-center text-lg">
Nobody paged. Nobody woke up. <strong>I did not find it.</strong>
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

# Remember the turtle? 🐢

<div class="mt-10 text-center text-2xl leading-relaxed">

A partition, full disks, a reboot, a bug during recovery.

That exact combination is **a seed**.

Found at night, by a machine, <strong>while I play squash</strong>.

</div>

---

# Build your loop 🔁

**Whatever your language, whatever your job. Start anywhere, each level adds value.**

| Level | What to do | What you get |
|---|---|---|
| **1** ✅ | Real tests, whatever your language | The agent has a loop at all |
| **2** 🎰 | Property-based testing | You test what you did not imagine |
| **3** 🎭 | Fakes, not mocks | Fast, deterministic tests of your dependencies |
| **4** 😈 | Fakes that fight back | Failures on every run |
| **5** 🎲 | Seed-driven simulation | Bugs found while you play squash |

---

# Same job, new tools 🚀

- ☕ I still turn coffee into software. The machine does the night shift
- 📖 I still read code for the shape. Now it is **diffs and seeds**, a whole system in minutes
- 📐 I still write **what must always be true**, and build the loop that says "wrong" to whoever types
- 🧗 I still do it because it's hard. For the first time, **the hard part is all that is left**

<div class="mt-8 text-center text-xl">
What distributed systems taught me is now every developer's job.<br>My new job hasn't changed much. <strong>It is only more fun.</strong>
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
