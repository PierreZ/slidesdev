---
theme: slidev-theme-pz
title: "My new job as a software engineer"
layout: cover
themeConfig:
  accent: '#1E6FD9'
  secondary: '#134A94'
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

# A repo that died twice 🪦

| When | What happened |
|---|---|
| 🌱 **May 2024** | I start `moonpool`, a simulator for distributed systems in Rust. **7 commits.** Then nothing. |
| 🍂 **March 2025** | Back in engineering after two years of management. I try again. **3 commits.** Dead again. |

<div class="mt-8 text-center text-lg">
On my evenings alone, this would have taken <strong>years</strong>.
</div>

---

# 18 August 2025: five commits in one day 🔁

The fifth one carries a line I had never seen in my own git log:

```text
feat: add moonpool simulation framework foundation

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>
```

<div class="mt-6">

That same week, I was still telling people: **[PLACEHOLDER: the skeptic sentence, who heard it, when]**

</div>

---

# One year later 📈

| Project | Commits | Lines of Rust | Written with an agent |
|---|---|---|---|
| 🌊 [moonpool](https://github.com/PierreZ/moonpool) | **714** | **70,392** | 61% of commits co-authored |
| 🏝️ [paros](https://github.com/PierreZ/paros) | **192** in three months | **61,279** | **100%**, Claude or Codex |

<div class="mt-8 text-center text-xl">
I typed <strong>almost none of it</strong>. And I have never had more fun. 🎉
</div>

---
layout: center
---

# How did a skeptic get here? 🤔

---

# Coffee and nights into anything ☕

> The programmer, like the poet, works only slightly removed from pure thought-stuff. He builds his castles in the air, from air.

*Fred Brooks, The Mythical Man-Month, 1975*

- 🎓 Engineering school: the **superpower** of turning coffee and night time into anything
- 🧠 I coded for the exercise, for the pleasure of the puzzle
- 🌙 **[PLACEHOLDER: one thing built in one night at school, with the year]**

---

# I read code for the shape, not the details 📖

- 📚 Early in my career I read a lot of code I would never touch: **[PLACEHOLDER: HBase, Kafka, FDB...]**
- 🔍 I did not care about the details, I wanted to understand **how it was cut**
- 🧩 You never remember the details of a codebase. You remember the **technique and the split**

<div class="mt-8 text-center opacity-70">
Keep that in mind. It comes back at the end.
</div>

---

# Then I got a pager 📟

- 🌐 A violent network partition hits a **70+ node Apache Hadoop cluster**
- 🐢 The cluster tries to heal, fills its disks, and **flips on its back like a turtle**
- ☕ We reboot. **`NullPointerException` at startup**, on all 70 machines
- 🩹 Known bug, fixed upstream. We **backport, recompile, redeploy**, under fire 😱
- ⏰ It hit us at the **worst moment**: during recovery

---

# The pager changed what I valued 🎯

> Nothing teaches you what your code actually does like getting paged by it at 3 a.m.

- ☕ Before: I coded to code. The **intellectual exercise** was the reward
- 📟 After: I wanted code that **behaves when everything goes wrong**. Correctness became the value
- 🧗 Today my motto is simple: **I do it because it's hard**

---

# Every writer needs a loop 🔁

<div class="flex items-center justify-center gap-8 mt-12">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">🧑‍💻 Me</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">📟 Pager, 3 a.m.</div>
</div>

<div class="mt-10 text-center text-lg">
Someone writes code. Something tells them <strong>when they were wrong</strong>. That something taught me everything I know.
</div>

---

# "It's my code, I trust it" 🙅

> We said "good enough" because we wrote it, we understood it, we tried it. AI broke all three.

*Steve Klabnik, [BugBash 2026](https://pierrezemb.fr/posts/bugbash-2026/)*

<div class="mt-6">

Every developer in this room has felt this. Let me tell you why it was **never true**.

</div>

---

# Who has read the kernel? 🐧

- 📟 When you are on call, you run code that is **not yours**: the kernel, the distribution, the database, the JVM
- 🙋 Raise your hand if you have read the Linux kernel. *(nobody)*
- 😴 And yet you sleep at night. Because it is **tested and operated**, not because you wrote it

<div class="mt-8 text-center text-lg">
We never trusted code because we wrote it. <strong>AI did not break that. It made it visible.</strong>
</div>

---
layout: center
---

# Codex, Claude, or the new hire: same problem 🎯

It has to **work in production**, and behave **when things go wrong**.

---

# Software's first act is over 🎬

> The cost of turning written business logic into code has dropped to zero. Or, at best, near-zero.

> Software's first act is over. The second act won't go like anybody expects, but it'll be more interesting, more economically valuable, and more mentally stimulating than we can imagine.

*Marc Brooker, [You Are Here](https://brooker.co.za/blog/2026/02/07/you-are-here.html), February 2026*

---

# Where my week went 📊

<div class="mt-8 space-y-8">
  <div>
    <div class="mb-1 font-semibold">Before</div>
    <div class="flex h-12 rounded-lg overflow-hidden text-sm">
      <div class="flex items-center justify-center text-white" style="width:85%; background: var(--theme-accent);">⌨️ typing code, 85%</div>
      <div class="flex items-center justify-center border-2 border-current opacity-60" style="width:15%;">the rest</div>
    </div>
  </div>
  <div>
    <div class="mb-1 font-semibold">Now</div>
    <div class="flex h-12 rounded-lg overflow-hidden text-sm">
      <div class="flex items-center justify-center border-2 border-current opacity-60" style="width:6%;">⌨️</div>
      <div class="flex items-center justify-center text-white" style="width:30%; background: var(--theme-accent);">📝 specs</div>
      <div class="flex items-center justify-center text-white" style="width:34%; background: var(--theme-accent); opacity:0.85;">✅ quality tests</div>
      <div class="flex items-center justify-center text-white" style="width:30%; background: var(--theme-accent); opacity:0.7;">🎯 hunting for correctness</div>
    </div>
  </div>
</div>

<div class="mt-8 text-center">
The typing half vanished. What is left is the half <strong>I already preferred</strong>. <span class="opacity-60">[PLACEHOLDER: real split if you have one]</span>
</div>

---

# We give juniors a pager 🧑‍🎓

<div class="flex items-center justify-center gap-8 mt-10">
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🧑‍🎓 New hire</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold">📟 Pager</div>
</div>

<div class="mt-8">

- 📟 The fastest way to learn what your code does is to be **woken up by it**
- 🎲 Our newcomer on Materia shipped a deep feature **in one week**, edge cases included. The simulator taught him the rules nobody had written down

</div>

---

# A junior who read every thesis 🎓

<div class="flex items-center justify-center gap-8 mt-10">
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🤖 Claude</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-dashed border-current rounded-lg text-center font-semibold opacity-40">❓ nothing</div>
</div>

<div class="mt-8">

> It is a junior who has read every thesis.

*Quentin Adam, CEO Clever Cloud, 2026*

A junior who has read everything and has **never been woken up** is still a junior.

</div>

---

# Give it the simulator 🎲

<div class="flex items-center justify-center gap-8 mt-10">
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">🤖 Claude</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 border-current rounded-lg text-center font-semibold opacity-40">📦 Code</div>
  <div class="text-3xl opacity-40">→</div>
  <div class="px-8 py-6 border-2 rounded-lg text-center font-semibold" style="border-color: var(--theme-accent); color: var(--theme-accent);">🎲 Simulator</div>
</div>

<div class="mt-8">

- 🎯 You test what you **imagine**. Bugs hide in combinations you didn't. Claude imagines too, just with more reading
- 🎲 A simulator does not imagine. It **rolls dice**: network splits, disk failures, clock skew, reboots, all seeded, all replayable
- 🔁 **You don't trust Claude. You trust the simulator.**

</div>

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---

# I went to squash 🏸

- 🚀 **[PLACEHOLDER: month, year]**: I launch a big job. **[PLACEHOLDER: what it was]**
- 🏸 I close the laptop and go play squash. **[PLACEHOLDER: how long]**
- 😳 When I come back, it is **done**. Tests green, simulation green
- ☕ At school I turned coffee and nights into software. That evening, **the machine did the night shift**

---

# Paxos: easy to read, a chasm to build 🏝️

> There is no paper called "Multi-Paxos."

> It is not a gap. It is a chasm, and the bodies of failed implementations line the bottom.

*[The Agony of Consensus Algorithms, ch. 6](https://cloudstreet-dev.github.io/The-Agony-of-Consensus-Algorithms/ch06-multi-paxos.html)*

- 📜 Lamport sketched it in a few paragraphs. The rest is **hallway folklore**
- 🕳️ What the paper leaves out: leader election, log gaps, membership changes, **snapshots**, disk corruption
- 🏢 Google found it "extremely difficult" to implement from the paper. This was **reserved for Google, Meta, AWS**

---

# paros: a core with no IO, a simulator that hunts 🔍

- 🧠 **A core with no IO**, easy to re-read: no clock, no network, no randomness. Events in, decisions out
- 🎲 The same code runs on real TCP in prod and **inside moonpool** in tests, bit-for-bit replayable
- 🔍 Every hunt: **2,000 to 3,000 seeds**. Every state transition audited by about **75 checks**
- 🤖 **192 commits, 61,279 lines, 272 tests.** Written 100% by Claude and Codex, on my evenings, since 15 June 2026

<div class="mt-6 text-center opacity-70">
Named after my favourite Greek island. Two islands, one parliament. 🏝️
</div>

---

# Seed 17898267817771645730 🎯

*8 September 2026, a 2,000-seed hunt on paros*

- 🗳️ Three nodes, **no leader elected for 60 seconds**
- 🌊 One node answered **18,451** client requests on one connection
- 🚪 ...and cancelled **14,497** incoming peer connections without completing a single one
- 🔧 Root cause: a `select!` loop dropping its `accept()` future on every pass. Fix: **one `Box::pin`**

<div class="mt-6 text-center text-lg">
Pre-existing bug. Nobody paged. Nobody woke up. <strong>I did not find it.</strong>
</div>

---

# The simulator doesn't care who you are 🤷

- 🦀 I contribute to FoundationDB, in C++, and **I don't know C++**. I can read it. I know exactly **what failure I want**
- ✅ [Pure C workload API](https://github.com/apple/foundationdb/pull/11288), [delay()](https://github.com/apple/foundationdb/pull/12357), merged upstream
- 🔴 One of my PRs stayed **red for [PLACEHOLDER: how long]**: my code was not **reboot-proof**
- 😅 The simulator treated me exactly the way it treats Claude. **[PLACEHOLDER: how you found out, what you changed]**

<div class="border-2 border-dashed border-current rounded-lg h-28 grid place-items-center opacity-60 mt-4">
  drop <code>public/fdb-pr-red.png</code> here
</div>

---

# What I write now: invariants 📜

- 🗳️ Leader election on FoundationDB, February 2026: I did not write the code, I pointed Claude at my post on workloads and said **"apply these patterns"**
- 📐 It proposed **13 invariants**: one leader at a time, fencing tokens only go up, one value per ballot...
- 🎲 The simulator ran them under clock skew of **±1 second**, resignations, partitions. Weeks of work, **hours of review**
- 🔁 **The LLM proposes, the simulation disposes.** ([post](https://pierrezemb.fr/posts/simulating-leader-election-on-foundationdb/))

<div class="mt-6 text-center">
I used to write code. Now I write <strong>what must always be true</strong>, and the machine that checks it.
</div>

---

# Not a silver bullet 🙅

- 📋 **The copy-paste era is real.** moonpool is honestly two simulators glued together, FoundationDB's and TigerBeetle's. It works. It is not innovation yet
- 🗑️ **You throw a lot away.** March 2026: I deleted a 6,000-line actor system and rewrote a test suite whose assertions were "structurally impossible"
- 🧱 **Some layers resist.** Our kernel and switch OS people say the AI slows them down. No dataset, no feedback loop, no magic
- ⚠️ **paros is not in production** and will not be. That is the point of a learning project

---

# Software quality will dip. Plan for it 🛡️

- 📉 More code, written faster, by more people and more agents. Some of it **will be worse**
- 🔥 Which means the job shifts: **handle failures better**, not just write fewer bugs
- 🎲 Nothing beats a simulator for proving your code **works despite failures**: partitions, crashes, corruption, clock skew, at every commit

<div class="mt-8 text-center text-lg">
The first act was about writing software. <strong>The second act is about software that survives.</strong>
</div>

---

# Invest in correctness, whatever you write 🪜

**Start anywhere. Each level adds value, in any language, for any job.**

| Level | What to do | What you get |
|---|---|---|
| **1** ✅ | Unit tests, real ones | The agent has a loop at all |
| **2** 🎰 | Property-based testing | You test what you did not imagine |
| **3** 🎭 | Fakes, not mocks | Fast, deterministic tests of your dependencies |
| **4** 😈 | Fakes that fight back | Failures on every run |
| **5** 🎲 | Seed-driven simulation | Reproducible bugs, found while you play squash |

---

# Features that work 🎉

- 🪦 The repo that died twice is alive. **Two of them.** Together they found bugs in software at Clever Cloud **[PLACEHOLDER: which]**
- ☕ I still turn coffee into software. I just do not do the night shift anymore
- 📖 I still read code for the shape. Now I read **diffs and seeds**
- 🧗 I do it because it's hard. And for the first time, the hard part is **all that is left**

<div class="mt-8 text-center text-xl">
More fun engineering than at any point in my career. 🚀
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
