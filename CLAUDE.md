# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static linktree page for icamp (i.camp) — a dark-mode, single-page HTML site with no build step required. The entry point is `index.html` at the root.

## Stack

- **HTML/CSS** — plain static file, no bundler or framework runtime
- **Tailwind CSS v3** — loaded via CDN (`https://cdn.tailwindcss.com`); custom tokens are defined inline in a `tailwind.config` script block inside `index.html`
- **Fonts** — Google Fonts CDN: `IBM Plex Mono` (numbers, monospaced subtitles); `Arial` is the body/display font. `Radiospace` (title only) loaded from `fonts/radiospacebold.ttf`
- **No JavaScript framework** — React is listed as a dependency but is not used

## Running the Page

Open `index.html` directly in a browser — no build step needed. For live-reload:

```bash
npx serve .
# or
python3 -m http.server 8080
```

## Design Tokens

Defined inside the `tailwind.config` script block in `index.html` (not as CSS custom properties):

| Token | Hex | Use |
|---|---|---|
| `surface` | `#000000` | Page background |
| `on-surface` | `#080C0D` | Card background |
| `on-on-surface` | `#141D1F` | Card hover background |
| `accent` | `#90F1F8` | Cyan accent, numbers, hover states |
| `accent-hover` | `#7AF6FF` | Accent on active hover |
| `brand-border` | `#123133` | Default border |
| `text-sec` | `#ACB4B5` | Subtitles, social icons default |

## Architecture

Everything lives in a single `index.html`. Layout inside `<body>`:

```
canvas#bg-canvas          — terminal feed animation (fixed, z-index 1)
div.relative.z-10         — main content column (max-w-[400px])
  .header                 — Radiospace title "i.camp" + tagline with blinking cursor
  .divider
  .links (4 cards)        — Tau (tauprofile.com), Bytes (bytes.i.camp),
                            Terminal (terminal.i.camp), Icamp (i.camp)
div.fixed.bottom-4        — social icons row + copyright (fixed to viewport bottom)
  social icons ×8         — X, GitHub, Instagram, LinkedIn, Reddit, Medium, Email, Farcaster
  copyright               — dynamic year via JS
```

## Custom CSS (in `<style>` block)

Only handles things Tailwind can't:
- `@font-face` for `Radiospace` and (unused) `SFProverbial`
- `body::before` — radial cyan glow
- `body::after` — noise grain overlay (z-index 3)
- `.logo-cursor` — blinking cursor animation
- `.link-card::before` — left accent bar that scaleY(1) on hover
- `.a1`–`.a7` — staggered `fadeUp` entrance animations

## Canvas Animation

The terminal feed background is a vanilla JS canvas animation in the `<script>` block at the bottom of `<body>`. It renders 3–6 scrolling columns of CS-themed text lines. Key constants: `BASE_ALPHA = 0.11`, `ACCENT_ALPHA = 0.30`, `FADE_ZONE = 0.22`. Columns are rebuilt on resize.

## Favicons

- `favicon.png` — square 1×1 icon (browser tab)
- `favicon-18x24.png` — tall terminal icon (Apple touch icon)

## If Switching to a Build Pipeline

`tailwind.config.js` is present but unused. To switch from CDN to compiled Tailwind:

1. Add Vite or another bundler
2. Create `src/index.css` with `@tailwind` directives
3. Remove the CDN `<script>` tag and import the compiled CSS instead
