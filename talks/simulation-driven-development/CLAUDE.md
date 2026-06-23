# Simulation-Driven Development (DST talk)

## Context
- **Source**: Pierre's Devoxx France 2025 talk "What if we embraced simulation-driven development?", reworked for the AI era.
- **Event**: Sunny Tech. Accent is Sunny Tech's brand-solid red `#d63f49` (secondary `#901f26`, their pink-3). Footer `#SunnyTech`.
- **Format**: ~45 min, about 49 slides.

## Narrative spine: "the world has changed, the correctness decade has begun"
1. War story (Hadoop NPE): this is how I learned what my code does, by **operating** it.
2. But it is 2026, the world has completely changed: an LLM writes the code now.
3. The old grounds for "good enough" are gone (Klabnik: we wrote it, understood it, tried it, AI broke all three), so correctness becomes the bottleneck and everyone suddenly cares (Amdahl). One slide now carries this.
4. How do you trust code you did not write? Confidence comes from understanding, understanding lives in behaviors (Moreira).
5. You witness behaviors two ways: operate it, or simulate it. Simulation witnesses them before prod, at agent speed.
6. Why naive testing cannot give that understanding (combinatorial users; a wall of research papers on how production really breaks).
7. The recipe (DST), simulate the users, simulate the world, fakes.
8. Proof (Materia). The loop closes the same for humans and agents (one merged Claude slide).
9. The correctness community's moment (Wilson). The correctness decade is here.

The on-call war story stays the cold open (recast as "how I learned behaviors"). The deck bookends back to the Hadoop NPE near the end.

## Reductions applied (the "reduce SRE" direction)
- Removed the SRE-vs-SWE two-cols slide (replaced by Moreira's "operate or simulate").
- Removed the standalone juniors slide (folded into "operate or simulate" and the AI payoff).
- Collapsed the multi-slide world-is-hostile section (Google stat, network-reliable, OSDI detail, microservices tweet) into one **wall-of-papers** slide ("The world is worse than your tests"), with 9 beliefs each linked to a paper (sourced from `talks/prevention-vs-discovery/slides.md`).
- **LLM framing condensed**: front cluster 5 slides to 3 (folded "everyone cares about correctness" + "the correctness decade has begun" into "AI broke good enough"); back Claude cluster 3 slides to 1 ("you don't trust Claude, you trust the simulator"); trimmed scattered AI bullets.
- **Voice restored**: the opening story slides ($whoami, the NPE war story, "what went wrong") were over-backported from the spoken transcript; restored to the original written slide bullets. Dropped the "black cat" persona everywhere (a "this is fine" gif slide follows $whoami instead).

## Keynote source map (BugBash 2026, absorbed as Pierre's argument, light attribution)
- Klabnik, "Steel, Rust, and truth": "wrote it / understood it / tried it, AI broke all three"; "every programmer is about to need what you already know"; "meaning leaks" (proof has a gap to reality).
- Will Wilson, "We won, what now?": the trend-graph spike ("everybody started caring all of a sudden"); Amdahl's law ("50% then 99%"); "winning looks like the world co-opting your thing"; "our moment to shine".
- Gabriela Moreira: "correctness, confidence, understanding, behaviors"; "confidence is not a checkmark"; behaviors witnessed by operating or simulating.
- Carl (Antithesis): example-based tests "only test what you think about", agents are confident about what they test; PBT is "now for everyone"; the loop reruns the whole suite after a fix.

## Conventions
- **No em dashes** anywhere (Pierre's rule). Commas, parentheses, colons, separate sentences.
- **Fakes, not mocks**: the deck argues fakes beat mocks and testcontainers. Do not reintroduce "mock" in titles.
- Code: **Java** for app-level examples, **TOML** for the Materia workload config, **C++** for the FDB disk-swap snippet.
- No `v-click`; progressive reveals are separate slides (the "What do we want from a test?" build, the TOML config walkthrough use Slidev line-highlighting).
- Recurring visual motif: "Your system" box wired to "Your users" / "The world" cloud pills, progressively highlighted in accent-red.
- Compact tables use a scoped `<style>` (`.ecom`, `.papers`) to shrink font and padding so they fit.

## Images
Real assets present: `public/materia-sim-{single,triple,ci}.png`.
Everything else is a **dashed-box placeholder** for Pierre to fill in (drop a PNG into `public/`, replace the box with `<img>`). Placeholders:
- `/pierre.png`, `/days-since-npe.png`, `/this-is-fine.gif` (gif slide right after $whoami)
- `/generator-meme.png`, `/property-meme.png`
- `/mario.png`, `/materia.png`
- `/claude-moonpool.png`
- cover background art (HTML comment on the cover slide)

## Commands
`nix develop ../.. --command pnpm install` then `pnpm dev` / `pnpm build` / `pnpm export`. Lockfile pins `@slidev/cli 52.14.1` (52.16.0 breaks the public-asset `<img>` build).
