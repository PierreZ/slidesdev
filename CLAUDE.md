# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Monorepo for Slidev presentations with a shared custom theme (`slidev-theme-pz`). Each talk is an independent Slidev project under `talks/`.

## Development Environment

Nix flake provides Node.js 22, corepack (pnpm), slidev-cli, and Playwright Chromium (for PDF export). Enter with `nix develop` or automatically via direnv.

## Commands

```bash
# Run a presentation (from a talk directory, e.g. talks/demo/)
pnpm dev              # Dev server at localhost:3030
pnpm build            # Build static SPA
pnpm export           # Export to PDF (Playwright provided by Nix flake)

# Theme development — edit theme/ files, changes are live via pnpm link
```

## Slidev MCP

The `slidev` MCP server (`.mcp.json`, HTTP at `http://localhost:3030/__mcp`) talks to the
running dev server. Use it instead of reading or grepping `slides.md` by hand: `slidev-get-info`
(which deck is live, slide count), `slidev-list-slides`, `slidev-get-slide`, `slidev-goto-slide`
(navigate the browser to a slide to check it visually), and the editing tools
`slidev-insert-slide`, `slidev-update-slide`, `slidev-move-slide`, `slidev-remove-slide`.

It only works while `pnpm dev` runs in a talk directory, and it targets that one deck. If the
tools are unreachable, ask which talk to start, then fall back to editing `slides.md` directly.

## CI/CD

GitHub Actions workflow (`.github/workflows/export-pdf.yml`) exports all talks to PDF on every push to `main` (when `talks/` or `theme/` change). Uses `nix develop` for environment parity. PDFs are uploaded as artifacts with 30-day retention.

To add a new talk to CI, add its directory name to the matrix in the workflow file. The export runs `slidev export --executable-path` with the Chromium from the flake: a deck on a recent `@slidev/cli` pulls a `playwright-chromium` whose expected browser build is newer than the one `playwright-driver.browsers` ships, and the bundled lookup then fails.

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
- Open with a specific war story (inciting incident), or with a dated first-person timeline (`# 2010: ...`, `# 2017: ...`, `# 2024: ...`)
- **A slide is an event, not an argument.** Thesis slides ("you own what ships", "not a silver bullet", "a factory needs an inspector") get cut in every editing pass. What survives is a year, a report, a tweet, a screenshot, a list of papers, a video
- **Bookend with content, not with a slide.** Plant a "before" slide whose bullets the closing slide mirrors one for one (same emoji, same order). A "Remember X?" callback slide gets cut
- **Concrete before abstract**: always a story or example first, then the concept
- Build concepts incrementally: each section's output becomes the next section's input
- **Group by subject, not by punchline.** A demo of technique X sits with the other X slides even if it would land harder later. A community plug comes after whoami, once the room knows who is speaking
- **The last line of a slide is the handoff.** The rhetorical question or punchline goes at the bottom, right before the slide that answers it
- **Rhythm is text, image, text, image.** Two text slides in a row on the same subject "break rhythm" and get merged
- For deep technical rooms only (BugBash, Devoxx): a recurring visual motif modified across slides, a "not a silver bullet" slide, an adoption table close. For a mixed room all three were built and all three were cut; skip them

### Slide Titles
- Every `# h1` ends with a trailing emoji: `# Title here 🔬`
- Titles are conversational: questions, imperatives, or quoted beliefs to bust
- Good: `# What do you believe about your system? 🤔`, `# Be worse than production 😈`, `# "The network is reliable" 🌐`
- Bad: `# Network Reliability Analysis`, `# Section 3: Testing Approaches`

### Content Density
- **Bullet slides**: 2-4 bullets, 1 line each, 1 level of sub-bullets max. The second clause after a comma is the first thing to cut
- **Image slides**: one visual per beat, talked over. Image plus caption plus bullets is "too much"; at most one short caption or citation line under the image
- **Context becomes a sub-bullet, a beat becomes a slide.** Fold background (why this field, the book to read) under an existing bullet; give a missing story beat its own slide
- **One slide per named thing** (a tool, a project, a person). If it spans two slides, merge and push the overflow to voice; it fits every time
- **Mechanism is spoken, output is shown.** For a mixed room no protocol, no simulator internals, no test methodology on screen: show the report, the failing seed, the TUI
- **Numbers only as punchlines, one per slide.** "4,000 years", "130+ evenings" stay; cluster sizes, throughput, seed and commit counts go to voice
- **Quote slides**: 1-2 blockquotes + 1-2 lines of commentary below
- **Code slides**: <15 lines with line highlighting (` ```java {4} `, ` ```java {5-9} `)
- **Transition slides**: 1-3 punchy lines to shift topic (often a rhetorical question)
- If a slide holds two beats, **split it**. If it holds one beat and too many words, **push words to voice**. Never compress

### Emoji Conventions
- Trailing emoji on every h1 title
- Leading emoji on bullet points as visual anchors
- Assign one emoji per concept and reuse it consistently throughout the talk (e.g. 👤=users, 🌍=world, 🖥️=system)

### Formatting
- `**bold**` for key phrases (renders in accent color via theme)
- `*italics*` for paper/article titles
- Inline `[links](url)` to sources — always cite
- Code blocks with language tag + optional line highlighting
- Blockquotes (`>`) for direct quotes, always attributed, always verbatim (editorial additions in square brackets: "foundation[DB]")
- First-person claims must be literally true to Pierre's experience: "working on", not "working around"; "like we did at Clever Cloud", not "like Clever Cloud"; "what I missed", not "wrong"

### Sources
- **Link primary sources, restate opinions.** Papers, books, Jepsen reports and Pierre's own talks are linked and named on the slide. Social posts, vendor blogs and other people's talks are restated in first person with no author
- At most one named quote per act (a tweet screenshot, or a blockquote with attribution). The rest is Pierre's voice

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

### Pierre's Editing Passes
- Pierre edits `slides.md` directly between sessions and drops `TODO:` lines that state a feeling ("this part is confusing", "move this under that"), not a fix. First move on any deck: `grep -n TODO slides.md` and `git diff slides.md`
- His uncommitted edits are decisions, not drafts: reconcile around them, never revert or reword them
- Propose first, apply on "go", "do all" or "fix all". Verify on a rendered export (`slidev export --format png`), not on the markdown
- Every pass records what changed and why in the talk's own `CLAUDE.md`

## Theme Conventions

- Two-cols layout uses named slots: `::title::` (full-width header), `::default::` (left), `::right::` (right)
- Override accent color per deck via headmatter: `themeConfig: { accent: '#yourcolor' }`
- Bold text (`**text**`) renders in accent color
