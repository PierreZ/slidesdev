# My new job as a software engineer

## Context
- **Event**: Nuit des Communautés, 3rd edition, Brest, DATE TBD (organized by Externatic, ADN Ouest, French Tech Brest). Eight talks, two rooms, three hours. Pierre represents FinistDevs, not Clever Cloud. Accent `#1E6FD9` is a placeholder (no known event color), footer `#NuitDesCommunautes` is a guess, verify both.
- **Format**: 30 min slot, Q&A position unknown (planned as 25 min of content). 31 slides including cover and end, five of them breathers or image-only. Delivered in French, slides in English.
- **Audience**: mixed, technical but not deep. Cybersecurity, marketing, AI agents, databases, dev. Almost nobody knows FoundationDB, few know Clever Cloud. No distributed systems deep dive: Paxos and simulation are explained by analogy, never by mechanism.
- **Source**: new talk, built from a transcript session (Sept 2026). Inputs: six talk transcripts (Will Wilson BugBash 2025 and 2026, Steve Klabnik BugBash 2026, Marc Brooker re:Invent 2024 and BugBash 2026, Quentin Adam interview 2026), five Brooker posts, one Josh Bleecher Snyder post, the Agony of Consensus chapter on Multi-Paxos, the blog at pierrezemb.fr, and the git histories of moonpool and paros.

## Narrative spine: "The author of the code never mattered. What tells them they were wrong is all that matters, and today that thing is a simulator."

Personal arc, first person, optimistic: skeptic, then convert, then someone who no longer types code and has more fun than ever. Told as the evolution of the job, not as a tool review.

1. Cold open: a repo that died twice. moonpool born May 2024 (7 commits), retried March 2025 (3 commits), restarted 18 August 2025 with the first Claude Code trailer. One year later, 714 commits, 70k lines, plus paros at 61k lines in three months, 100% agent-written. "I typed almost none of it."
2. Me before: coffee and nights at engineering school (Brooks quote), reading code for the shape not the details, then the pager (Hadoop story) and the shift to correctness. Motto: "I do it because it's hard."
3. Motif introduced: writer, code, and the thing that tells the writer they were wrong. State 1: me, code, pager.
4. The belief: "It's my code, I trust it" (Klabnik). Busted by the on-call view: who has read the kernel? Trust comes from tests and operations. Thesis slide: Codex, Claude, or the new hire, same problem.
5. Why now: Brooker's "software's first act is over", then the two bars (85% typing before, now specs, quality tests, correctness hunting).
6. Motif states 2 to 4: junior with a pager, Claude with nothing (Quentin's "junior who read every thesis"), Claude with the simulator. "You don't trust Claude, you trust the simulator." Materia TUI screenshot as breather.
7. Proof: the squash flip, Paxos as a chasm (Agony chapter), paros recipe (core with no IO easy to re-read, simulator that hunts), seed 17898267817771645730 found on 8 September 2026, the FDB contribution in C++ that the simulator rejected (not reboot-proof), the 13 invariants for leader election.
8. Limits: copy-paste era (moonpool is two simulators glued), subsystems thrown away, kernel and switch OS people unconvinced, paros not for prod.
9. Software quality will dip, so handle failures better, and nothing beats a simulator for that.
10. Ladder: invest in correctness whatever you write, five levels.
11. Bookend: the repo is alive, coffee without the night shift, still reading code for the shape (diffs and seeds now), the hard part is all that is left. "More fun than at any point in my career."
12. End: FinistDevs, pierrezemb.fr, moonpool, paros. No booth, no rating QR (none known for this event).

## Spoken beats (not on slides)
- Slide 2: the "sport, not the git command" squash joke. Poll the room: who codes for a living, who uses an agent daily.
- Slide 4: the skeptic sentence. PLACEHOLDER, Pierre has not given it yet. The blog says the first months with LLMs were "honestly frustrating" and that Claude over-engineers without context.
- Slide 7: what he built in one night at school. PLACEHOLDER.
- Slide 8: which codebases he read for the shape. PLACEHOLDER. Candidate: FoundationDB's Flow in C++ read to port it to Rust.
- Slide 9: the Hadoop story is told in full verbally, black cat nickname, "comme de très bons ingénieurs d'astreinte, dans le doute on reboot". Alternative if he wants a lay-friendlier one: the 15 minute hang (tcp_retries2, 924 seconds), unpublished draft on the blog.
- Slide 13: hand-raise poll on the kernel. Nobody raises a hand, that is the joke.
- Slide 16: Will Wilson's Amdahl explanation, spoken: customers used to laugh at "50% of your time is testing", now they say 99% with a haunted look. Pierre spends his days there and enjoys it.
- Slide 17: the newcomer on Materia, January, one month to learn, one week to ship.
- Slide 18: Quentin's other lines, spoken: "l'IA est inhumaine, demandez-lui des tâches inhumaines", "c'est toi en un peu plus con mais qui travaille cent mille ans d'affilée". Pierre disagrees on stage with "tu deviens testeur, c'est cher payé": testing was the fun half all along.
- Slide 21: the squash scene in full. PLACEHOLDER date, job, duration, what he expected.
- Slide 23: massive analyses of systems in minutes (the frankenpaxos and FoundationDB analysis docs). Josh Bleecher Snyder's "I read a vanishingly small amount of the actual code" and "number of DNS incidents a month later: 0" as a spoken parallel.
- Slide 25: the FDB PR story in full, "the simulator treated me like Claude". PLACEHOLDER which PR, how long red, how found, what changed.
- Slide 27: Claude and Codex judging each other, "Claude more elegant, Codex more thorough" (Bleecher Snyder), Pierre runs both.
- Slide 30: Quentin's closing "si vous vous emmerdez tous les jours dans votre métier, changez de taf, parce que là on s'amuse" as a callback to his CEO.
- Throughout: French delivery is loose, the slides are sparse on purpose.

## Evidence
- "It's my code" → Klabnik at BugBash 2026, quoted in https://pierrezemb.fr/posts/bugbash-2026/ → "we wrote it, we understood it, we tried it, AI broke all three".
- First act over → Brooker, You Are Here, Feb 2026, https://brooker.co.za/blog/2026/02/07/you-are-here.html → cost of code near zero, "software's first act is over".
- Feedback loops decide what is easy → Brooker, What's easy, what's hard, May 2026, https://brooker.co.za/blog/2026/05/18/whats-easy-whats-hard.html → "SaaS is hard, system software is easy". Spoken, not on a slide yet.
- Junior who read every thesis → Quentin Adam interview, 2026, no URL yet. PLACEHOLDER link.
- Multi-Paxos chasm → https://cloudstreet-dev.github.io/The-Agony-of-Consensus-Algorithms/ch06-multi-paxos.html → "there is no paper called Multi-Paxos", "it is a chasm".
- paros numbers → git log at /Users/pierrezemb/workspace/rust/paros, 11 Sept 2026 → 192 commits, 61,279 lines, 272 tests, 109 of 192 commits co-authored, seed hunt 2,000 to 3,000, about 75 audit callbacks.
- moonpool numbers → git log at /Users/pierrezemb/workspace/rust/moonpool → 714 commits, 70,392 lines, 438 of 714 co-authored (61%), dormant May 2024 to Aug 2025, 132 commits in March 2026.
- Seed 17898267817771645730 → paros commit abbd4aa, 8 Sept 2026 → no leader for 60 s, 18,451 requests, 14,497 accepts cancelled, one Box::pin.
- 13 invariants → https://pierrezemb.fr/posts/simulating-leader-election-on-foundationdb/ → "the LLM proposes, simulation disposes".
- FDB upstream PRs → https://github.com/apple/foundationdb/pull/11288 and /12357.
- Vibe-engineering → https://blog.exe.dev/claude-is-not-a-compiler, July 2026 → "I read a vanishingly small amount of the actual code", "vibe-engineering is just engineering".
- Brooks → The Mythical Man-Month, 1975, "castles in the air, from air".

## Open questions for Pierre (answers change slides)
1. The skeptic sentence, with date and who heard it (slide 4).
2. The last function written by hand that shipped, or the month he noticed he had stopped (candidate slide near 16).
3. The squash scene: month, job launched, duration, expectation (slide 21).
4. The FDB PR that was not reboot-proof: which, how long red, how found, what changed (slide 25).
5. Which Clever Cloud software moonpool found bugs in (slide 30). The moonpool log mentions a hang co-investigated for the magnetar Pulsar client.
6. Real split of the week now, if any (slide 16).
7. HBase figures if the HBase story is used instead of Hadoop: 255 machines, 2M and 6M, or 250+, 2.5M and 6.5M.
8. Event date, Q&A inside the 30 min or not, accent color and hashtag.
9. Whether the "junior with a pager" is a real Clever Cloud practice and at what tenure (slide 17).

## Decisions made in the session, and why
- Personal arc over technical arc: the room is mixed, and Pierre wants to share the feeling of having fun again.
- The cold open is the git history of moonpool, not an outage: dates as beats, Klabnik style, no code on screen. The Hadoop story survives as the "then I got a pager" beat.
- The thesis is "the author never mattered", stronger than Klabnik's anxiety and Quentin's "the code no longer matters": an on-call engineer's position.
- Motif is the three-box loop (writer, code, what tells them they were wrong), modified four times, geometry constant.
- The 71 KB AGENTS.md story was dropped at Pierre's request. The "what I write now" slide uses the 13 invariants instead.
- Clever Cloud appears as engineering facts only (newcomer, Materia TUI, bugs found), never as a pitch. Pierre represents FinistDevs.
- Rust only in one code block (the git trailer, which is text). No Java, no C++ on screen.

## Conventions
- No em dashes. No `v-click`, progressive reveals are separate slides. No presenter notes.
- Emoji map: ☕ creating (school), 📖 reading code, 📟 pager and on-call, 🧗 hard work, 🔁 feedback loop, 📦 code, 🧑‍🎓 junior, 🤖 Claude and agents, 🎲 simulator and seeds, 🎯 unknown unknowns and correctness, 🏸 squash, 🪦 the dead repo, 🌊 moonpool, 🏝️ paros, 🎬 first act, 🪜 ladder.
- Bold for key phrases only, they render in accent.

## Images
Real assets present: `public/materia-sim-triple.png` (Materia simulation TUI, breather after the simulator slide), `public/claude-moonpool.png` (Claude fixing a moonpool bug from a replay, not placed yet, candidate for slide 19 or 24).
Placeholders to fill (dashed boxes in the deck): `public/fdb-pr-red.png` (slide 25, the red CI on the FDB PR). Candidates not yet placed: a photo of Pierre's face or a git log screenshot for slide 3, a squash racket or court photo for slide 21.

## Commands
`nix develop ../.. --command pnpm install --ignore-scripts` then `pnpm dev` / `pnpm build` / `pnpm export`.
