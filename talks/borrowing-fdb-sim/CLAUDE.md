# Borrowing FoundationDB's Simulator for Layer Development

## Talk context
- **Event**: BugBash 2026
- **Duration**: 10 minutes (lightning talk)
- **Speaker**: Pierre Zemb, French speaker presenting in English
- **Audience**: Simulation testing / correctness crowd. ~75% know FDB. Interested in techniques, not product catalogs.

## Key narrative
We build serverless databases (KV, KMS, ETCD, workflow engine) on top of FoundationDB at Clever Cloud. FDB core is battle-tested via simulation — our Rust layers were not. We figured out how to compile our Rust code as a `.so` and load it inside FDB's own simulator, running under full chaos. One of two teams in the world doing this (the other is an EU cloud provider, but not at the same scale).

## Reference material
- Blog: https://pierrezemb.fr/posts/diving-into-foundationdb-simulation/ (how FDB sim works: Sim2/Net2 swap, deterministic execution, seed-based replay)
- Blog: https://pierrezemb.fr/posts/writing-rust-fdb-workloads-that-find-bugs/ (three-crate pattern, workload design)
- Local crate: ~/workspace/rust/foundationdb-rs/foundationdb-simulation/ (RustWorkload trait, C FFI bridge, examples)
- FDB docs: https://apple.github.io/foundationdb/client-testing.html (external workload API)
- Upstream PRs: C workload bindings (#11288), delay() API (#12357)
- Forum: https://forums.foundationdb.org/t/externalworkload-segmentation-fault-in-7-3/4375 (C++ ABI nightmare that motivated the C API)

## Design decisions
- No v-click animations — split into separate slides instead
- Recurring visual motif: layer/core stack diagram, evolving across slides
- Diagrams as Tailwind HTML (not Mermaid) for consistency with theme
- Keep RustWorkload code block — it's the API surface
- No LLM feedback loop — save for the prevention-vs-discovery talk
- Accent color: #007AFF (FDB blue)
