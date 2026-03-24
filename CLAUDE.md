# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Monorepo for Slidev presentations with a shared custom theme (`slidev-theme-pz`). Each talk is an independent Slidev project under `talks/`.

## Development Environment

Nix flake provides Node.js 22, corepack (pnpm), and slidev-cli. Enter with `nix develop` or automatically via direnv.

## Commands

```bash
# Run a presentation (from a talk directory, e.g. talks/demo/)
pnpm dev              # Dev server at localhost:3030
pnpm build            # Build static SPA
pnpm export           # Export to PDF (needs playwright-chromium)

# Theme development — edit theme/ files, changes are live via pnpm link
```

## Architecture

- **`theme/`** — `slidev-theme-pz`, a light-only theme with configurable accent color (`--theme-accent` CSS variable, default `#F57C00`). Provides `cover`, `end`, and `two-cols` layouts. Fonts: Roboto + Fira Code.
- **`talks/{name}/`** — Each talk is a standalone Slidev project with its own `package.json` that links to the theme via `"slidev-theme-pz": "link:../../theme"`. Slides live in `slides.md`.
- **Footer bar** — Not part of the theme. Each talk optionally creates its own `global-bottom.vue` with hashtags and slide number.

## Creating a New Talk

1. Create `talks/{name}/` with a `package.json` depending on `@slidev/cli` and `slidev-theme-pz: link:../../theme`
2. Create `slides.md` with `theme: slidev-theme-pz` in headmatter
3. Optionally add `global-bottom.vue` for a footer bar
4. Run `pnpm install` then `pnpm dev`

## Slide Conventions

- **Storytelling above all** — slides should feel like a coffee break with a whiteboard, not an academic paper
- **No click animations** (`v-click`, `v-clicks`) — split content into separate slides instead if a slide is too dense
- Use emojis liberally on most slides (section headers, bullets, titles)
- Keep slides punchy: 2-3 bullet points max per slide, split if denser
- Use recurring visual motifs to create a narrative thread

## Theme Conventions

- Two-cols layout uses named slots: `::title::` (full-width header), `::default::` (left), `::right::` (right)
- Override accent color per deck via headmatter: `themeConfig: { accent: '#yourcolor' }`
- Bold text (`**text**`) renders in accent color
