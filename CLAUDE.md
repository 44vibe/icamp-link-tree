# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static linktree page for icamp (i.camp) — a dark-mode, single-page HTML site with no build step required. The entry point is `index.html` at the root.

## Stack

- **HTML/CSS** — plain static file, no bundler or framework runtime
- **Tailwind CSS v3** — installed as a devDependency but the current `index.html` uses the **Tailwind CDN** (`https://cdn.tailwindcss.com`), not a compiled stylesheet
- **Fonts** — Google Fonts CDN: `IBM Plex Mono` (logo, numbers, subtitles) + `Outfit` (link titles)
- **No JavaScript framework** — React is listed as a dependency but is not currently used

## Running the Page

Open `index.html` directly in a browser — no build step needed. If you want a live-reload dev server:

```bash
npx serve .
# or
python3 -m http.server 8080
```

## Design Tokens

All colors are defined as CSS custom properties in `index.html`:

| Token | Hex | Use |
|---|---|---|
| `--surface` | `#000000` | Page background |
| `--on-surface` | `#080C0D` | Card background |
| `--on-on-surface` | `#141D1F` | Card hover background |
| `--accent` | `#90F1F8` | Cyan accent, numbers, hover states |
| `--accent-hover` | `#7AF6FF` | Accent on active hover |
| `--border` | `#123133` | Default border, footer text |
| `--text-primary` | `#FFFFFF` | Headings, link titles |
| `--text-secondary` | `#ACB4B5` | Subtitles, social icons default |

## Architecture

Everything lives in a single `index.html`. Structure inside `<body>`:

```
.page
  .header          — logo (i.camp + blinking cursor) + tagline
  .social-row      — 8 social icon links (X, GitHub, Instagram, LinkedIn, Reddit, Notion, Email, Facebook, Medium)
  .divider
  .links
    .link-card ×4  — i.camp, Terminal, Bytes, Tau Profile (all href="#" placeholders)
  .footer
```

## Updating Links

All link `href` values are set to `"#"` as placeholders. Replace them directly in `index.html` on the `<a>` tags inside `.social-row` and `.links`.

## If Switching to a Build Pipeline

The `tailwind.config.js` and `postcss.config.js` are already configured and point to `./src/**/*.{js,jsx,ts,tsx}` and `./index.html`. To switch from CDN to compiled Tailwind:

1. Add Vite or another bundler
2. Create `src/index.css` with `@tailwind` directives
3. Remove the CDN `<script>` tag and import the compiled CSS instead
