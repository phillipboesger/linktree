# Personal Link Hub (LinkTree Style)

This repository contains my simple personal link hub: a single static HTML page that centralizes the most important links related to me (projects, social profiles, automation tools, etc.). It is intentionally lightweight, dependency‑free, and easy to customize.

> Purpose: Provide a clean, fast, self‑hostable alternative to commercial link aggregation services.

## ✨ Key Features

- 100% static: just `index.html`, one CSS file, optional JavaScript effect.
- Fast to load + privacy friendly (no analytics or trackers included by default).
- Easily change links, icons, headings, and profile image directly in HTML.
- Sub‑sections supported (see the "Automation" heading in the current layout).
- Ambient animated starfield background (CSS only) or optional alternative snowfall effect.

## 📂 Structure

```
index.html            # Main page
assets/css/style.css  # Styling
assets/js/snowfall.js # (Optional) alternative snowfall animation (currently commented out)
assets/images/        # Profile image, logos, icons
assets/gif-readme/    # Demo GIF (legacy / template origin)
```

## 🔧 Customization Guide

### 1. Update Profile Image & Title

In `index.html` locate:

```
<img class="display-image" ... src="assets/images/about-image.jpg" />
<h2 class="page-title">Phillip Bösger</h2>
```

Replace the image file (keep the same filename or adjust the `src`). Keep images optimized (< ~300 KB) for faster load times.

### 2. Add / Remove Links

Each link block follows this pattern:

```
<div class="page-item-wrap relative">
    <div class="page-item flex-both-center absolute"></div>
    <a target="_blank" class="page-item-each py-10 flex-both-center" href="https://example.com" data-type="page_item">
        <img class="link-each-image" src="assets/images/icon.png" alt="Label" />
        <span class="item-title text-center">Label</span>
    </a>
</div>
```

Duplicate or remove blocks as needed. Keep `alt` text meaningful for accessibility.

### 3. Section Headings

To visually group related links you can add:

```
<h3 class="section-heading">Automation</h3>
```

Add more headings wherever logical.

### 4. Icons & Images

All images live in `assets/images/`. Reuse existing file names to avoid changing HTML, or update the `src` attributes. Recommended formats: PNG or WEBP with transparent background when suitable. Maintain consistent aspect ratios so buttons feel balanced.

### 5. Styling

Adjust global colors, fonts, spacing in `assets/css/style.css`. If you introduce additional utility classes, stay consistent with existing naming (`page-*`, `flex-*`, etc.).

## 🚀 Usage / Deployment

Because this is plain static content you can:

- Open `index.html` directly in a browser (double‑click locally).
- Host on any static platform (GitHub Pages, Netlify, Vercel, Cloudflare Pages, S3, your own Nginx, etc.).

No build step, no dependencies, no Node.js required.

## 📄 License

Released under the MIT License. See `LICENSE` for details.
