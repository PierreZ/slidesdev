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

**Storytelling above all** — slides should feel like a coffee break with a whiteboard, not an academic paper. **No click animations** (`v-click`, `v-clicks`) — split content into separate slides instead.

### Narrative Structure
- Follow a **problem → evidence → solution → proof → call-to-action** arc
- Open with a specific war story (inciting incident), close by calling back to it (**bookending**)
- Introduce a recurring visual motif early (e.g. a diagram), then modify it throughout the talk (highlight/dim elements to shift focus)
- **Concrete before abstract**: always a story or example first, then the concept
- Build concepts incrementally — each section's output becomes the next section's input
- Include a "not a silver bullet" / limitations slide before concluding
- End with an actionable takeaway (adoption table, checklist, or spectrum)
- Alternate pace: heavy content slide → breather/transition slide → heavy content slide

### Slide Titles
- Every `# h1` ends with a trailing emoji: `# Title here 🔬`
- Titles are conversational: questions, imperatives, or quoted beliefs to bust
- Good: `# What do you believe about your system? 🤔`, `# Be worse than production 😈`, `# "The network is reliable" 🌐`
- Bad: `# Network Reliability Analysis`, `# Section 3: Testing Approaches`

### Content Density
- **Bullet slides**: 2-4 bullets, 1 line each, 1 level of sub-bullets max
- **Quote slides**: 1-2 blockquotes + 1-2 lines of commentary below
- **Code slides**: <15 lines with line highlighting (` ```java {4} `, ` ```java {5-9} `)
- **Diagram slides**: single visual element, no competing text
- **Transition slides**: 1-3 punchy lines to shift topic (often a rhetorical question)
- If a slide feels dense, **split it** — don't compress

### Emoji Conventions
- Trailing emoji on every h1 title
- Leading emoji on bullet points as visual anchors
- Assign one emoji per concept and reuse it consistently throughout the talk (e.g. 👤=users, 🌍=world, 🖥️=system)

### Formatting
- `**bold**` for key phrases (renders in accent color via theme)
- `*italics*` for paper/article titles
- Inline `[links](url)` to sources — always cite
- Code blocks with language tag + optional line highlighting
- Blockquotes (`>`) for direct quotes, always attributed

### Visual Diagrams (HTML)
- Use Tailwind HTML for structural diagrams: `flex`, `border-2`, `rounded-lg`, `gap`, `grid`
- Highlight focused elements with accent color: `style="border-color: var(--theme-accent); color: var(--theme-accent);"`
- Dim de-emphasized elements with `opacity-40` class
- Reuse the same diagram template with progressive modifications across slides — this creates the visual motif

### Tone & Voice
- Conversational: "Let's imagine...", "What if we...", "Remember our..."
- Slightly irreverent, no hedging — strong statements backed by data
- Rhetorical questions as slide titles to engage the audience
- Humor through emoji reactions to serious stats (😱, 💀, 😈)

### Research Citations
- Format: `*[Paper Title](url) — Author et al., Venue Year*`
- Follow with blockquoted key findings, **bold the key numbers/percentages**
- Add 1-2 lines of commentary that contextualizes for the audience

### Layout Guidance
- `cover` → title slide only (once, at the start)
- default (no layout) → most content slides
- `two-cols` → comparisons (code vs code, concept vs visual, tweet + diagram)
- `end` → closing slide only (once, at the end)
- Image-only slides → just `<img>` tag centered, no layout specified

## Theme Conventions

- Two-cols layout uses named slots: `::title::` (full-width header), `::default::` (left), `::right::` (right)
- Override accent color per deck via headmatter: `themeConfig: { accent: '#yourcolor' }`
- Bold text (`**text**`) renders in accent color
