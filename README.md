# slidesdev

Monorepo for [Slidev](https://sli.dev) presentations with a shared custom theme.

## Setup

```bash
nix develop   # or direnv allow
```

## Usage

```bash
cd talks/<name>
pnpm install
pnpm dev      # dev server at localhost:3030
pnpm export   # export to PDF
```

## Talks

| Talk | Description |
|------|-------------|
| `demo` | Theme demo |
| `prevention-vs-discovery` | Testing: Prevention vs Discovery |

## CI

PDF exports are generated on every push to `main` via GitHub Actions and uploaded as artifacts.
