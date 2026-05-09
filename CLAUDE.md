# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A fully static personal link hub (Linktree-style) for Phillip Bösger. There is no build system, no package manager, and no Node.js. To preview locally, open `index.html` directly in a browser. Deployment to GitHub Pages happens automatically on every push to `main` via `.github/workflows/updateWebsite.yml`.

## Architecture

The page is a single `index.html` backed by one CSS file and one active JS file:

- **`index.html`** — All content lives here: profile section, section labels, and link cards. The footer year is set by an inline `<script>` at the bottom.
- **`assets/css/style.css`** — All styling. Colors and spacing are driven by CSS custom properties defined in `:root`. Mobile adjustments use a single breakpoint at `max-width: 540px`.
- **`assets/js/network.js`** — An IIFE that draws an animated particle-network on `<canvas id="network-canvas">`, rendered behind all content at `z-index: 0`.
- **`assets/js/snowfall.js`** — An alternative snowfall animation (currently **not loaded**). It targets `#bg-canvas` (which does not exist in the current HTML) and bundles a minified copy of Underscore.js. It can be enabled by swapping the canvas ID and script tag.

## CSS Custom Properties

All theme values are in `:root` in `style.css`:

| Variable | Purpose |
|---|---|
| `--bg` | Page background (`#020c18`) |
| `--bg-card` / `--bg-hover` | Card background / hover state |
| `--cyan` / `--blue` | Primary accent colors |
| `--border` / `--border-h` | Border color / hover border |
| `--text` / `--text-dim` | Primary and muted text |
| `--radius` | Link card border radius (`14px`) |

## HTML Conventions

**Link card pattern** (copy-paste to add a link):
```html
<a href="https://example.com" target="_blank" rel="noopener" class="link-card">
  <img src="assets/images/icon.png" alt="Label" />
  <span>Label</span>
  <svg class="arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
    <path d="M7 17L17 7M17 7H7M17 7v10" />
  </svg>
</a>
```

For links to private/internal services, use `rel="nofollow noopener"` instead of `rel="noopener"`.

**Section label** (groups links visually):
```html
<p class="section-label">Section Name</p>
```

## Images

All icons live in `assets/images/`. Recommended formats: PNG or WEBP with transparent background. Keep images under ~300 KB. Link card icons are displayed at `34×34 px`.
