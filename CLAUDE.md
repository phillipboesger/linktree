# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A fully static personal hub/link-tree site for Phillip Bösger (Bösger Digital) — hero, work/portfolio, blog teaser, and a "Connect" link hub. There is no build system, no package manager, and no Node.js — no React/JSX, everything is hand-authored HTML/CSS/vanilla JS. To preview locally, open `index.html` directly in a browser (or serve the folder statically). Deployment to GitHub Pages happens automatically on every push to `main` via `.github/workflows/updateWebsite.yml`.

This repo is the canonical `boesger.com` root, and it is deliberately a **thin routing/hub page**, not a content duplicate of the other properties. `digital.boesger.com` (Hugo site) is the full-depth site — About Me, CV, Skills, Working With Me/Services — and stays the source of truth for that content; boesger.com only links to it, it does not re-host it. Other Bösger properties (`polarion.boesger.com`, `colors.boesger.com`, `photography.boesger.com`, product subdomains like `polarion-mcp.boesger.com`) are likewise separate deployments/repos and are **not** managed here — this site links out to all of them (Work section + Connect section + footer) so every page stays reachable from one place, rather than folding their content into this repo. Do not re-add a full About/CV/Services section here — that content lives on digital.boesger.com and was deliberately removed from this site to avoid two sources of truth.

`cloudflare/tools-proxy/` is a separate, optionally-deployed Cloudflare Worker that reverse-proxies select product subdomains under `boesger.com/tools/<slug>` (see its README) — it is not part of the static site build/deploy and requires a manual `wrangler deploy` or dashboard setup.

## Architecture

The page is a single `index.html` backed by one CSS file and two JS files:

- **`index.html`** — All content lives here: sticky nav, hero, work/portfolio, blog teaser, connect (link hub), contact CTA, and footer (incl. a discreet "Internal" row for private tools). The footer year is set by an inline `<script>` at the bottom.
- **`assets/css/style.css`** — All styling. Design tokens (colors, spacing, radii, shadows, motion, typography) are CSS custom properties in `:root`, ported from the Bösger Digital design system (canonical source: colors.boesger.com). Responsive breakpoints: `900px` (nav collapses to a mobile menu, hero/blog grids stack), `640px`/`560px` (spacing and CTA adjustments).
- **`assets/js/network.js`** — An IIFE that draws an animated particle-network on `<canvas id="network-canvas">`, rendered behind all content at `z-index: 0`.
- **`assets/js/site.js`** — An IIFE handling sticky-header background on scroll, nav scroll-spy (active section highlighting), the mobile menu toggle, and reveal-on-scroll (`[data-reveal]` + `IntersectionObserver`, respects `prefers-reduced-motion`).
- **`assets/js/snowfall.js`** — An alternative snowfall animation (currently **not loaded**). It targets `#bg-canvas` (which does not exist in the current HTML) and bundles a minified copy of Underscore.js. It can be enabled by swapping the canvas ID and script tag.

## CSS Custom Properties

All theme values are in `:root` in `style.css`. Key tokens:

| Variable | Purpose |
|---|---|
| `--bg-deep` / `--bg-surface` | Page background / raised panels |
| `--accent` / `--accent-glow` / `--accent-blue` | Primary electric-cyan accent family |
| `--border-subtle` / `--border-default` / `--border-strong` | Hairline border states |
| `--text-primary` / `--text-body` / `--text-muted` | Heading / body / caption text |
| `--surface-glass` | Glassmorphism card fill (used with `backdrop-filter: blur(...)`) |
| `--radius-sm` … `--radius-xl` / `--radius-pill` | Corner radii scale |
| `--shadow-sm` … `--shadow-xl` / `--glow-accent` | Elevation + cyan glow shadows |
| `--space-2` … `--space-8` | 4px-base spacing scale |
| `--container` | Max content width (`1024px`) |

Legacy aliases (`--bg`, `--cyan`, `--blue`, `--border`, `--text`, `--radius`, …) are kept mapped to the new tokens for backward compatibility — prefer the new names above in new code.

## HTML Conventions

**Link card pattern** (Connect section — copy-paste to add a link):
```html
<a href="https://example.com" target="_blank" rel="noopener" class="link-card">
  <img src="assets/images/icon.png" alt="" />
  <span>Label</span>
  <svg class="arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
    <path d="M7 17L17 7M17 7H7M17 7v10" />
  </svg>
</a>
```

For links to private/internal services, use `rel="nofollow noopener"` and place them in the footer's `.site-footer-internal` row (`.internal-link`) instead of the public Connect grid — see the n8n/Proxmox entries.

**Work/portfolio card pattern** (`.glass-card.work-card`): an `<a>` wrapping a `.work-card-media` (icon + status `.badge`) and `.work-card-body` (`h3`, tagline, description, `.tag` chips, "Learn more →" link). Only add cards for things that are actually live/real — this section has previously contained AI-fabricated placeholder products; verify against the real product site or GitHub repo before adding one.

**Section pattern**: `<section class="section" id="...">` (add `.section-tinted` for the alternating background band), with a `.section-head` (`.kicker` + `h2` + intro `p`) at the top.

## Data correctness

Marketing/bio copy on this site (years of experience, CV dates, stats, product statuses) must be traceable to a real source (this repo's git history, `github.com/phillipboesger`, LinkedIn, or the linked product repos) — do not invent numbers, testimonials, blog post titles, or CV entries, including when porting content from a Claude Design mockup. If a fact can't be verified, ask before publishing it.

## Images

All icons/photos live in `assets/images/` (flat, no subfolders). Recommended formats: PNG or WEBP with transparent background for icons, JPEG for photos. Keep images under ~300 KB. Link card icons are displayed at `34×34 px`; work-card media icons are shown at up to `60px` tall.

## Cache-busting

`boesger.com` is proxied through Cloudflare, which caches `assets/css/style.css` and the `assets/js/*.js` files for **4 hours** (`Cache-Control: max-age=14400`) — and browsers cache them too. The filenames never change, so a plain deploy can leave visitors with stale CSS/JS paired against fresh HTML (new markup styled by old rules — this has happened, e.g. new-but-unstyled elements). `index.html` references these files with a `?v=YYYYMMDDx` query string; **bump that version string on every commit that changes `style.css`, `site.js`, or `network.js`** so the new URL bypasses both caches immediately. Grep for `?v=` in `index.html` to find and update all three references together.
