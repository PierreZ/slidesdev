# Pierre's voice, on stage and on slides

Extracted from six transcripts (Sunny Tech 2023 FoundationDB intro, FinistDevs foundationdb-rs maintainer talk, Materia KV launch interview, Devoxx observability talk, Sunny Tech 2026 simulation-driven development, BugBash 2026 lightning) and the Slidev decks under `talks/`. Quotes are his own words. Use this to make proposals sound like him, not to script him.

## 1. The persona on stage

The developer who is also on call. Not an SRE, not an academic: "mon background c'est vraiment du développement logiciel, l'astreinte me permet d'avoir des bons réflexes en tant que dev." He has scars and names them: "je suis chat noir, c'est toujours moi qui me tape l'incident un peu bizarre." The nickname follows him from company to company and he complains about it with a smile.

He is opinionated and sourced. He says "la doc et le marketing c'est souvent des trucs bullshit" and then shows the Jepsen report. Strong claims, always backed by a paper, a number from his own production, or a story he lived.

He is proud of his team and honest about their doubts: "my team was not really convinced at all, until each team member found a bug in their own code." Proof slides are team stories, not feature lists.

He is candid about limits and money: "je vais être totalement franc", "c'est pas magique", "je vous préviens, c'est très très très cher."

## 2. How he opens

Greeting, place, a status line about himself, then whoami, then a poll.

- "Bonjour à tous, j'espère que vous allez bien. Moi je suis super content d'être ici à Montpellier."
- Status line: "je n'ai plus de voix, c'est à cause du stand sur les deux jours." In English: "as you may have guessed from my accent."
- Whoami is a fixed bit, delivered fast: Clever Cloud (French or European cloud provider, booth "à côté de la nourriture"); distributed systems, "tous les trucs un peu relous que les gens ne veulent pas opérer, généralement c'est moi qui suis d'astreinte dessus"; maintainer of a Rust database driver; co-organizes FinistDevs; "je joue beaucoup au squash, le sport, pas la commande git." On slides this is the `$ whoami 👋` two-column with photo, followed by an "Also me 🔥" slide with the "this is fine" gif.
- Poll by hand-raise, then react to the count: "qui connaît Clever Cloud? une bonne moitié de la salle. Qui utilise? un peu moins." He polls two or three times per talk and uses the answer to set depth ("ok, I will start simpler").

Then the war story, with no transition sentence: "Moi je vais vous raconter celui qui m'a le plus marqué."

## 3. How he tells a story

Past tense, first person, one incident, escalating beats, a physical image, then the twist at the worst possible moment, then the abstraction.

The Hadoop story, as told: 70 node cluster, huge network split, "le cluster commence à essayer de se sauver", disks fill up on some roles, "il se met sur le dos, tel une tortue, bouge plus", add disk, nothing, "comme de très bons ingénieurs d'astreinte, dans le doute on reboot", NullPointerException at startup on all 70 machines, the bug is known and patched upstream, backport, recompile, redeploy under fire, "c'est pas la meilleure des expériences", and the point: "il nous impacte au pire moment, pendant le recovery."

Then the pivot, always the same question: "Donc, qu'est-ce qui s'est passé?" or "So what went wrong?" and the generalization in one or two sentences ("le code ne prenait pas en compte la réalité").

Other stories follow the same shape: the disk physically pulled from a server by mistake and nothing happened; the contributor whose PRs sat unanswered for a year until an email from the ex-maintainer; the 200 line feature that became a 14,000 line PR; the flame graph that showed 80% of CPU in serialization.

## 4. Signature devices

**Voice the skeptic, then answer.** He plays the audience's objection in their voice before rebutting: "Là vous vous dites: oui bon Pierre, t'es gentil, mais moi dans mes tests je vérifie à la main." Then: "Oui et non." Slides mirror this with quoted beliefs as titles: `# "The network is reliable" 🌐`, `# "I have 3 replicas, I'm safe" 🛡️`.

**Callbacks.** "Vous vous souvenez de mon bug Hadoop?" "Remember our e-commerce API?" The cold open returns near the end ("that exact combination is a seed in a simulation, found in seconds, no 3am wake-up call"). Use the callback as a slide title.

**Rhetorical question as transition.** Nearly every section starts with one: "Comment on fait pour tester ça?", "Qu'est-ce qui pourrait bien mal se passer?", "Et si on testait le pire en dev?", "Can we inject our code inside FoundationDB?" These become breather slides with a single line.

**Numbers as punchlines, escalated.** 648 test cases, then add one feature per dimension, 3,840. Ten simulation rounds were enough, then 50, then 500. 4,000 years of simulation per year at Apple. 219 days of bug hunting per month on GitHub Actions. He pauses after the number and lets the emoji on the slide do the reaction (😱, 💀).

**Concrete before abstract, every time.** The e-commerce checkout table before "property-based testing". The Kafka order service before "fakes". The lending-a-tool-to-a-colleague image before "borrow checker".

**Name the thing late.** He walks through generators, then properties, then fakes, then fault injection, and only then says "that bundle is Deterministic Simulation Testing." Naming early would let the audience file it away.

**"We cheated."** His favourite framing for a clever hack: "on a triché, on a pris notre code et on l'a injecté dans le simulateur de FDB, comme ça on n'a pas besoin d'écrire un simulateur." Also "on est les premiers au monde à avoir fait ça", said lightly.

**The honesty flag before the limits slide.** "Je vais être totalement franc." "C'est pas magique la simulation." Then two or three real limits with no softening (performance is invisible, fakes can diverge, a bug can hide in a seed for months).

**Warnings about cost and difficulty.** "C'est très très cher." "Ça reste un système distribué à opérer, il faut pas qu'on vende ça comme un Postgres qu'on laisse tourner."

**Recurring names.** Jean-Michel d'astreinte à 3h du matin. "The users" and "the world" as the two sources of chaos. "Your users, but worse" and "the world, but worse". "La niche dans la niche" for FoundationDB. "Hobby grade" projects. "Accident industriel" for FoundationDB: research-paper-perfect design that somehow shipped to production.

## 5. Humor register

Self-deprecating first: "j'ai un problème d'addiction au squash", "je comprends pas pourquoi 5 millions de téléchargements, c'est pas moi qui les fais, je précise parce que c'est important pour l'histoire."

Irreverent toward vendors, docs, and process, never toward the audience: "Est-ce qu'il y a des gens qui utilisent MariaDB Galera en prod? Je suis désolé pour toi, tu vas détester cette slide." "Moi globalement je voulais me débarrasser de Windows, je tiens à préciser."

Deadpan about scale and chaos: "Qu'est-ce qui pourrait bien mal se passer quand on code ça à la main?" "Il inverse les disques durs, c'est quand même un peu rigolo comme test d'intégration."

Meta about time: "je vais accélérer parce que sinon on va me gronder", "il me reste deux minutes, je suis trop bien dans les temps."

On slides, the humor is emoji reactions to grim stats and dry one-liners under a table ("just to cover the happy path").

## 6. Evidence habits

Papers cited with title, first author, venue, year, and a link. Key findings quoted and the number bolded. One line of commentary that lands it for developers ("Tested on PostgreSQL, LMDB, LevelDB, SQLite, Redis, none handle fsync failures correctly").

Jepsen reports as the weapon against marketing claims. Tweets as screenshots (Boris Cherny on giving Claude a way to verify). Conference videos linked with timestamps (Will Wilson, Mario). His own previous talks linked by year so he can say "je ne vais pas en parler, allez voir le talk de 2023."

Production numbers from Clever Cloud as proof: rounds of simulation per PR, bug categories found, download counts, team size. He dates them ("when I joined we were 35").

## 7. Pacing

Heavy slide, then a breather with one line, then heavy again. Progressive builds are separate slides, not clicks, because he wants to talk between states.

He front-loads the story and the evidence, and accelerates through the last five minutes. Anything essential goes before the limits slide. After it: the bookend, the ladder, thanks.

Live Q&A is inline and honest: "non, par fainéantise", "je ne répondrai pas à cette question", "je prendrai les questions sur le stand."

## 8. The closing ritual

Bookend to the cold open. One-sentence thesis ("Testing must evolve from prevention to discovery", "Invest in correctness now"). Actionable table (adoption ladder). Then the end slide: thank you, booth location, slides on pierrezemb.fr, links to his OSS, QR to rate the talk when the event has one, "any questions?"

## 9. French versus English delivery

Same skeleton. French is looser: more asides, more jokes, more room polls, longer stories, and he gets ahead of the slides. English is tighter and closer to the slide text, with the accent joke up front and fewer digressions. For an English event, slides can carry slightly more of the connective tissue; for French, slides should be sparser because he will fill them.

## 10. What he does not do

- Read a slide, or keep presenter notes.
- Use click animations.
- Hedge. He says "it works" or "it does not", and cites.
- Write section-header titles ("Section 3: Testing Approaches").
- Use em dashes in text.
- Sell. Even the Clever Cloud part is told as an engineering story ("35 people, our own datacenters, was it the moment to build a database? maybe").
- Say "mock" when he means fake.

## 11. Slide writing voice (complements the repo root CLAUDE.md)

Second person, present tense, short: "Your system can't tell the difference." "Same fake from Step 1. Now it fights back." Imperatives as titles: "Be worse than production 😈", "Don't write tests, write a generator 🎰". Quoted beliefs as titles for the evidence wall. Questions for transitions. "Let's" for recipes: "Let's test the worst 😈". Bold the key phrase, never a whole sentence. Emoji as bullet anchors, one per concept, reused.
