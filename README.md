# boesger.com — Personal Branding Site

This repository contains my personal branding site: a single static HTML page covering who I am, what I do (Polarion consulting, AI enablement, full-stack builds), what I've built, and a "Connect" hub centralizing all my other links (projects, social profiles, automation tools, etc.). It replaces the previous simple Linktree-style page — it's intentionally lightweight, dependency‑free, and easy to customize.

> Purpose: One comprehensive, self‑hostable home for my brand — story + services + portfolio + every other link, instead of a bare list of buttons.

## ✨ Key Features

- 100% static: just `index.html`, one CSS file, two small JS files.
- No build step, no framework, no Node.js — hand-authored HTML/CSS/vanilla JS.
- Fast to load + privacy friendly (no analytics or trackers included by default).
- Sticky nav with scroll-spy, mobile menu, and reveal-on-scroll animation.
- Sections: Hero, About/CV, Services, Work & Portfolio, Blog teaser, Connect (all links), Contact.
- Ambient animated network/constellation background (canvas, no dependencies) or optional alternative snowfall effect.

## 📂 Structure

```
index.html             # Main page (all sections)
assets/css/style.css   # Styling + design tokens
assets/js/network.js   # Animated network/constellation background
assets/js/site.js      # Nav scroll-spy, mobile menu, reveal-on-scroll
assets/js/snowfall.js  # (Optional) alternative snowfall animation (currently not loaded)
assets/images/         # Profile photos, logos, product icons
assets/gif-readme/     # Demo GIF (legacy / template origin)
```

## 🔧 Customization Guide

### 1. Update Profile Photos

The hero uses `assets/images/about-image.webp` (in focus) and `assets/images/off-duty.webp` (off duty); the About section uses `assets/images/ai-layer.jpg`. Replace the files (keep the same filenames) or update the `src` attributes in `index.html`. Keep images optimized (< ~300 KB) for faster load times.

### 2. Add / Remove Links

**Connect section** (public link hub) — each entry follows the link-card pattern documented in `CLAUDE.md`. **Work & Portfolio** — each project is a `.glass-card.work-card` (see `CLAUDE.md` for the pattern). **Footer "Internal" row** — private/self-hosted tools use `rel="nofollow noopener"`.

Duplicate or remove blocks as needed. Keep `alt` text meaningful for accessibility. Only add real, verifiable content — see the "Data correctness" note in `CLAUDE.md`.

### 3. Section Headings

Sections use a `.kicker` + `<h2>` + intro `<p>` inside `.section-head`. Add new `<section class="section" id="...">` blocks and a matching nav pill in both the desktop and mobile nav to add a new page section.

### 4. Icons & Images

All images live in `assets/images/`. Reuse existing file names to avoid changing HTML, or update the `src` attributes. Recommended formats: PNG or WEBP with transparent background when suitable. Maintain consistent aspect ratios so cards feel balanced.

### 5. Styling

Adjust global colors, spacing and typography via the CSS custom properties in `:root` in `assets/css/style.css` (see `CLAUDE.md` for the token table). If you introduce additional utility classes, stay consistent with existing naming (`hero-*`, `work-card-*`, `link-card`, etc.).

## 🚀 Usage / Deployment

Because this is plain static content you can:

- Open `index.html` directly in a browser (double‑click locally).
- Host on any static platform (GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3, your own Nginx, etc.).

No build step, no dependencies, no Node.js required.

## 📄 License

Released under the MIT License. See `LICENSE` for details.
