# My new job as a software engineer

## Context
- **Event**: Nuit des Communautés, 3rd edition, Brest, DATE TBD (organized by Externatic, ADN Ouest, French Tech Brest). Eight talks, two rooms, three hours. Pierre represents FinistDevs, not Clever Cloud. Accent `#1E6FD9` is a placeholder (no known event color), footer `#NuitDesCommunautes` is a guess, verify both.
- **Format**: 30 min slot, Q&A position unknown (planned as about 25 min of content). 32 slides including cover, three act titles, one centered thesis slide, and end. Delivered in French, slides in English.
- **Audience**: mixed, technical but not deep. Cybersecurity, marketing, AI agents, databases, dev. Almost nobody knows FoundationDB, few know Clever Cloud. No distributed systems mechanism on screen: Paxos and simulation are explained by analogy.
- **Source**: new talk, built in a transcript session (Sept 2026). Inputs: six talk transcripts (Will Wilson BugBash 2025 and 2026, Steve Klabnik BugBash 2026, Marc Brooker re:Invent 2024 and BugBash 2026, Quentin Adam interview 2026), five Brooker posts, Josh Bleecher Snyder's "Claude is not a compiler", the Agony of Consensus chapter on Multi-Paxos, the blog at pierrezemb.fr, and the git histories of moonpool and paros.

## Narrative spine: "Code got cheap, correctness did not, and correctness is the half of the job the pager taught me to love."

Personal, first person, optimistic. Three acts and a conclusion. Storytelling over numbers: figures appear only as punchlines (70 machines, ten minutes, four days, one week, three months).

**Before ☕, the job I learned.** Coffee and nights at school (coding to code). Reading code for the shape, not the details (planted, paid at the end). The pager: Hadoop, the turtle, the NPE. Correctness becomes the value, "I do it because it's hard". Motif state 1: me, code, pager. On call you run code you never wrote, who has read the kernel, trust comes from tests and operations, not the author (planted, paid in act 3).

**Transition 🌫️, the day the machine wrote the code.** The skeptic (placeholder). Ten minutes with Windsurf on a stuck OSS contribution: it read the code for the shape (pays slide 2). Same model under a simulator at work: very good, "it was the loop". Squash: a big job launched, done on return, and the chores now run at night. Materia rewritten in four days because years of tests and simulation checked every step: "code is the cheap part, the loop is the capital". Code got cheap, correctness did not, the Amdahl argument in first person. Two bars: where the week went.

**Now 🔬, my new job.** Codex, Claude or the new hire, same problem (pays slide 6). Motif states 2 to 4: junior with a pager, Claude with nothing ("a junior who read every thesis and was never woken up"), Claude with the simulator ("you don't trust Claude, you trust the simulator"). FDB contribution in C++ without knowing C++, the PR the simulator kept red. Paxos as a chasm, paros built on evenings, the bug never looked for. Not just evenings: moonpool catches bugs at work, Clever Cloud core rewritten in Rust, legacy no longer a wall. What I do all day: read systems in minutes, write invariants, build the loop, review diffs and seeds (pays slide 2 again).

**Conclusion 🔬, the age of correctness has begun.** Not a silver bullet. Quality will dip, so handle failures better, nothing beats a simulator. First era writing software, second era software that survives, ladder in five levels, invest in correctness and tooling. Bookend: coffee without the night shift, still reading for the shape, the hard part is all that is left, more fun than ever.

## Spoken beats (not on slides)
- whoami: "sport, not the git command". Poll: who codes for a living, who runs an agent daily.
- Coffee and nights: Brooks's "castles in the air, from air" if he wants a quote, and one thing built in one night at school (not given yet).
- Read code for the shape: which codebases (not given yet; candidates HBase, Kafka, FoundationDB's Flow in C++).
- Pager: the full Hadoop story, black cat nickname, "dans le doute on reboot". Alternative lay-friendly story: the 15 minute hang (tcp_retries2, 924 seconds), unpublished draft on the blog.
- Kernel: hand-raise, nobody, that is the joke.
- Skeptic: Quentin's "perroquet statistique" phrasing is his, Pierre's own sentence still missing.
- Windsurf: which repo, what he was trying to contribute (not given yet).
- Squash: the doubt beat, spoken here: his CEO's "tu as largué la partie fun à l'agent, tu deviens testeur, c'est cher payé", and the answer that testing was the fun half all along. Klabnik's "which one am I going to be?" can be mentioned.
- Materia rewrite: when, from what to what, why (not given yet).
- Code got cheap: Will Wilson's sales anecdote, customers used to laugh at "50% of your time is testing", now they say 99% with a haunted look.
- Give it the simulator: the inversion, spoken: agents find easy what answers fast and mechanically, hard what needs a human eye, so the website became hard and the database with a simulator became easy. "The thing I did because it was hard became the thing an agent does best, because I had built the loop."
- FDB PR: the full story (which PR, how long red, how found, what changed; not given yet). Upstream PRs: apple/foundationdb #11288 (pure C workload API) and #12357 (delay()).
- paros: named after his favourite Greek island, next to Lamport's Paxos. Milestones M1 to M5 mirror what the paper leaves out.
- The bug: seed 17898267817771645730, 8 September 2026, 2,000 seed hunt, 18,451 requests answered, 14,497 accepts cancelled, fixed with one Box::pin. Numbers are spoken, not on the slide.
- Not just evenings: which Clever Cloud software moonpool found bugs in (not given yet; the moonpool log mentions a hang co-investigated for the magnetar Pulsar client). Quentin's 2026 goal of running only code written in 2026.
- What I do all day: the 13 invariants for leader election on FDB, "the LLM proposes, the simulation disposes". Josh Bleecher Snyder's "I read a vanishingly small amount of the actual code" and "DNS incidents a month later: 0".
- Not a silver bullet: moonpool is FoundationDB's Flow plus TigerBeetle's disk faults plus Antithesis's assertion vocabulary. March 2026, the 6,000-line actor system deleted, a test suite rewritten because its assertions were "structurally impossible". Quentin's kernel and switch OS people: "l'IA nous casse les couilles".
- Closing: Quentin's "si vous vous emmerdez tous les jours dans votre métier, changez de taf, parce que là on s'amuse", as a callback to his CEO.
- French delivery is loose, slides are sparse on purpose.

## Evidence (sources behind the arguments, cited verbally or not at all)
- Klabnik at BugBash 2026, quoted in https://pierrezemb.fr/posts/bugbash-2026/: "we wrote it, we understood it, we tried it, AI broke all three". Behind "It's my code" (now spoken with the kernel slide).
- Brooker, You Are Here, https://brooker.co.za/blog/2026/02/07/you-are-here.html: cost of code near zero, "software's first act is over". Behind "Code got cheap" and "the age of correctness". Not cited on slides by Pierre's choice.
- Brooker, What's easy, what's hard, https://brooker.co.za/blog/2026/05/18/whats-easy-whats-hard.html: the feedback loop hypothesis. Behind the spoken inversion on "Give it the simulator".
- Will Wilson, BugBash 2026 opener: Amdahl, vibe quality, the Trends graph. Behind "Code got cheap".
- Quentin Adam interview, 2026, no URL yet: "junior qui a lu la totalité des thèses", "tu deviens testeur", pentest and simulation on every commit, 70 developers, no legacy in 2026.
- The Agony of Consensus Algorithms, ch. 6, https://cloudstreet-dev.github.io/The-Agony-of-Consensus-Algorithms/ch06-multi-paxos.html: "there is no paper called Multi-Paxos", "it is a chasm".
- Josh Bleecher Snyder, https://blog.exe.dev/claude-is-not-a-compiler: vibe-engineering, scar-tissue document.
- paros git log (/Users/pierrezemb/workspace/rust/paros): 192 commits, 61,279 lines, 272 tests, first commit 15 June 2026, 109 of 192 commits co-authored, seed hunt 2,000 to 3,000, commit abbd4aa for the bug.
- moonpool git log (/Users/pierrezemb/workspace/rust/moonpool): 714 commits, 70,392 lines, 438 of 714 co-authored, restarted 18 August 2025 with the first Claude Code trailer.
- Leader election invariants: https://pierrezemb.fr/posts/simulating-leader-election-on-foundationdb/.

## Open questions for Pierre (answers change slides)
1. The skeptic sentence, with date and who heard it (slide "The skeptic").
2. Windsurf: which repo, what contribution, roughly when.
3. The squash scene: month, job launched, duration, what he expected.
4. Materia rewrite: when, from what to what, why.
5. The FDB PR that was not reboot-proof: which, how long red, how found, what changed.
6. Which Clever Cloud software moonpool found bugs in.
7. One thing built in one night at school, and which codebases he read for the shape.
8. Event date, Q&A inside the 30 min or not, accent color and hashtag.
9. Whether juniors carrying a pager is a real Clever Cloud practice and at what tenure.

## Decisions made in the session, and why
- Personal three-act structure (before, transition, now) plus a conclusion on correctness, at Pierre's request, after two rejected outlines that leaned on evidence tables and citations.
- No Brooker or Wilson quotes on slides: their arguments are restated in first person. Pierre's choice.
- Storytelling over numbers: numbers only as punchlines. Pierre's choice.
- Dropped by Pierre: the moonpool "repo that died twice" cold open and its 18 August 2025 beat, the management years, the 71 KB AGENTS.md story, the Materia TUI screenshot, evidence tables.
- The thesis "the author never mattered" is the on-call engineer's answer to Klabnik's anxiety and Quentin's "the code no longer matters". Planted on the kernel slide, paid on "Codex, Claude, or the new hire".
- Motif is the three-box loop (writer, code, what tells them they were wrong), four states, geometry constant. Pager for humans, simulator for agents.
- Clever Cloud appears as engineering facts only (newcomer, rewrite in Rust, Materia in four days), never as a pitch. Pierre represents FinistDevs.
- No code on screen except the NPE mention. No Java, no Rust, no C++.

## Conventions
- No em dashes. No `v-click`, progressive reveals are separate slides. No presenter notes.
- Emoji map: ☕ creating and school, 📖 reading code, 📟 pager and on-call, 🧗 hard work, 🔁 feedback loop, 📦 code, 🧑‍🎓 junior, 🤖 Claude and agents, 🎲 simulator and seeds, 🎯 correctness and unknown unknowns, 🏸 squash, 🌙 evenings and night shift, 🏝️ paros, 🔬 the age of correctness, 🏢 work.
- Bold for key phrases only, they render in accent.

## Images
Real assets present but not placed: `public/materia-sim-triple.png` (Materia simulation TUI, removed from the deck by Pierre), `public/claude-moonpool.png` (Claude fixing a moonpool bug from a replay, candidate for "Give it the simulator" or "The bug I never looked for").
Placeholders: none as dashed boxes yet. Candidates if Pierre wants images: a squash court for the squash slide, the red CI on the FDB PR, a photo of the Brest office or FinistDevs for whoami.

## Commands
`nix develop ../.. --command pnpm install --ignore-scripts` then `pnpm dev` / `pnpm build` / `pnpm export`.
