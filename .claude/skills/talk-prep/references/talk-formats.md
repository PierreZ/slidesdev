# Talk formats: budgets and real beat sheets

Slide counts are from Pierre's actual decks. Content slides run about a minute; image-only, gif, and breather slides run 20 to 30 seconds and count half.

| Slot | Slides | What survives |
|---|---|---|
| 10 min lightning | 12 to 15 | One story, one mechanism, one proof, one ask. No evidence wall, no whoami beyond one slide, no limits slide unless it is the point. |
| 20 to 25 min | 22 to 28 | Story, why now, motif, one half of the problem in depth, evidence as one table, recipe in three steps, proof, limits, ladder. |
| 45 min | 45 to 50 | Full spine below. Room for both halves of the problem, the evidence wall, the recipe built step by step, Mario, the team proof, the AI loop, the correctness-age coda. |
| Meetup (30 to 45 min, questions inline) | 30 to 40 | Looser, more personal, more numbers and screenshots, expect to lose five minutes to questions mid-talk. |
| Co-presented | shared budget | Alternate speakers per section, not per slide. One of you owns the demo, the other the story. |

## Beat sheet: 45 minute conference (Sunny Tech 2026, 49 slides)

1. Cover, whoami, "Also me" gif (3)
2. Cold open: Hadoop NPE, then "So, what went wrong?" (2)
3. Why now: "It's 2026, and AI broke good enough" (1)
4. Motif neutral: "Your code meets two things" (1)
5. Users half: motif highlight, checkout table 648, table 3,840, "You can't test what you don't know" (4)
6. World half: motif highlight, wall of papers table (2)
7. Pivot: "Let's test the worst", "Two ways to be the worst" (2)
8. Recipe, users: generator code, properties code (2)
9. Recipe, world: motif with simulated world, Kafka seam diagram, fake bus interface, fake fights back, be worse than production (5)
10. Name it: "Bundle it: that's DST" (1)
11. Who does it, Mario (2)
12. Proof: "So we're building a database", two TUI screenshots, "It found bugs everywhere", "Simulation-driven development" with CI screenshot (5)
13. "Not a silver bullet" (1)
14. AI loop: "You don't trust Claude, you trust the simulator" (1)
15. Coda: "The age of correctness has started", "Invest in correctness, now" (2)
16. Ladder: "How to adopt DST" table (1)
17. End: thanks, QR, booth, links (1)

Spoken beats carried verbally in that talk, not on slides: the black cat nickname, driving test in Brest versus Paris, the 14,000 line PR, "we cheated", the newcomer who shipped in a week, the disk-swap condition in the FDB simulator, "four Claudes running simulations", Antithesis is very expensive.

## Beat sheet: 10 minute lightning (BugBash 2026, 14 slides)

1. Cover (1)
2. Background in one slide: HBase trauma, discovered FDB (1)
3. Mechanism: FDB sim framework diagram, two TUI screenshots (3)
4. Our situation: "So, we started building", "Team grew from 1 to 6", ending on "How do we test this? 🙃" (2)
5. The question as a centered slide: "Can we inject our code inside FoundationDB?" (1)
6. The trick: ExternalWorkload diagram, Rust inside it (2)
7. The arc of belief: "At first: boring bugs", "Then workloads got richer", "Simulation finds unknown unknowns" (3)
8. Contribute back, end with crate links (2)

Nothing on limits, nothing on AI. One story arc: skeptical team to simulation-first.

## Beat sheet: meetup with questions (FinistDevs, foundationdb-rs, Google Slides)

1. Greeting, whoami, squash joke (spoken)
2. Lockdown, side project, Java first, then Rust, missing feature
3. PR timeline with dates, CI broken, dead repo, downloads keep going
4. The email from the ex-maintainer, why nobody can merge
5. Hard fork, neutral org, master to main, platform tiers, first release
6. Today: downloads graph, users list with "peut-être" caveats, projects
7. "How I maintain it": FFI and unsafe explained with the borrowed-tool analogy, the scan function before and after
8. CI matrix, nightly catches more than stable, MSRV rant
9. Tests don't scale, so generate them: the binding tester, seed, Python as reference, hourly runs, 219 days a month
10. Feedback loop good in dev, painful six months later
11. FDB simulator: what it injects, disk swap, seeds, 4,000 years
12. Our Rust code inside the simulator, open sourced, upstream PRs
13. Automation: dependabot, release-please, "I waited too long"
14. Good and bad: work time helped, met people, good codebase inherited, lacks maintainers and docs
15. Questions inline, redirected to the booth at the end

## Beat sheet: 45 minute conference, older style (Devoxx observability)

Story-light and list-heavy compared to 2026. Kept as a voice sample, not a structure to copy: software eats the world, value when running, mental model quiz, "the app is slow" Slack message, monitoring versus observability, architecture growing slide by slide, 3am SMS, silent errors (GC), butterfly effect, debugging is questions plus observations, USE, RED, four golden signals, static versus dynamic instrumentation, logs, metrics, traces, OpenTelemetry, visualize, flame graph story, conclusion.

## Worked example: cutting a 45 minute talk to fit a new event

What Pierre did when reworking Devoxx 2025 into Sunny Tech 2026 (from that deck's CLAUDE.md), in order of what went first:

1. Merge slides that make the same point: five LLM-framing slides became three, three Claude slides at the back became one.
2. Collapse a multi-slide evidence section into one table with links (nine papers, one slide). The long form stays in `prevention-vs-discovery` for the 60 minute version.
3. Replace a comparison slide with a quote that does the same job in one line.
4. Fold a standalone persona slide into a neighbour.
5. Drop a persona that had leaked from the spoken transcript into the slides (black cat) and keep it verbal.
6. Restore the original written bullets on the opening slides when the spoken version had been over-backported.

When cutting further, to 20 minutes: keep one half of the problem in depth (users, because the table is the funniest slide), keep the fake-fights-back code, keep one TUI screenshot, keep the ladder. Drop Mario, drop the correctness coda, drop the AI loop unless the event is about AI.
