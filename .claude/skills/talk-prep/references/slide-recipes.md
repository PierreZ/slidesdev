# Slide recipes in Pierre's style

Copy, then adapt. All use the `slidev-theme-pz` theme. Accent comes from `themeConfig.accent`; `**bold**` renders in accent.

## Headmatter and cover

```md
---
theme: slidev-theme-pz
title: "What if we embraced simulation-driven development?"
layout: cover
themeConfig:
  accent: '#d63f49'
  secondary: '#901f26'
---

# What if we embraced<br>simulation-driven development? 🎲

Pierre Zemb, Staff Engineer @ Clever Cloud

Sunny Tech 2026 🦩
```

Title is the question or the promise, never the topic. Event name with an emoji on its own line.

## whoami and the gif

```md
# $ whoami 👋

<div class="grid grid-cols-[1fr_auto] gap-8 items-start">
<div>

- 🛠️ Staff engineer, working around **distributed systems**
  - 🐞 Building, contributing, debugging, **paging**…
- 🦀 Maintaining several OSS libraries in Rust ([foundationdb-rs](https://github.com/foundationdb-rs/foundationdb-rs))
- 🤝 Involved in local communities (**FinistDevs** / JUG)
- 🎾 Squash player

</div>
<img src="https://pierrezemb.fr/images/myself.jpg" class="w-40 h-40 rounded-lg object-cover" />
</div>

---

# Also me 🔥

<div class="absolute inset-0 grid place-items-center">
  <img src="https://media1.tenor.com/m/MYZgsN2TDJAAAAAC/this-is.gif" class="rounded-lg max-h-72" />
</div>
```

The squash line stays; the "sport, not git" joke is spoken.

## War story slide

```md
# One of my most "wait what" bugs 🤯

- 🌐 A tough network partition hit a **70+ node Apache Hadoop cluster**
- 🐢 The cluster could not restart
- ☕ A **`NullPointerException` at startup**, caused by its faulty state
- 🩹 Patched in a newer HDFS: we **backported** the patch and redeployed the jar 😱
- ⏰ We hit it at the **worst moment**, during recovery
```

Five beats maximum, one emoji each, the twist last. Follow with `# So, what went wrong? 🔍`.

## The motif: system and its two sources of chaos

Neutral state:

```md
# Your code meets two things 🌍

<div class="flex items-center justify-center gap-10 mt-12">
  <div class="px-10 py-12 border-2 border-current rounded-lg text-center font-semibold">🖥️ Your system</div>
  <div class="text-3xl opacity-40">↔</div>
  <div class="flex flex-col gap-6">
    <div class="px-8 py-3 border-2 border-current rounded-full text-center">👤 Your users</div>
    <div class="px-8 py-3 border-2 border-current rounded-full text-center">🌍 The world</div>
  </div>
</div>

Two sources of chaos. Let's start with the **users**.
```

Focused state: put `style="border-color: var(--theme-accent); color: var(--theme-accent);"` and `font-semibold` on the focused pill, `opacity-40` on the others and on the system box. Later states rename the pills ("🤖 Simulated users", "🌪️ Simulated world", "👤 Your users, but worse"). Keep the geometry identical across states so the audience sees the change, not a new diagram.

Any talk can have its own motif (a layer stack, a pipeline, a cluster). The rules are the same: introduce it neutral, modify it at least three times, never redraw it.

## Combinatorics table with a punchline

```md
# One checkout, so many paths 🛒

| Dimension | Options | Count |
|---|---|---|
| 👤 User type | Guest, Logged-in, Premium | 3 |
| 💳 Payment | Card, PayPal, Apple Pay, Gift Card | 4 |
| 🚚 Delivery | Standard, Express, Pickup | 3 |

<div class="text-center mt-6 text-lg font-mono">3 x 4 x 3 x 2 x 3 x 3 = <span class="font-bold" style="color: var(--theme-accent);">648</span> happy-path test cases 😱</div>
```

Then the same table on the next slide with one bolded addition per row and the bigger number with 💀.

## Wall of papers

```md
# The world is worse than your tests 📚

You do not have to believe me. Believe the literature:

| You believe... | The research says otherwise |
|---|---|
| 🌐 "The network is reliable" | [Network-Partitioning Failures, OSDI '18](url) |
| 💾 "My data is safe on disk" | [Data Corruption in the Storage Stack, FAST '08](url) |
```

For the long form, one slide per belief: quoted belief as h1, italic linked citation line, blockquotes with bold numbers, one line of commentary. Tables of this size need `style.css` from `assets/` to shrink font and padding.

## Code slide with a highlighted diff

````md
# Make the fake fight back 😈

```java
class FakeBus implements MessageBus {
    Map<String, List<Message>> queues = new HashMap<>();
    Random rng;

    public Future<Void> publish(String topic, Message msg) {
        sleep(rng.nextInt(500));                            // random lag
        float r = rng.nextFloat();
        if (r < 0.15) return failed(new ConnectionLost());  // never happened
        queues.get(topic).add(msg);                         // committed...
        if (r < 0.30) return failed(new ConnectionLost());  // ...ack lost!
        return completed();
    }
}
```

**Your system should still behave correctly!**
````

Under 15 lines, trailing comments carry the story, `{5-9}` after the language tag to highlight the new lines when the slide builds on the previous one.

## Two implementations, one interface

````md
---
layout: two-cols
---

::title::

# Two implementations, one interface

::default::

🗄️ **Production**

```java
class PostgresUserRepository implements UserRepository { ... }
```

::right::

🎭 **Simulation**

```java
class FakeUserRepository implements UserRepository { ... }
```

Same interface. One talks to Postgres. One lives in memory. **Your system can't tell the difference.**
````

## Breather and question slides

```md
# What do you believe about your system? 🤔
```

```md
---
layout: center
---

# Can we inject our code inside FoundationDB? 🤔
```

A title alone is a valid slide. He talks over it.

## Image-only slide

```md
<img src="/materia-sim-triple.png" class="w-full rounded shadow" />
```

No title, no layout. Screenshots of a TUI, a CI run, a tweet, a graph. Series of them in a row are fine (the FDB config walkthrough used six).

## Placeholder for an image he has not supplied

```md
<div class="border-2 border-dashed border-current rounded-lg h-72 grid place-items-center opacity-60">
  drop <code>public/days-since-npe.png</code> here
</div>
```

List every placeholder in the per-talk `CLAUDE.md`.

## Not a silver bullet

```md
# Not a silver bullet 🙅

- 📈 **Performance is invisible** in sim: you still need a perf farm
- 🕳️ **Verified fakes**: you must [ensure your fake matches the real thing](https://pierrezemb.fr/posts/designing-fakes-that-prove-correctness/)
- ⏳ **Bug-finding has latency**: a bug can hide in a seed for months
```

Three limits, each with the mitigation in the same line.

## Adoption ladder

```md
# How to adopt DST 📈

**Start anywhere. Each level adds value.**

| Level | What to do | What you get |
|---|---|---|
| **1** 🎰 | Random workload generation | Test unusual combinations |
| **2** ✅ | Property-based testing | Flush out your system spec |
| **3** 🎭 | Fakes | Fast, deterministic tests |
| **4** 😈 | Fault-injectable fakes | Discover edge cases |
| **5** 🎲 | Seed-driven DST | Reproducible bugs, autonomous discovery |
```

Emoji per level match the emoji used for that concept earlier in the deck.

## End slide

```md
---
layout: end
---

# Thank you! 🙏

any questions?

<div class="flex items-center justify-center gap-12 mt-6">
  <div class="flex flex-col items-center gap-2">
    <img src="/review-qr.png" class="w-44 h-44 rounded-lg" />
    <div class="text-sm">📝 Rate this talk</div>
  </div>
  <div class="flex flex-col gap-3 text-left text-lg">
    <div>🎈 Come say hi at the <strong>Clever Cloud booth</strong></div>
    <div>🔗 Everything is on <a href="https://pierrezemb.fr/">pierrezemb.fr</a></div>
    <div>🦀 <a href="https://github.com/PierreZ/moonpool">My own DST in Rust</a></div>
  </div>
</div>
```

Drop the QR block when the event has no rating app. Keep the booth line whenever Clever Cloud sponsors.
