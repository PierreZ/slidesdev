---
theme: slidev-theme-pz
title: "Same job, only more interesting"
themeConfig:
  accent: '#8A3FC9'
  secondary: '#B85E1F'
layout: cover
class: nuit-cover
---

# Software engineer:<br>same job, only more interesting <span class="rocket">🚀</span>

Pierre Zemb, FinistDevs

La Nuit des Communautés Bretonne #3

17 September 2026 🌙

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
.nuit-cover h1 .rocket {
  color: #ffffff !important;
}
</style>

---

# $ whoami 👋

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🛠️ Staff engineer at Clever Cloud, working on **distributed systems**
  - 📟 Building, contributing, debugging, **getting paged**
- 🦀 Maintaining [foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs) and a few other Rust libraries
- 🏸 Squash player
- 🤝 Co-organizing **FinistDevs**

</div>
<img src="https://pierrezemb.fr/images/myself.jpg" class="w-40 h-40 rounded-lg object-cover" />
</div>

---

<div class="grid place-items-center mb-4">
  <img src="https://finistdevs.org/img/uploads/2023/11/cropped-logo_FinistDevs_bandeau.png" class="max-h-24" />
</div>

- 🎂 Born as **FinistJUG** in December 2011
- 🌙 **130+ evenings** since, on Java, cloud, DevOps, web, and whatever a member wants to share
- 👥 Three organizers: Horacio, Stéphanie, and me
- 🎤 **We are looking for speakers.** Never given a talk? Perfect, we help you prepare

<div class="mt-6 flex items-center justify-center gap-6">
  <img src="/finistdevs-qr.png" class="w-28 h-28 rounded-lg" />
  <div class="text-xl text-left">
    📱 <a href="https://www.meetup.com/finistdevs">meetup.com/finistdevs</a><br>
    🔗 <a href="https://finistdevs.org">finistdevs.org</a>
  </div>
</div>

---

# 2010: I coded for the puzzle ☕

- 🎓 Engineering school in Brest: I came for electronics, I found **C**, and many other things
- ☕ I found out I could turn **coffee into software**, and I never stopped
- 🏗️ Part-time internships, always in **infrastructure teams**
  - 🌐 That is where I met **distributed systems**: hard, everywhere, so much to learn
  - 📖 The bible: *[Designing Data-Intensive Applications](https://dataintensive.net/)*, Martin Kleppmann

---

# 2017: then I got a pager 📟


<div class="grid place-items-center mt-2">
  <img src="https://media1.tenor.com/m/MYZgsN2TDJAAAAAC/this-is.gif" class="rounded-lg max-h-80" />
</div>

---

# The world is worse than your tests 📚

You do not have to believe me (or your SRE colleague). Believe the literature:

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

---

# What the pager taught me 📟

- 👀 Nothing teaches you a system like **watching it misbehave in production**
  - 🤔 How do you fix a system you do not understand?
- 💥 Software **will fail**: sometimes loudly, often **silently**
- 🫣 The failures that hurt are the ones **nobody imagined** while writing the code
- 🔭 You cannot fix what you cannot see: **observability first**

<div class="mt-4 text-sm">
<em><a href="https://www.youtube.com/watch?v=ivnI1BKywW4&t=603s" target="_blank">Développer des applications observables pour la production</a>, Devoxx France</em>
</div>

---

# Down the rabbit hole of correctness 🕳️

- ✈️ Some software cannot afford the pager: aerospace, health, **distributed databases**, ...
- 🛡️ So those teams built stronger harnesses: guardrails, fault injection, whole testing strategies
- 📚 Sixty years of techniques, refined in a niche: **types, property-based testing, fuzzing, model checking, deterministic simulation testing (DST), proofs**

<div class="mt-6 text-center text-xl">
Distributed systems live down that hole. <strong>That is where I went.</strong>
</div>

---

# Jepsen breaks databases for a living 🔨

<div class="grid place-items-center">
  <a href="https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2" target="_blank">
    <img src="/jepsen-report.png" class="max-h-80 rounded shadow" />
  </a>
</div>

<div class="mt-2 text-center text-lg">
Since 2013: MongoDB, Redis, Cassandra, Kafka, etcd, CockroachDB... <strong>almost none came out clean</strong>
</div>

---

# The one database Jepsen never bothered with 🍎

> "haven't tested foundation[DB] in part because their testing appears to be waaaay more rigorous than mine"

*Kyle Kingsbury, [aphyr](https://aphyr.com/), 2013*

- 🎲 FoundationDB wrote its **simulator before the database**

<div class="flex justify-center mt-2">
  <div class="flex flex-col items-stretch w-[30rem]">
    <div class="px-6 py-3 border-2 border-current rounded-t-lg text-center font-bold">FoundationDB server code</div>
    <div class="flex">
      <div class="flex-1 px-4 py-3 border-2 border-t-0 border-current rounded-bl-lg text-center opacity-40">
        <div class="font-bold">Production</div>
        <div class="text-sm">real network, disks, clocks</div>
      </div>
      <div class="flex-1 px-4 py-3 border-2 border-t-0 border-l-0 rounded-br-lg text-center" style="border-color: var(--theme-accent); color: var(--theme-accent);">
        <div class="font-bold">Simulation</div>
        <div class="text-sm">fake network, disks, clocks, crashes</div>
      </div>
    </div>
  </div>
</div>

- 🍎 **4,000 years** of simulated failures a year, at Apple
- 🧰 Years later, in production: **the sanest distributed system I have operated**

---

<img src="/materia-sim-single.png" class="w-full rounded shadow" />

---

<img src="/materia-sim-triple.png" class="w-full rounded shadow" />

---

# DST for building distributed systems 📺

<div class="grid place-items-center mt-2">
  <a href="https://www.youtube.com/watch?v=U3m7yFvc598&t=1336s" target="_blank">
    <img src="https://img.youtube.com/vi/U3m7yFvc598/maxresdefault.jpg" class="rounded-lg shadow max-h-72" />
  </a>
</div>

<div class="mt-4 text-center text-lg">
<em><a href="https://www.youtube.com/watch?v=U3m7yFvc598&t=1336s" target="_blank">Et si on faisait du simulation-driven development ?</a></em>, Devoxx France
</div>

---

# Yes, DST can even beat Mario 🎮

<div class="grid place-items-center">
  <img src="/mario.png" class="rounded-lg shadow max-h-56" />
</div>

- 🎲 [Antithesis](https://antithesis.com/) beat Super Mario Bros with **random inputs only**. No human, no script
- 🎯 Point that machine at your software and it finds **what you never thought to test**

<div class="mt-2 text-sm">
<em><a href="https://www.youtube.com/watch?v=m3HwXlQPCEU" target="_blank">Testing a Single-Node, Single Threaded, Distributed System Written in 1985</a>, Will Wilson</em>
</div>

---

# 2024: what was I actually paid for? 🤔

- ☕ Turning coffee into software: **less of the week than you would think**
- 📖 Reading **other people's code**, papers and logs
- 📟 Getting paged, then finding out **what I had missed**
- 🧗 The hard part: **understanding the system** well enough to change it

<div class="mt-6 text-center text-xl">
The job was never typing. It was <strong>finding out what I missed before the users did.</strong>
</div>

---

# Then the LLMs showed up 🤖

Before LLMs, we trusted code because we:

- 🧠 understood it
- ✍️ wrote it
- 🧪 tried it
- 📟 got paged by it

<div class="mt-4 text-center text-lg">
AI broke the first three. The only feedback left is the <strong>worst</strong> one: the pager.
</div>

---

# "We no longer pay developers to write code" 📺

That is **my own CEO**, Quentin Adam.

<div class="grid place-items-center mt-2">
  <a href="https://www.youtube.com/watch?v=AiytemqB_F0" target="_blank">
    <img src="https://img.youtube.com/vi/AiytemqB_F0/maxresdefault.jpg" class="rounded-lg shadow max-h-72" />
  </a>
</div>

<div class="mt-4 text-center text-lg">
<em><a href="https://www.youtube.com/watch?v=AiytemqB_F0" target="_blank">On ne paie plus les développeurs pour écrire du code</a></em>, Underscore_
</div>

---

# Same model, not the same code 😬

- 🏢 Clever Cloud: **~100 employees, 70 developers**
- 📉 The quality of the generated code varied **wildly** from one project to the next
- ✅ Three kinds of projects where AI **just worked**:
  - 🦀 written in **Rust**
  - 🧪 with **lots of tests**
  - 🎲 or **stronger tests**: property-based testing, **DST**

---

# DST: the ultimate LLM feedback loop 🤖🔁

<div class="grid place-items-center mt-2">
  <img src="/boris-cherny-tweet.png" class="rounded shadow max-h-80" />
</div>

---

<div class="flex flex-col items-center gap-6 pt-8">
  <img src="/claude-moonpool.png" class="w-full rounded shadow" />
  <div class="flex flex-wrap justify-center items-center gap-2 text-sm">
    <div class="px-3 py-2 border-2 border-current rounded whitespace-nowrap">🤖 LLM writes code</div>
    <div>→</div>
    <div class="px-3 py-2 border-2 border-current rounded whitespace-nowrap">🧪 Simulation finds bug</div>
    <div>→</div>
    <div class="px-3 py-2 border-2 border-current rounded whitespace-nowrap">🔍 LLM reads failing seed</div>
    <div>→</div>
    <div class="px-3 py-2 border-2 border-current rounded whitespace-nowrap">🔧 LLM fixes code</div>
    <div class="text-xl">🔁</div>
  </div>
</div>

---


# So, what is changing? ⏳

Few of us ever had time for **software quality**. Now that code costs nothing, **the time is back**.

- 🎯 I am the **architect, not the typist**
- 🔁 I give the agent a **fast feedback loop**
- 🛡️ Tests, simulation, property-based testing: **the correctness work now outweighs the code**
- 📜 Contracts, specifications and invariants are **the capital**

```gherkin
# Gherkin: requirements the agent can run
Scenario: a guest cannot pay with a saved card
  Given a guest user with a saved card
  When they check out with that card
  Then the payment is refused
```

---

# I used to craft everything by hand 🏭

<div class="grid place-items-center">
  <img src="/factorio-handcraft.png" class="rounded-lg shadow max-h-88" />
</div>

---

# We can automate! 🏸

<div class="absolute inset-0 grid place-items-center pt-16 pb-12">
  <img src="/squash-agents.png" class="rounded-lg shadow max-h-96" />
</div>

---

# Do we automate all code? 🤔

**No.** What is left: specs, success criteria, feedback loops, QA, code review.

*Simon Willison, [Vibe engineering](https://simonwillison.net/2025/Oct/7/vibe-engineering/), 2025*

- 🔪 [Code like a surgeon](https://www.geoffreylitt.com/2025/10/24/code-like-a-surgeon): the agent preps, **I keep the scalpel**

---

# Same job, only more interesting 🚀

- ☕ I still turn coffee into software, **a bit faster, with way more quality**
- 📖 Reading code and papers has **never been easier**
- 📟 I still get paged, but first I **torture software in conditions worse than production**
- 🧗 **The hard part is all that is left**
- 🔥 More **fun** than ever: never about writing code, always about **building** and **learning**

<div class="mt-4 text-sm">
<em><a href="https://antirez.com/news/158" target="_blank">Don't fall into the anti-AI hype</a>, antirez</em>
</div>

---

# Make correctness your goal 🎯

- 📏 Production code written by an agent needs a **higher bar** than a human's
- 🎲 DST pays off when you **rebuild from scratch**, like we did at Clever Cloud.<br>The rest of the toolbox is **less invasive**
- 📈 Every technique was niche. **It is now the standard for anyone running agents**
- 🧭 While everyone is talking about speed, **talk about correctness and quality**

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
