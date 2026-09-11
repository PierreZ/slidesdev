---
name: talk-prep
description: Interactive co-writing session that takes one of Pierre's talks from a brief (event, duration, topic, audience) to a finished Slidev deck under talks/, in Pierre's own storytelling voice (on-call war story cold open, belief-busting titles, recurring diagram motif, sourced evidence, adoption ladder close). Use it whenever Pierre mentions a talk, a conference, a meetup, a lightning talk, a keynote, a CFP or abstract, a storyline or narrative for slides, reworking or shortening an existing talk for another event, or names an event (Sunny Tech, Devoxx, BugBash, FinistDevs, FOSDEM, Riviera DEV, a JUG...), even when he only says "j'ai un talk", "new deck", or "help me tell this story". Do not use for a one-off edit on a deck whose narrative is already settled (the slidev skill covers that).
---

# Talk prep: build the story with Pierre, then the deck

Pierre improvises on stage. He never reads slides and never uses presenter notes. The deck is a set of cue cards for a story he already knows how to tell, so the real work of this skill is the story: the war story that opens it, the belief it busts, the motif that carries it, the proof from his own team, the honest limits, the ladder the audience climbs afterwards. Slides come last and fall out of the narrative almost mechanically.

Run this as a conversation, one phase per exchange. Propose a concrete default, then ask. Do not write `slides.md` before the slide outline is locked.

Talk to Pierre in the language he writes in (French or English). Slides are always English regardless. When the brief already answers a question, restate it in one line and move on rather than asking again. A one-line teaser of the next phase at the end of a message is fine ("next: the cold open, I will propose the Hadoop NPE by default"), as long as the message stays on one phase.

## Read before starting

- `references/voice.md`: how Pierre talks and tells a story, extracted from six talk transcripts. Read it in full at the start of every session so the proposals sound like him.
- `references/story-library.md`: his reusable war stories, analogies, one-liners, numbers, evidence with links, and index of previous talks. Consult it during the cold open and evidence phases.
- `references/talk-formats.md`: slide budgets per duration and the real beat sheets of his past talks, including a worked example of cutting a 45 minute talk down. Consult it during the arc phase.
- `references/slide-recipes.md`: Markdown and Tailwind templates for his signature slides. Consult it while writing the deck.
- The repo root `CLAUDE.md` already carries the slide conventions (titles with trailing emoji, density, layouts, emoji anchors, citation format). Everything there applies. This skill adds what is not derivable from the repo.

## Phase 0: the brief

Ask only for what changes the work. Batch the questions in one message.

- Event, city, date, and format (conference slot, meetup, lightning, keynote, co-presented).
- Duration, and whether Q&A is inside it. Pierre takes questions live and often redirects to the booth.
- Delivery language (French or English). Slides stay in English either way. French delivery runs looser, with more asides and jokes; English runs tighter and closer to the slides. This changes slide density a little, not the content.
- Audience: how many already know the core tech (he polls the room by hand-raise and adapts). "75% know FDB" and "half the room knows Clever Cloud" are the kind of answer to expect.
- Topic and the one sentence the audience should leave with.
- Existing material: a previous deck under `talks/`, a Google Slides talk, a blog post on pierrezemb.fr, a transcript. Reworking is the common case. Read the previous deck and its `CLAUDE.md` before proposing anything, and treat its narrative spine and reductions as decisions already made unless he reopens them.
- Event brand color for the accent, and the event hashtag for the footer. Known events below; verify colors against the event site, they change year to year.

| Event | Accent used | Footer | Booth / rating QR | Notes |
|---|---|---|---|---|
| Sunny Tech (Montpellier) | `#d63f49`, secondary `#901f26` | `#SunnyTech2026` | Clever Cloud booth next to the food, rating QR | 45 min slots, French delivery, he polls the room |
| Devoxx France (Paris) | per year | `#DevoxxFR` | Booth when sponsoring | 45 min, French, large rooms |
| BugBash (Antithesis) | `#007AFF` (FDB blue) or `#BB77FF` | none | No booth | Lightning 10 min, English, DST-literate crowd |
| FinistDevs (Brest) | any | none | No booth, he co-organizes | Meetup, French, questions inline, personal stories welcome |
| FOSDEM (Brussels, ULB) | FOSDEM purple, check the site | `#FOSDEM` | No booth, no rating app | Devroom slots 20 to 30 min including questions, English |

## Phase 1: the cold open

Every talk of his starts with a specific incident, told in past tense with escalating beats and a twist at the worst moment. Propose one from `references/story-library.md` when the topic fits (the Hadoop NPE story is the default for anything testing, correctness, or reliability). Otherwise ask for a new one with these prompts:

- "What is the incident that made you care about this? Where were you, what broke, what was the moment you said wait what?"
- "What did you have to do that you would not do again?"
- "Who else was in the room?"

Ask for the numbers (70 nodes, 270 machines, 2.5M writes/s). He carries them in his head and they are the punchlines.

Exit: the story has a place, a system, an escalation, a worst-moment twist, and a lesson in one sentence that the talk will bookend to.

## Phase 2: the belief and the takeaway

Pierre's talks bust a belief the audience holds. Titles quote it: "The network is reliable", "I have 3 replicas, I'm safe", "It's 2026, and AI broke good enough". Ask:

- "What does the audience believe today that the talk proves wrong?"
- "What do you want them to do on Monday?" The answer becomes the closing ladder, checklist, or spectrum.
- "What is the honest limit?" He always places a "not a silver bullet" slide before the close, and he says "je vais être totalement franc" out loud before it. Get the limits early so the arc reserves room for them.

Exit: one belief, one Monday action, two or three limits.

## Phase 3: the arc

Propose a beat sheet: numbered sections, one line each, rough slide count per section, matching the budget in `references/talk-formats.md`. Use his standard spine and adapt it:

1. Cold open war story, then "so what went wrong?" abstraction.
2. Why now (the world changed: AI, scale, a new constraint).
3. The recurring motif introduced in its neutral state (a system box wired to two sources of chaos is his, but any diagram he will modify five times works).
4. Problem split into halves, each explored with concrete before abstract (a combinatorics table, then the sentence "you test what you imagine").
5. Evidence wall: papers, Jepsen reports, quotes, each linked. Bold the numbers.
6. The recipe, built incrementally: each step's output is the next step's input, code slides under 15 lines with line highlighting, the same fake grows from honest to hostile.
7. Bundle and name the thing (DST, simulation-driven development).
8. Proof from his own team: Materia, foundationdb-rs, the team that was not convinced until each member found a bug in their own code, the newcomer who shipped a core feature in a week.
9. Who else does it, and the spectacular external demo (Antithesis beating Mario).
10. Not a silver bullet.
11. Bookend to the cold open: "that exact combination is a seed".
12. Monday action: adoption ladder table, then the end slide with booth, QR, links.

Ask him to react to the beat sheet, not to a blank page. Mark which sections he will carry verbally over an image-only or one-line slide, because that is where his best material lives. For a 10 minute slot, sections 2, 5, 9 usually go, see the lightning beat sheet in the formats reference.

Also settle here: the motif and its states, the emoji per concept (one emoji per concept, reused throughout), the callbacks ("Remember our e-commerce API?").

Exit: beat sheet approved, slide budget adds up, motif and emoji map agreed.

## Phase 4: evidence and proof

Collect what each evidence slide needs, with links. Pierre cites everything: paper title, authors, venue and year, or the Jepsen analysis, or the tweet, or his own previous talk on YouTube. The story library has his usual set. For new claims, ask what he already has before searching, then search and bring back candidates with the key number bolded.

For the proof section, ask for fresh production numbers (rounds of simulation per PR, bugs found by category, download counts, team size then and now). Numbers dated to the event are stronger than round ones.

Exit: every evidence beat has a source and a headline number.

## Phase 5: the slide outline

Write the full outline as a numbered list: slide title with trailing emoji, layout if not default, and one line describing what is on it. Group by section. Flag slides as one of:

- content (bullets, table, code, quote),
- motif (the diagram in a given state),
- breather (one to three lines, a question, a transition),
- image-only (screenshot, gif, meme; he talks over it),
- spoken beat (the slide is nearly empty because the story is verbal; note the story in the per-talk `CLAUDE.md`, not in the slide).

Check pace: heavy, breather, heavy. Check the callbacks land. Check emoji consistency. Count against the budget.

Ask for approval of the outline. Expect edits on titles: he likes questions, imperatives, and quoted beliefs, and dislikes anything that reads like a section header.

Exit: outline approved.

## Phase 6: write the deck

Scaffold `talks/{name}/` from `assets/` (package.json, global-bottom.vue with the event hashtag, style.css, CLAUDE.md template), then write `slides.md` from the outline using `references/slide-recipes.md`. Write the per-talk `CLAUDE.md` at the same time: context, narrative spine, the spoken beats he carries verbally, reductions applied if reworked, conventions, image placeholders, commands. That file is the memory of the session for the next rework, so record decisions and reasons, not just outcomes.

Rules that the repo root does not state:

- No em dashes anywhere, in slides or in `CLAUDE.md`. Use commas, colons, parentheses, or a new sentence.
- No presenter notes in `slides.md`. Spoken material goes to the per-talk `CLAUDE.md` under "Spoken beats".
- Fakes, not mocks, when the topic is testing. He argues fakes beat mocks and testcontainers.
- Code examples in Java for app-level code, Rust for his own libraries, TOML for Materia workload config, C++ only for FoundationDB internals.
- Images he has not supplied yet become a dashed-box placeholder with the intended file name in `public/`, listed in `CLAUDE.md`, so he can drop PNGs in later.
- Pin `@slidev/cli` to what the other talks use (check a sibling `package.json`); a newer major has broken public-asset builds before.

After writing, run the review checklist and offer the commands:

```bash
nix develop ../.. --command pnpm install --ignore-scripts
```

```bash
pnpm dev
```

## Review checklist

Go through it yourself before handing the deck over, and report what you fixed.

- Cold open in the first four slides, bookend in the last five.
- Every h1 ends with an emoji, no h1 reads like a section header.
- Motif appears at least three times, each state different, accent highlight on the focused element, `opacity-40` on the dimmed ones.
- No `v-click`, no em dash, no presenter notes.
- Bullet slides: 2 to 4 bullets, one line each. Code slides: under 15 lines with highlighting. Dense slide: split it.
- Every paper, quote, and screenshot has a link or attribution.
- A limits slide before the close, an actionable table or ladder at the close, an end slide with booth, QR placeholder, and links.
- Slide count within the budget for the duration.
- Accent color and hashtag match the event.

## Timing rule of thumb

| Slot | Slides | Notes |
|---|---|---|
| 10 min lightning | 12 to 15 | One story, one recipe, one proof, no evidence wall |
| 20 to 25 min | 22 to 28 | Evidence collapsed to one table |
| 45 min | 45 to 50 | Full spine, image-only slides count half |

Q&A defaults: conference slots usually include 3 to 5 minutes of questions at the end, so plan the talk for the slot minus that; lightning slots have none; meetups take questions inline and lose about five minutes to them. Content slides run about a minute each. Image-only and breather slides run 20 to 30 seconds. He speeds up in the last five minutes and says so aloud, so put nothing essential after the limits slide except the ladder and the thanks.
