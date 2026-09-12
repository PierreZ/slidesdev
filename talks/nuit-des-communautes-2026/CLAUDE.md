# My new job as a software engineer

## Context
- **Event**: Nuit des Communautés, 3rd edition, Brest, DATE TBD (organized by Externatic, ADN Ouest, French Tech Brest). Eight talks, two rooms, three hours. Pierre represents FinistDevs, not Clever Cloud. Accent `#1E6FD9` is a placeholder (no known event color), footer `#NuitDesCommunautes` is a guess, verify both.
- **Format**: 30 min slot, Q&A position unknown (planned as about 25 min of content). 23 slides including cover and end, three of them light (a breather, an image-only, a bookend). The FinistDevs slide comes before the cover, at Pierre's request. Delivered in French, slides in English.
- **Audience**: mixed, technical but not deep. Cybersecurity, marketing, AI agents, databases, dev. Almost nobody knows FoundationDB, few know Clever Cloud. No distributed systems mechanism on screen: Paxos and simulation are explained by analogy.
- **Source**: third version, rewritten on 12 September 2026 after Pierre reopened the narrative. Inputs added this time: Kiro's "Frontier engineering" ten principles, three social posts Pierre likes (restated on slides, never linked or attributed by his choice), the FinistDevs site, and his blog (ten-years-programming, back-engineering, 2025 year in review, llms-for-engineering, specs-are-back, testing-prevention-vs-discovery, bugbash-2026, simulating-leader-election). Earlier inputs still behind the arguments: six talk transcripts (Will Wilson BugBash 2025 and 2026, Steve Klabnik BugBash 2026, Marc Brooker re:Invent 2024 and BugBash 2026, Quentin Adam interview 2026), five Brooker posts, Josh Bleecher Snyder's "Claude is not a compiler", the Agony of Consensus chapter on Multi-Paxos, and the git histories of moonpool and paros.

## Narrative spine: "Everyone says AI changed my job. It didn't. My job was never typing, it was finding out I was wrong before the users did. Every job I had made that loop faster. AI only changed who types."

Chronological, first person, optimistic, told as a career story from 2010 to today. No cold-open war story: the Hadoop turtle arrives as the 2015 beat, where it happened. Storytelling over numbers: figures appear only as punchlines (800 servers, 70 machines, 250 machines, 4,000 years, ten minutes, four days, one week, three months, two thirds, 130 evenings).

**FinistDevs (slide 1), then the cover (slide 2) and whoami (slide 3).** The deck opens on the community because it is the Nuit des Communautés and Pierre speaks for it: FinistJUG since December 2011, 130+ evenings, three organizers, next evening placeholder.

**Act 1, the loop through my career (slides 4 to 10).** 2010, coding for the puzzle, the compiler and valgrind as the first thing that said "wrong" (motif state 1). Why distributed systems: hard so interesting, everywhere, a lot to learn (the third reason pays on the closing slide). 2015, the pager, OVH Metrics, the 70-node Hadoop turtle and the NPE at recovery. "So what went wrong?" carries motif state 2 and the thesis: my job became finding out I'm wrong before the users do. "What the pager taught me": mostly that I did not know a lot of things, but also observability and error handling; tests check what I imagined, the pager found the rest; then the question that hands over to simulation, "can we automate finding what we don't know in the code?". HBase (250+ machines, the repair tool that moves data out, the pager tells the truth too late) and FoundationDB (the simulator written before the database) share the motif state 3 slide, "same move every job: compiler, pager, simulator". Materia: the team not convinced until each found a bug in their own code, the newcomer who shipped in a week.

**Act 2, the writer changed (slides 11 to 13).** Motif state 4 carries the whole conversion: the skeptic sentence (placeholder), ten minutes with Windsurf reading the code for the shape, the same model outside and inside the simulator; Claude in a dashed box, code dimmed, simulator in accent, "it was not the model, it was the loop, the writer changed, the loop did not". Squash and the four-day Materia rewrite share a slide: the night shift has a new worker, and years of tests and simulation checked every step, code is cheap, the loop is the capital. Pick the language that talks back: Rust pays twice, for me and for the agent.

**Act 3, the factory (slides 14 to 19).** Factorio image: I used to craft everything by hand, now I build small factories. "Someone wrote the factory manual": four Kiro principles on the left, the year the pager taught each one on the right, "nothing new, just named". "You own what ships under your name": throwaway code as black box, agent code in production needs a higher bar, if you can't debug it you can't own it, two thirds of assumptions wrong on hard problems. paros on evenings and the bug never looked for on one slide, FoundationDB in C++ without C++ and the PR the simulator kept red. Not a silver bullet.

**Close (slides 20 to 23).** Bookend to the turtle: that combination is a seed, found at night while I play squash. Ladder in five levels. "Same job, new tools" now also carries what he does all day: coffee without the night shift, reading diffs and seeds, writing what must always be true, building the loop, the hard part is all that is left, what distributed systems taught me is now every developer's job, "my new job hasn't changed much, it is only more fun". Thanks, with a FinistDevs callback.

## Spoken beats (not on slides)
- whoami: "sport, not the git command". Poll: who codes for a living, who runs an agent daily.
- 2010: engineering school, electronics track, first `Hello, world` in C on CentOS. Brooks's "castles in the air, from air" if he wants a quote, and one thing built in one night at school (not given yet).
- Why distributed systems: the Arkea internship in third year, the tutor pushing him toward Hadoop and Kafka, the moment the puzzle got bigger than one machine.
- Pager: the 70-node cluster belonged to another team, he was on the incident. Black cat nickname, "dans le doute on reboot". The Metrics platform numbers (800 servers, 1.8M points per second, 450M series) are his own team's.
- What the pager taught me: the observability talk at Devoxx ("how on-call changed the way I develop"), the 3 a.m. SMS, silent errors, the butterfly effect. Prevention versus discovery: tests ask "did we break what worked", the pager asks "what else is broken". Added by Pierre on 12 September 2026 as the bridge to DST.
- HBase: 250 to 300 machines, 2.5M writes per second, 6.5M reads. Why he went looking for a database with a simulator.
- FoundationDB: "accident industriel", research-grade design that shipped. Disk swap as an integration test, "c'est quand même un peu rigolo".
- Materia: 1 to 6 people, almost none with database internals. Bugs found: wrong index, corruption during reindexing, dual leader under clock skew, etcd compaction deleting live data.
- Skeptic: Quentin's "perroquet statistique" phrasing is his, Pierre's own sentence still missing. Windsurf: which repo, what contribution (not given yet).
- Motif state 4: the inversion, agents find easy what answers fast and mechanically, hard what needs a human eye, so the website became hard and the database with a simulator became easy.
- Squash: the doubt beat: his CEO's "tu as largué la partie fun à l'agent, tu deviens testeur, c'est cher payé", and the answer that testing was the fun half all along. Klabnik's "which one am I going to be?" can be mentioned.
- Materia rewrite: when, from what to what, why (not given yet).
- Pick the language: the Carl exchange behind it ("when the language you pick doesn't impact productivity, why not pick the one that gets you the best end product", and the reply that Rust does impact productivity by giving early feedback to agents). Restated in first person, not attributed.
- Factorio: one sentence for non-players, "you start by crafting each gear by hand, and you end up never touching a gear again". "The factory must grow" as a spoken joke only. No biters, Pierre removed them.
- Factory manual: Kiro's ten principles, the six not on screen (architect not typist, maximize agent time, build your codebase for agents, direction over execution, agents for everything, tune your setup). "Frontier developers hand-write less than 1% of the code they produce."
- You own what ships: Boris Cherny's "there is room for both" post (prototypes as black box, production code by Claude needs a higher bar, Anthropic's lint, tests, fuzzers, reviews, "your job is to hold the bar"), and the "if you can't debug it, you can't own it" post. Both restated, not attributed. The "two thirds of assumptions wrong" line is Pierre's own experience.
- paros: named after his favourite Greek island, next to Lamport's Paxos. Milestones M1 to M5 mirror what the paper leaves out. "There is no paper called Multi-Paxos."
- The bug: seed 17898267817771645730, 8 September 2026, 2,000 seed hunt, 18,451 requests answered, 14,497 accepts cancelled, fixed with one Box::pin. Numbers are spoken, not on the slide.
- FDB PR: the full story (which PR, how long red, how found, what changed; not given yet). Upstream PRs: apple/foundationdb #11288 (pure C workload API) and #12357 (delay()).
- What I do all day: the 13 invariants for leader election on FDB, "the LLM proposes, the simulation disposes". Josh Bleecher Snyder's "I read a vanishingly small amount of the actual code".
- Not a silver bullet: moonpool is FoundationDB's Flow plus TigerBeetle's disk faults plus Antithesis's assertion vocabulary. March 2026, the 6,000-line actor system deleted, a test suite rewritten because its assertions were "structurally impossible". Quentin's kernel and switch OS people: "l'IA nous casse les couilles".
- Same job, new tools: Klabnik at BugBash 2026, "every programmer is about to need what you already know". Quentin's "si vous vous emmerdez tous les jours dans votre métier, changez de taf, parce que là on s'amuse", as a callback to the CEO beat.
- FinistDevs: FinistJUG's first evening was 8 December 2011 with Antonio Goncalves. About 131 posts in the archive. The March 2026 evening with Finist'AI Club at ISEN was his own "prévention vs découverte" talk.
- French delivery is loose, slides are sparse on purpose.

## Evidence (sources behind the arguments, cited verbally or not at all)
- Kiro, Frontier engineering, https://kiro.dev/topics/frontier-engineering/: the only external link on a slide. Ten principles; the four on screen are fast feedback loop, human standards, trust the boundaries, code is disposable (with "boundary tests are the contract any rewrite has to satisfy"). "The work is deciding, over and over, what needs your attention and what does not."
- Pierre's blog: ten years of programming (2010 to 2020 timeline, "on-calls was a good way to ensure software quality"), back in engineering (January 2025), 2025 year in review, what I tell colleagues about LLMs ("LLMs amplify expertise, they do not replace it", 219 days of testing per month), simulating leader election (13 invariants, "the LLM proposes, simulation disposes"), testing prevention vs discovery, specs are back ("code is becoming like fast food"), BugBash 2026 ("you do not trust Claude, you trust the simulator").
- Klabnik at BugBash 2026, quoted in https://pierrezemb.fr/posts/bugbash-2026/: "we wrote it, we understood it, we tried it, AI broke all three", "every programmer is about to need what you already know".
- Brooker, You Are Here, https://brooker.co.za/blog/2026/02/07/you-are-here.html, and What's easy, what's hard, https://brooker.co.za/blog/2026/05/18/whats-easy-whats-hard.html. Not cited on slides by Pierre's choice.
- Will Wilson, BugBash 2026 opener: Amdahl, vibe quality, the Trends graph.
- Quentin Adam interview, 2026, no URL yet: "junior qui a lu la totalité des thèses", "tu deviens testeur", 70 developers, no legacy in 2026.
- The Agony of Consensus Algorithms, ch. 6, https://cloudstreet-dev.github.io/The-Agony-of-Consensus-Algorithms/ch06-multi-paxos.html.
- Josh Bleecher Snyder, https://blog.exe.dev/claude-is-not-a-compiler.
- paros git log (/Users/pierrezemb/workspace/rust/paros): 192 commits, 61,279 lines, 272 tests, first commit 15 June 2026, commit abbd4aa for the bug.
- moonpool git log (/Users/pierrezemb/workspace/rust/moonpool): 714 commits, 70,392 lines, restarted 18 August 2025.
- FinistDevs, https://finistdevs.org/ and /archives/: founded 2011 as FinistJUG, organizers Horacio Gonzalez, Stéphanie Moallic, Pierre Zemb.

## Open questions for Pierre (answers change slides)
1. The skeptic sentence, with date and who heard it (slide "A statistical parrot").
2. Windsurf: which repo, what contribution, roughly when.
3. The squash scene: month, job launched, duration, what he expected.
4. Materia rewrite: when, from what to what, why.
5. The FDB PR that was not reboot-proof: which, how long red, how found, what changed.
6. FinistDevs: next evening date and venue, and whether "130+ evenings" and the three names are what he wants on screen.
7. The Factorio screenshot, and whether he wants a real one or a drawing.
8. Event date, Q&A inside the 30 min or not, accent color and hashtag.
9. One thing built in one night at school.

## Decisions made in the session, and why
- Chronological career story from 2010, no cold-open war story. Pierre found the previous opening weird (belief slide, then the outage) and asked to start with his story directly. FinistDevs is slide 1, before the cover, moved there by Pierre after versions at slide 3 and before the thanks. The Hadoop turtle moved to its place in time, 2015, and still gets the bookend.
- The cover keeps the question "My new job as a software engineer"; the answer "hasn't changed much" is saved for the closing slide.
- Thesis restated: the job was never typing, it was finding out you are wrong before the users do; every job made that loop faster; AI changed who types. "More fun than ever" is the closing emotion.
- Motif is the three-box loop (writer, code, what tells them they were wrong), four states, geometry constant: segfault, pager, simulator, then Claude replaces the writer. Claude's box is dashed (unproven), the simulator stays in accent (trusted).
- Why distributed systems, three reasons, added by Pierre: hard so interesting, everywhere, a lot to learn. The third pays on the closing slide.
- Two clusters, two slides: the 70-node Hadoop NPE was another team's cluster (told as the pager story); the 250 to 300 machine HBase cluster was his own (told as "the pager is too late").
- Social posts restated in Pierre's voice, never linked or attributed, by his choice. Kiro is linked because it is an article, not a post.
- Factorio analogy introduced late (act 3), not as a second motif, so it does not compete with the loop. Biters removed by Pierre.
- Compacted from 33 to 27, then to 22, at Pierre's request. First pass: belief slide folded into the skeptic, one Hadoop slide instead of two, "same move" breather folded into the simulator slide, "I recognized my pager" became the right column of the manual slide, "hold the bar" and "can't debug it" merged. Second pass: HBase into the simulator slide, the skeptic and Windsurf into motif state 4, the four-day rewrite into the squash slide, paros into the bug slide, "what I do all day" into "same job, new tools".
- No Brooker or Wilson quotes on slides. Storytelling over numbers. Clever Cloud as engineering facts only, never a pitch. No code on screen except the NPE mention.

## Conventions
- No em dashes. No `v-click`, progressive reveals are separate slides. No presenter notes.
- Emoji map: ☕ creating and school, 📖 reading code, 📟 pager and on-call, 🧗 hard work, 🔁 feedback loop, 📦 code, 🧑‍🎓 junior, 🤖 Claude and agents, 🎲 simulator and seeds, 🎯 correctness and unknown unknowns, 🏸 squash, 🌙 evenings and night shift, 🏝️ paros, 🏭 the factory, 📜 the manual, 📏 the bar, 🐢 the turtle, 🤝 FinistDevs, 🏢 work.
- Bold for key phrases only, they render in accent.

## Images
Placeholders (dashed boxes in the deck): `public/factorio-handcraft.png` on "I used to craft everything by hand".
Real assets present but not placed: `public/materia-sim-triple.png` (Materia simulation TUI, removed from the deck by Pierre), `public/claude-moonpool.png` (Claude fixing a moonpool bug from a replay, candidate for "You don't trust Claude" or "The bug I never looked for").
Candidates if Pierre wants more images: a squash court, the red CI on the FDB PR, a FinistDevs evening photo for the community slide.

## Commands
`nix develop ../.. --command pnpm install --ignore-scripts` then `pnpm dev` / `pnpm build` / `pnpm export`.
