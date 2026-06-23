# Simulation-Driven Development (DST talk)

## Context
- **Source**: Pierre's Devoxx France 2025 talk "What if we embraced simulation-driven development?" (`~/Downloads/Devoxx FR 2025_ DST-2.pdf`, 47 slides). This deck backports that flow.
- **Event**: Sunny Tech. Accent is Sunny Tech's brand-solid red `#d63f49` (secondary `#901f26`, their pink-3). Footer `#SunnyTech`.
- **Format**: ~45 min, about 51 slides.

## Narrative spine
One human thread ties the talk together: **"will it break production?"**
- On-call gave me production intuition (scars).
- I cannot transplant scars into juniors.
- Now an LLM writes the code too.
- Juniors and LLMs share the same problem: neither knows if the code breaks prod.
- DST is the feedback loop that answers it for both, before prod does.

AI enters as an amplifier at the problem-to-solution hinge (slides 17-18) and pays off after the Materia demo (slides 45-47). The on-call war story is the cold open and must stay first. The deck closes by bookending back to the Hadoop NPE.

## Conventions
- **No em dashes** anywhere (Pierre's rule). Use commas, parentheses, colons, separate sentences.
- **Fakes, not mocks**: the deck argues fakes beat mocks and testcontainers. Don't reintroduce "mock" in titles.
- Code: **Java** for app-level examples (generator, property, fakes), **TOML** for the Materia workload config, **C++** for the FDB disk-swap snippet.
- No `v-click`; progressive reveals are separate slides (the "What do we want from a test?" build and the TOML config walkthrough use Slidev line-highlighting).
- Recurring visual motif: "Your system" box wired to "Your users" / "The world" cloud pills, progressively highlighted in accent-red.

## Images
Real assets present: `public/materia-sim-{single,triple,ci}.png` (the two seed runs + the CI bot).
Everything else is a **dashed-box placeholder** for Pierre to fill in. Drop a PNG into `public/` and replace the box with `<img src="/name.png" ...>`. Placeholders, with suggested filenames:
- `/pierre.png` (whoami photo), `/days-since-npe.png`
- `/datacenter.png`, `/osdi-tables.png`, `/network-config-meme.png`, `/murder-mystery.png`
- `/generator-meme.png`, `/property-meme.png`
- `/mario.png`, `/materia.png`
- `/feedback-loop.png` (boris-cherny tweet), `/claude-moonpool.png`
- cover background art (HTML comment on the cover slide)

## Commands
`nix develop ../.. --command pnpm install` then `pnpm dev` / `pnpm build` / `pnpm export`. Lockfile pins `@slidev/cli 52.14.1` (52.16.0 breaks the public-asset `<img>` build).
