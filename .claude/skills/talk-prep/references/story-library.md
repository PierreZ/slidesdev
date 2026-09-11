# Story library: Pierre's reusable material

Everything here has been used on stage at least once. Offer it, do not assume it; he decides what to reuse. Ask for current numbers, the ones below are as told at the time.

## War stories (cold opens and proofs)

**Hadoop NPE at recovery (OVHcloud).** 70 node Hadoop cluster, critical for the company. Violent network partition. Cluster tries to heal, then hits full disks on journal nodes, "se met sur le dos, tel une tortue". Add disk, nothing. Reboot, NullPointerException at startup on the whole cluster: the internal state was irreparable. Known bug, fixed in a newer HDFS. Upgrading was too risky, so backport, recompile, redeploy on 70 machines carrying data. Lesson: the bug hits at the worst moment, during recovery; the code never accounted for that combination. Bookend: "that exact combination is a seed in a simulation." Default cold open for testing, correctness, reliability talks.

**HBase hbck trauma.** 270 machines, 2.5M writes/s, 6.5M reads/s. Network split during a region split leaves the cluster inconsistent. The repair tool moves the partitioned data out of the keyspace to make it "consistent". Why he went looking for FoundationDB.

**The disk pulled by hand.** In the Paris datacenter a machine vanished from a FoundationDB cluster. Inventory mistake, someone physically pulled the disk. No incident, no worry, the cluster re-replicated. "On aurait arraché un disque dans d'autres systèmes qu'on connaît, on aurait eu un peu plus de gestion d'incident."

**The 14,000 line PR.** A 200 line feature with huge product value took two and a half months because 13,000 lines of tests were needed to cover its combinations with the 36 existing features. Punchline for "tests don't scale".

**The unanswered contributions.** Lockdown 2020, PRs opened on 31 December, no reply for six months, CI broken, repo dead but 20k downloads a day. Email from the ex-maintainer: he left the company, nobody else has access, but he still has crates.io publish rights and the license is MIT/Apache. Hard fork into a neutral org (`foundationdb-rs/foundationdb-rs`) so it never depends on one company again. First release April 2022. Today 5M+ downloads, 16k lines of Rust, 30 contributors, one main maintainer.

**The team that was not convinced.** First workloads only checked KV integrity and statistics, "essentially testing FDB's transactional promises", boring. As workloads got richer, simulation found bugs everywhere. Each engineer found a bug in their own code, or shipped a low-level piece quickly and correctly, and switched. Now everything is simulation-first.

**The newcomer.** Joined in January, one month to learn the codebase, shipped a deep Materia feature in one week with all edge cases handled, because the simulator taught him the indexes and permission rules he did not know existed.

**The flame graph.** A Java streaming job at 137% CPU on 8 cores, backpressure everywhere. Flame graph: 60% deserialization, 20% serialization. "Je passais mon temps à cramer du CPU pour sérialiser."

**JS sort quiz.** Two ways to sort an array of objects by name. Which is faster? Hands up for each. Nobody measured. "On ne sait pas parce qu'on n'a pas mesuré."

## Analogies

- Dev versus prod is the driving theory test versus driving in Paris rush hour. "Je suis content d'avoir passé mon permis à Brest."
- The system interacts with two things: your users (they do weird things) and the world (network, disk, time, dependencies). Test both, but worse.
- Rust ownership: lending a tool to a colleague, you know who has it, and it comes back half broken if it was mutable.
- Code as fast food (João Alves): cheap, fast, everywhere. What stays expensive: knowing what to build, knowing if it works, knowing when it breaks.
- Incidents as the butterfly effect: the on-call job is finding the butterfly.
- Monitoring is red or green; observability is how well it works.
- A failing seed is a time machine.
- "Give a computer a test, it finds a bug. Teach a computer to test, it finds bugs forever."
- FoundationDB as an industrial accident: research-grade design (own language, actor model, simulator written before the database) that somehow shipped and is usable.
- "La niche dans la niche": FoundationDB inside distributed systems.

## One-liners (French and English)

- Trouver ce que vous ne savez pas / discover what you don't know.
- Soyez pire que votre production / be worse than production.
- You test what you imagine. Bugs hide in combinations you didn't.
- Tests are a regression net, not a proof that bugs are absent.
- Don't write tests, write a generator. Don't write assertions, write properties.
- Same interface, two implementations, your system can't tell the difference.
- Make the fake fight back.
- On ne fait pas du test-driven development, on fait du simulation-driven development.
- You don't trust Claude, you trust the simulator.
- AI broke "good enough": we used to understand it, write it, try it, get paged by it. Only the pager is left.
- The age of correctness has started. Invest in correctness now.
- Notre but c'est pas d'écrire le logiciel parfait, ça existe pas.
- Si vous voulez pas embêter Jean-Michel à 3h du matin...
- Reboot in doubt fixes the problem and teaches you nothing.
- Software creates value when it runs, not when you write it.
- Écrire du logiciel qui répond aux questions par lui-même.

## Numbers he uses

| Number | Context |
|---|---|
| 648, then 3,840 | Checkout test cases, before and after one feature per dimension |
| 200 lines, 14,000 line PR | Tests don't scale |
| 70 nodes | Hadoop NPE |
| 270 machines, 2.5M w/s, 6.5M r/s | HBase |
| 4,000 years of simulation per year | Apple, FoundationDB |
| 20,000 simulation rounds per PR, two hours | Apple and Snowflake |
| 10,000 rounds per PR, then a farm hunting continuously | Clever Cloud Materia |
| 3 hours of simulation per low-level PR | Materia, as told in the launch interview |
| 30 min of sim is about 24 h of chaos | Materia |
| 10, then 50, then 500 rounds to feel safe; one bug at 250,000 | Bug-finding latency |
| 219 days of bug hunting per month | foundationdb-rs binding tester on GitHub Actions |
| 5M+ downloads, 20k/day, 16k lines, 30 contributors | foundationdb-rs |
| 1 to 6 people, almost none with database internals | Materia team |
| 35 people when he joined, ~90 now | Clever Cloud |
| 10M ops/s on ~400 cores, linear | FoundationDB scalability |
| 895 databases | Choosing a database, 2023 |

## Evidence with links

Papers (format on slides: `*[Title](url), Author et al., Venue Year*`; the repo root CLAUDE.md shows an em dash there, the 2026 decks use a comma):

| Belief to bust | Source | Headline |
|---|---|---|
| The network is reliable | [Network-Partitioning Failures, Alquraan et al., OSDI 2018](https://www.usenix.org/system/files/osdi18-alquraan.pdf) | 80% catastrophic, 27% data loss, 90% silent, 83% need 3+ events |
| My data is safe on disk | [Data Corruption in the Storage Stack, FAST 2008](https://www.usenix.org/legacy/events/fast08/tech/full_papers/bairavasundaram/bairavasundaram.pdf), [SSD Reliability, FAST 2020](https://www.usenix.org/system/files/fast20-maneas.pdf) | Silent corruption 1.4% of enterprise HDDs per year |
| fsync means saved | [Can Applications Recover from fsync Failures?, Rebello et al., ATC 2020](https://www.usenix.org/system/files/atc20-rebello.pdf) | None of Postgres, LMDB, LevelDB, SQLite, Redis handle it |
| Consensus recovers from crashes | [Protocol-Aware Recovery, Alagappan et al., FAST 2018](https://www.usenix.org/system/files/conference/fast18/fast18-alagappan.pdf) | ZooKeeper recovers 46 of 2,401 corruptions |
| 3 replicas means safe | [Redundancy Does Not Imply Fault Tolerance, Ganesan et al., FAST 2017](https://www.usenix.org/system/files/conference/fast17/fast17-ganesan.pdf) | One corrupted Kafka entry takes the cluster down |
| We handle all our errors | [Simple Testing Can Prevent Most Critical Failures, Yuan et al., OSDI 2014](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf) | 92% of catastrophic failures from mishandled non-fatal errors, 77% reproducible by a unit test |
| No concurrency bugs | [TaxDC, Leesatapornwongsa et al., ASPLOS 2016](https://ucare.cs.uchicago.edu/pdf/asplos16-TaxDC.pdf) | 64% triggered by untimely messages |
| We just retry | [Metastable Failures in the Wild, Huang et al., OSDI 2022](https://www.usenix.org/system/files/osdi22-huang-lexiang.pdf) | Retry policy sustains >50% of incidents |
| The docs are right | [Jepsen: MariaDB Galera 12.1.2](https://jepsen.io/analyses/mariadb-galera-cluster-12.1.2) | Lost updates and stale reads in healthy clusters |
| LLM code quality | [SWE-CI, Chen et al., 2026](https://arxiv.org/pdf/2603.03823); [FT on Amazon AI-related outages, March 2026](https://www.ft.com/content/7cab4ec7-4712-4137-b602-119a44f771de) | Zero-regression rate below 0.25 |

Other sources:

- Will Wilson, [Testing a Single-Node, Single Threaded, Distributed System Written in 1985](https://www.youtube.com/watch?v=m3HwXlQPCEU) (Antithesis beats Super Mario Bros, finds a wall clip). Warn that Antithesis is very expensive.
- Boris Cherny's tweet on giving Claude a way to verify its work (screenshot `boris-cherny-tweet.png` in existing decks).
- João Alves, [When software becomes fast food](https://world.hey.com/joaoqalves/when-software-becomes-fast-food-23147c9b).
- Google Trends for property-based testing spiking mid-2025 (image on pierrezemb.fr, bugbash-2026 post).
- Pierre's own posts: [Diving into FoundationDB simulation](https://pierrezemb.fr/posts/diving-into-foundationdb-simulation/), [Writing Rust FDB workloads that find bugs](https://pierrezemb.fr/posts/writing-rust-fdb-workloads-that-find-bugs/), [Designing fakes that prove correctness](https://pierrezemb.fr/posts/designing-fakes-that-prove-correctness/).
- Brendan Gregg's USE method, Tom Wilkie's RED method, Google SRE book four golden signals (observability talk).

## Proof assets from Clever Cloud

- Materia: multi-tenant, multi-model, serverless database on FoundationDB in Rust. Products and layers: KV (Redis and GraphQL compatible), etcd for the Kubernetes product, KMS, workflow engine, leader election, document store, a Kafka layer as the CTO's side project. Virtualized logical databases per tenant, like Apple's CloudKit.
- Bugs found by simulation: query execution returning wrong data, planner picking the wrong index, corruption during reindexing, dual leader under clock skew, etcd compaction deleting live data, TTL in the Redis shim.
- Screenshots in existing decks: `materia-sim-single.png`, `materia-sim-triple.png`, `materia-sim-ci.png` (the TUI with seed, network splits, timeline).
- foundationdb-rs: first binding outside C++ with simulation support; upstream PRs for a [pure C workload API](https://github.com/apple/foundationdb/pull/11288) and [delay()](https://github.com/apple/foundationdb/pull/12357). Binding tester runs hourly with a seed, Python as the reference implementation, "Apple burns CPU for me".
- moonpool: his own DST framework in Rust, [github.com/PierreZ/moonpool](https://github.com/PierreZ/moonpool), with a screenshot of Claude fixing a bug from a deterministic replay.
- Automation stories: dependabot merges trusted because of the test battery, release-please automating a four-crate ordered release he did by hand for years ("j'ai attendu trop longtemps").

## Previous talks (link them instead of re-explaining)

- FoundationDB intro, Sunny Tech 2023, with Steven: https://www.youtube.com/watch?v=cChMz4m8w5A
- Distributed DBs with FDB and Rust, Sunny Tech 2024: https://www.youtube.com/watch?v=Q_8CRjf3M24
- Borrowing FDB's simulator, BugBash 2026: https://www.youtube.com/watch?v=tTxY8IbT88A
- What if we embraced simulation-driven development? Devoxx France 2025, reworked for Sunny Tech 2026 (`talks/simulation-driven-development/`)
- Testing: Prevention vs Discovery (`talks/prevention-vs-discovery/`), the long form with one slide per paper
- How on-call changed the way I develop (observability), Devoxx France, Google Slides
- From contributor to maintainer of foundationdb-rs, FinistDevs, Google Slides
