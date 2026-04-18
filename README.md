# 🇺🇾 Montevideo City Tour — Road Trip Guide

[![Deploy to GitHub Pages](https://github.com/AxiomaAbsurdo/mvd-roadtrip-mamoca/actions/workflows/pages.yml/badge.svg)](https://github.com/AxiomaAbsurdo/mvd-roadtrip-mamoca/actions/workflows/pages.yml)
[![Live Site](https://img.shields.io/badge/live-axiomaabsurdo.github.io-c45d3e)](https://axiomaabsurdo.github.io/mvd-roadtrip-mamoca/)

A single-file, zero-dependency, mobile-first tour guide built with pure **HTML**, **CSS**, and minimal **JavaScript**. Designed as an interactive timeline that walks users through 12 stops across Montevideo, Uruguay — from colonial landmarks to specialty coffee roasters.

**🌐 Live demo:** <https://axiomaabsurdo.github.io/mvd-roadtrip-mamoca/>

---

## Table of Contents

1. [Overview](#overview)
2. [Live Features](#live-features)
3. [Architecture](#architecture)
4. [File Structure](#file-structure)
5. [Design System](#design-system)
6. [Internationalization (i18n)](#internationalization-i18n)
7. [Theming — Dark & Light Mode](#theming--dark--light-mode)
8. [Responsive Design & Mobile Strategy](#responsive-design--mobile-strategy)
9. [iPhone & iOS Optimizations](#iphone--ios-optimizations)
10. [Timeline Component](#timeline-component)
11. [Card Component](#card-component)
12. [Google Maps Integration](#google-maps-integration)
13. [Performance](#performance)
14. [Accessibility](#accessibility)
15. [CSS Architecture](#css-architecture)
16. [JavaScript Architecture](#javascript-architecture)
17. [Data Model](#data-model)
18. [Tour Stops Reference](#tour-stops-reference)
19. [Browser Support](#browser-support)
20. [Customization Guide](#customization-guide)
21. [Deployment — GitHub Pages](#deployment--github-pages)
22. [SEO & Social Sharing](#seo--social-sharing)
23. [Known Limitations](#known-limitations)
24. [License](#license)

---

## Overview

| Property          | Value                                     |
| ----------------- | ----------------------------------------- |
| **File**          | `index.html`                              |
| **Size**          | ~30 KB (single file)                      |
| **Hosting**       | GitHub Pages (auto-deploy via Actions)    |
| **Dependencies**  | None (Google Fonts loaded externally)      |
| **JS Framework**  | None — vanilla JS (~100 lines)            |
| **CSS Framework** | None — hand-written, variable-driven CSS  |
| **Languages**     | Português (default), English              |
| **Themes**        | Light (default), Dark                     |
| **Tour Stops**    | 12                                        |
| **Tour Duration** | 8:00 AM – 2:30 PM                        |

---

## Live Features

- **Vertical timeline** with animated dots, connecting line, and staggered card entrance animations.
- **12 location cards** each with a hero image, bilingual title/description, transport badge, address, and Google Maps deep link.
- **i18n toggle** (PT / EN) — instantly switches all UI copy including hero section, badges, buttons, and card content.
- **Dark / Light theme** — iOS-style pill toggle with `☀️` / `🌙` icons, smooth CSS transitions, `localStorage` persistence, and `prefers-color-scheme` auto-detection.
- **Sticky frosted-glass header** with `backdrop-filter: blur(20px)`.
- **Fully responsive** — mobile-first breakpoints at `600px` and `900px`.

---

## Architecture

The project follows a **single-file architecture** — all HTML, CSS, and JS live inside one `.html` file. This was an intentional constraint to maximize portability (share via AirDrop, email attachment, USB, etc.) and eliminate build steps.

```
┌──────────────────────────────────────────────┐
│                tour-guide.html               │
│                                              │
│  ┌─────────────────────────────────────────┐ │
│  │  <head>                                 │ │
│  │  ├── Meta tags (viewport, iOS, theme)   │ │
│  │  ├── Google Fonts preconnect + load     │ │
│  │  └── <style> — Full CSS (~450 lines)    │ │
│  └─────────────────────────────────────────┘ │
│                                              │
│  ┌─────────────────────────────────────────┐ │
│  │  <body>                                 │ │
│  │  ├── Header (logo, lang switch, toggle) │ │
│  │  ├── Hero section (badge, title, meta)  │ │
│  │  ├── <main> Timeline container (#id)    │ │
│  │  └── Footer                             │ │
│  └─────────────────────────────────────────┘ │
│                                              │
│  ┌─────────────────────────────────────────┐ │
│  │  <script> — JavaScript (~100 lines)     │ │
│  │  ├── stops[] — Tour data array          │ │
│  │  ├── i18n{} — Translation strings       │ │
│  │  ├── renderTimeline() — DOM builder     │ │
│  │  ├── setLang() / updateStaticI18n()     │ │
│  │  ├── toggleTheme()                      │ │
│  │  └── init() — IIFE bootstrap            │ │
│  └─────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

---

## File Structure

```
project/
├── index.html              # Complete application (single file)
├── 404.html                # Bilingual not-found page (GitHub Pages)
├── manifest.webmanifest    # PWA manifest (installable on iOS/Android)
├── robots.txt              # Crawler directives
├── sitemap.xml             # SEO sitemap
├── .nojekyll               # Bypass Jekyll processing on Pages
├── .github/
│   └── workflows/
│       └── pages.yml       # GitHub Actions — auto-deploy to Pages
└── README.md               # This documentation
```

No `node_modules`, no `package.json`, no build system. Open `index.html` in any browser, or push to `main` and GitHub Actions deploys to Pages automatically.

---

## Design System

### Typography

| Role      | Font              | Weights       | Usage                       |
| --------- | ----------------- | ------------- | --------------------------- |
| Display   | Playfair Display  | 400, 600, 700 | Hero title, card titles, header |
| Body      | DM Sans           | 300, 400, 500, 600 | Descriptions, labels, buttons |
| Fallback  | `-apple-system, BlinkMacSystemFont, sans-serif` | — | System stack |

Fonts are loaded from Google Fonts with `preconnect` hints for performance:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

### Color Palette

**Light Theme:**

| Token              | Hex         | Purpose                    |
| ------------------ | ----------- | -------------------------- |
| `--bg-primary`     | `#f8f6f3`   | Page background            |
| `--bg-card`        | `#ffffff`   | Card surface               |
| `--text-primary`   | `#1a1a1d`   | Headings, body text        |
| `--text-secondary` | `#5a5a5f`   | Descriptions               |
| `--text-tertiary`  | `#8a8a8f`   | Labels, metadata           |
| `--accent`         | `#c45d3e`   | Terracotta — buttons, dots |
| `--border`         | `#e8e5e0`   | Card borders               |

**Dark Theme:**

| Token              | Hex         | Purpose                    |
| ------------------ | ----------- | -------------------------- |
| `--bg-primary`     | `#0d0d0f`   | Page background            |
| `--bg-card`        | `#1c1c1f`   | Card surface               |
| `--text-primary`   | `#f0eeeb`   | Headings, body text        |
| `--accent`         | `#e07a5f`   | Warmer terracotta variant  |
| `--border`         | `#2a2a2d`   | Card borders               |

### Shadow System

Four elevation levels defined as CSS variables:

```
--shadow-sm    → subtle resting state
--shadow-md    → medium elevation
--shadow-lg    → high elevation
--shadow-card-hover → card hover state (strongest)
```

Dark theme uses higher opacity shadows to maintain depth perception on dark surfaces.

---

## Internationalization (i18n)

### How It Works

The i18n system uses two complementary strategies:

**1. Static elements** — HTML elements with `data-i18n` attributes are updated by `updateStaticI18n()`:
```html
<span data-i18n="heroBadge">Roteiro de viagem</span>
```

**2. Dynamic elements** — Timeline cards are fully re-rendered by `renderTimeline()` which reads `currentLang` and pulls the correct string from each stop's bilingual data.

### Translation Object

```javascript
const i18n = {
  pt: {
    heroBadge: "Roteiro de viagem",
    heroStops: "12 paradas",
    walkLabel: "A pé",
    driveLabel: "De carro",
    mapBtn: "Ver no mapa",
    // ... 11 keys total
  },
  en: {
    heroBadge: "Road trip guide",
    heroStops: "12 stops",
    walkLabel: "Walking",
    driveLabel: "Driving",
    mapBtn: "View on map",
    // ... 11 keys total
  }
};
```

### Stop-Level Translations

Each stop in the `stops[]` array carries its own bilingual content:

```javascript
{
  title: { pt: "Puerta de la Ciudadela", en: "Gateway of the Citadel" },
  desc:  { pt: "O último vestígio da muralha colonial...", en: "The last remnant..." }
}
```

### Language Switch UI

A segmented control in the header with `PT` | `EN` buttons. The active button receives the accent color background. Switching is instant — no page reload, no flash.

### Adding a New Language

1. Add a new key to the `i18n` object (e.g., `es: { ... }`).
2. Add `es` title/desc to every object in `stops[]`.
3. Add a new `<button class="lang-btn" data-lang="es" onclick="setLang('es')">ES</button>` in the header.

---

## Theming — Dark & Light Mode

### Toggle Component

The theme toggle replicates the iOS `UISwitch` pattern:

```
┌──────────────────────────┐
│  ╭──────╮                │   Light mode
│  │  ☀️  │                │   Knob left
│  ╰──────╯                │
└──────────────────────────┘

┌──────────────────────────┐
│                ╭──────╮  │   Dark mode
│                │  🌙  │  │   Knob right (translateX: 22px)
│                ╰──────╯  │
└──────────────────────────┘
```

### Implementation Details

- **CSS-driven** — The toggle uses `[data-theme="dark"]` attribute selector on `<html>`.
- **Transition** — Knob slides with `cubic-bezier(0.4, 0, 0.2, 1)` (Material ease-out).
- **Persistence** — Theme choice saved to `localStorage` under key `mvd-theme`.
- **System detection** — On first visit (no `localStorage`), the app checks `prefers-color-scheme: dark` via `window.matchMedia`.
- **Meta tag** — Two `<meta name="theme-color">` tags with `media` queries ensure the browser chrome matches the theme.

### What Changes Between Themes

Every visual property transitions through CSS custom properties:

- Background colors (page, card, header blur)
- Text colors (3 tiers)
- Accent color shifts warmer in dark mode (`#c45d3e` → `#e07a5f`)
- Shadows increase in opacity for dark surfaces
- Image overlays become more opaque in dark mode
- Hero gradient darkens proportionally
- Transport badges use darker tinted backgrounds
- Toggle track color

All transitions are 300–400ms for a smooth feel without sluggishness.

---

## Responsive Design & Mobile Strategy

### Approach: Mobile-First

Base styles target the smallest viewport (iPhone SE at 375px). Enhancements are layered via `min-width` media queries.

### Breakpoints

| Breakpoint    | Target            | Key Changes                                    |
| ------------- | ----------------- | ---------------------------------------------- |
| Base (< 600)  | iPhone / mobile   | Timeline line at `left: 19px`, card images 180px tall |
| `≥ 600px`     | Tablet / iPad     | Timeline shifts to `left: 39px`, images 220px, larger padding |
| `≥ 900px`     | Desktop           | Images 240px, increased card margins           |

### Content Width

The timeline container is capped at `max-width: 780px` with `margin: 0 auto` to maintain readability on wide screens while feeling native on mobile.

### Fluid Typography

The hero title uses `clamp()` for fluid sizing:
```css
font-size: clamp(2rem, 6vw, 2.8rem);
```

---

## iPhone & iOS Optimizations

The UI is specifically tuned for iPhone Safari with these techniques:

### Viewport & Safe Areas

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```

- `viewport-fit=cover` — extends content under the notch/Dynamic Island.
- `user-scalable=no` — prevents accidental zoom on double-tap.
- `apple-mobile-web-app-capable` — enables full-screen mode when saved to Home Screen.

### Safe Area Insets

```css
.site-header {
  padding-top: env(safe-area-inset-top);
}
.header-inner {
  padding-left: max(20px, env(safe-area-inset-left));
  padding-right: max(20px, env(safe-area-inset-right));
}
.site-footer {
  padding-bottom: max(40px, env(safe-area-inset-bottom));
}
```

These ensure content never hides behind the notch, rounded corners, or the home indicator bar.

### Touch Optimizations

```css
-webkit-tap-highlight-color: transparent;   /* No gray flash on tap */
```

All interactive elements (buttons, links, toggle) have this applied.

### Rendering

```css
-webkit-font-smoothing: antialiased;        /* Sharper text on Retina */
-moz-osx-font-smoothing: grayscale;
min-height: 100dvh;                         /* Dynamic viewport height (avoids Safari toolbar issues) */
```

### Backdrop Filter

The sticky header uses `-webkit-backdrop-filter` (prefixed for Safari) alongside the standard `backdrop-filter`:
```css
-webkit-backdrop-filter: saturate(180%) blur(20px);
backdrop-filter: saturate(180%) blur(20px);
```

### Theme Color Meta Tags

Two `<meta name="theme-color">` tags with `media` attributes ensure Safari's address bar matches the active theme automatically:
```html
<meta name="theme-color" content="#f8f6f3" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#0d0d0f" media="(prefers-color-scheme: dark)">
```

---

## Timeline Component

### Visual Structure

```
  │
  ●── 8:00 – 8:15
  │   ┌──────────────────────────┐
  │   │  [Image]        🚶 A pé │
  │   │  Card Title              │
  │   │  Description text...     │
  │   │  📍 Address   [Map btn]  │
  │   └──────────────────────────┘
  │
  ●── 8:15 – 8:30
  │   ┌──────────────────────────┐
  │   │  ...                     │
  │   └──────────────────────────┘
  │
```

### CSS Implementation

The vertical line is a `::before` pseudo-element on the `.timeline` container:
```css
.timeline::before {
  content: '';
  position: absolute;
  left: 19px;
  top: 32px;
  bottom: 0;
  width: 2px;
  background: var(--timeline-line);
}
```

Each `.timeline-dot` is absolutely positioned to align with the line:
```css
.timeline-dot {
  position: absolute;
  left: 12px;
  top: 24px;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--timeline-dot);
  border: 3px solid var(--bg-primary);
  box-shadow: 0 0 0 3px var(--timeline-dot-ring);
}
```

### Entrance Animation

Cards fade in and slide up with staggered delays:
```css
.timeline-item {
  opacity: 0;
  transform: translateY(20px);
  animation: cardFadeIn 0.5s ease forwards;
}

.timeline-item:nth-child(1) { animation-delay: 0.05s; }
.timeline-item:nth-child(2) { animation-delay: 0.1s; }
/* ... up to 12 */
```

### Hover Effects

- **Dot**: Scales to 1.3× with an expanded glow ring.
- **Card**: Lifts 2px (`translateY(-2px)`) with an intensified shadow.
- **Image**: Subtle zoom to 1.06× scale.

---

## Card Component

### Anatomy

```
┌──────────────────────────────────┐
│                                  │
│   [Hero Image — 180/220/240px]   │
│   gradient overlay               │
│                  🚶 A pé  badge  │
│                                  │
├──────────────────────────────────┤
│  Card Title     (Playfair)       │
│  Description... (DM Sans 300)    │
│                                  │
│  📍 Address        [🗺 Map btn]  │
└──────────────────────────────────┘
```

### Image Handling

- Images loaded from Unsplash with `w=800&h=500&fit=crop&q=80` parameters for optimized delivery.
- `loading="lazy"` — native lazy loading, critical for 12 images.
- `decoding="async"` — prevents image decoding from blocking the main thread.
- Gradient overlay ensures text readability over any image content.
- Hover zoom uses `transform: scale(1.06)` with `overflow: hidden` on the wrapper.

### Transport Badges

Two variants with backdrop blur:

| Type  | Class         | Icon | Background                  |
| ----- | ------------- | ---- | --------------------------- |
| Walk  | `.badge-walk` | 🚶   | `rgba(46,125,50,0.85)` green  |
| Drive | `.badge-drive`| 🚗   | `rgba(21,101,192,0.85)` blue  |

---

## Google Maps Integration

### Deep Linking Strategy

Each card includes a "View on map" button that opens Google Maps via the **Maps URLs API**:

```
https://www.google.com/maps/search/?api=1&query=ENCODED_QUERY
```

### Query Format

Queries are URL-encoded place names with city and country for disambiguation:
```
Puerta+de+la+Ciudadela,+Montevideo,+Uruguay
Mercado+del+Puerto,+Montevideo,+Uruguay
Templo+Tostadores,+Charrua+2825,+Montevideo,+Uruguay
```

### Behavior by Platform

| Platform       | Behavior                                       |
| -------------- | ---------------------------------------------- |
| iOS Safari     | Opens Google Maps app (if installed) or web     |
| iOS (PWA)      | Opens Google Maps app                           |
| Android Chrome | Opens Google Maps app                           |
| Desktop        | Opens Google Maps in a new tab                  |

### Link Attributes

```html
<a href="..." target="_blank" rel="noopener noreferrer" class="map-link">
```

- `target="_blank"` — new tab/app.
- `rel="noopener noreferrer"` — security best practice for external links.
- `aria-label` — translated label for screen readers.

---

## Performance

### Zero-Build Optimizations

| Technique                | Implementation                                    |
| ------------------------ | ------------------------------------------------- |
| Single HTTP request      | All HTML/CSS/JS in one file                        |
| Font preconnect          | `<link rel="preconnect">` for Google Fonts          |
| Lazy image loading       | `loading="lazy"` on all 12 card images               |
| Async image decoding     | `decoding="async"` on all images                     |
| CSS-only animations      | `@keyframes` + `animation-delay`, no JS animation library |
| Minimal JS               | ~100 lines, no frameworks, no DOM diffing            |
| CSS custom properties    | Theme switching without class toggling on every element |
| Efficient re-render      | `innerHTML` template literal — single DOM write per language switch |
| Reduced motion           | `prefers-reduced-motion` media query disables all animations |

### Image Optimization

Images are served from Unsplash's CDN with query parameters:
- `w=800` — max width constrained to reduce payload
- `h=500` — height hint for aspect ratio
- `fit=crop` — ensures consistent card dimensions
- `q=80` — quality balance between size and clarity

---

## Accessibility

| Feature                     | Implementation                                    |
| --------------------------- | ------------------------------------------------- |
| Semantic HTML               | `<header>`, `<main>`, `<article>`, `<footer>`, `<nav>` structure |
| Language attribute          | `<html lang="pt">` updated dynamically on language switch |
| Alt text                    | Every image has a descriptive `alt` attribute       |
| ARIA labels                 | Theme toggle and map links have `aria-label`        |
| Color contrast              | Text colors meet WCAG AA against their backgrounds  |
| Reduced motion              | All animations and transitions disabled via `prefers-reduced-motion` |
| Focus indicators            | Browser default focus rings preserved (not suppressed) |
| Semantic headings           | `<h1>` in hero, `<h2>` for each card title          |

---

## CSS Architecture

### Organization (top to bottom)

```
1.  Custom Properties — Light Theme (:root)
2.  Custom Properties — Dark Theme ([data-theme="dark"])
3.  Reset & Base Styles
4.  Sticky Header
5.  Language Switch
6.  Theme Toggle (iOS style)
7.  Hero Section
8.  Timeline Container
9.  Timeline Card
10. Footer
11. Responsive — Tablet (≥ 600px)
12. Responsive — Desktop (≥ 900px)
13. Reduced Motion
14. Safe Area Insets
```

### Naming Convention

BEM-influenced flat classes — no deep nesting:
```
.card
.card-img-wrap
.card-img
.card-img-overlay
.card-body
.card-title
.card-desc
.card-footer
.card-location
.card-transport-badge
```

### Custom Properties Count

- **Light theme**: 26 variables
- **Dark theme**: 26 variables (same names, different values)

---

## JavaScript Architecture

### Functions (4 total + 1 IIFE)

| Function            | Lines | Purpose                                              |
| ------------------- | ----- | ---------------------------------------------------- |
| `renderTimeline()`  | ~40   | Builds all 12 cards via template literal + `innerHTML` |
| `updateStaticI18n()`| ~5    | Updates `data-i18n` elements in the static DOM        |
| `setLang(lang)`     | ~6    | Switches language: updates buttons, static text, re-renders timeline |
| `toggleTheme()`     | ~4    | Flips `data-theme` attribute, persists to `localStorage` |
| `init()` (IIFE)     | ~15   | Restores saved theme or detects system preference, renders initial timeline |

### Data Structures (2)

| Structure   | Type     | Purpose                                 |
| ----------- | -------- | --------------------------------------- |
| `stops[]`   | Array    | 12 stop objects with bilingual content   |
| `i18n{}`    | Object   | Translation strings keyed by language    |

### No External Dependencies

- No jQuery, no React, no Alpine.js.
- No build step, no transpilation, no polyfills.
- Template literals handle HTML generation.
- `querySelectorAll` + `forEach` for DOM queries.

---

## Data Model

### Stop Object Schema

```javascript
{
  time: string,          // Display time range (e.g., "8:00 – 8:15")
  title: {
    pt: string,          // Portuguese title
    en: string           // English title
  },
  desc: {
    pt: string,          // Portuguese description
    en: string           // English description
  },
  transport: "walk" | "drive",   // Transport mode to reach this stop
  address: string,               // Physical address
  mapQuery: string,              // URL-encoded Google Maps search query
  img: string,                   // Unsplash image URL
  imgAlt: string                 // Image alt text
}
```

---

## Tour Stops Reference

| # | Time          | Stop                    | Transport | Neighborhood    |
|---|---------------|-------------------------|-----------|-----------------|
| 1 | 8:00 – 8:15   | Puerta de la Ciudadela  | 🚶 Walk   | Ciudad Vieja    |
| 2 | 8:15 – 8:30   | Calle Sarandí           | 🚶 Walk   | Ciudad Vieja    |
| 3 | 8:30 – 8:45   | Plaza Matriz            | 🚶 Walk   | Ciudad Vieja    |
| 4 | 8:45 – 9:00   | Plaza Zabala            | 🚶 Walk   | Ciudad Vieja    |
| 5 | 9:00 – 9:45   | Café La Farmacia        | 🚶 Walk   | Ciudad Vieja    |
| 6 | 9:45 – 10:00  | Plaza Fabini            | 🚗 Drive  | Centro          |
| 7 | 10:00 – 10:45 | Templo Tostadores       | 🚗 Drive  | La Blanqueada   |
| 8 | 10:45 – 11:00 | Letras de Montevideo    | 🚗 Drive  | Pocitos         |
| 9 | 11:00 – 11:15 | Estadio Centenario      | 🚗 Drive  | Parque Batlle   |
| 10| 12:00         | Mercado del Puerto      | 🚗 Drive  | Ciudad Vieja    |
| 11| 13:00         | Palacio Legislativo     | 🚗 Drive  | Aguada          |
| 12| 13:30         | Jardín Botánico         | 🚗 Drive  | Prado           |

---

## Browser Support

| Browser              | Version | Status      | Notes                          |
| -------------------- | ------- | ----------- | ------------------------------ |
| Safari (iOS)         | 15.4+   | ✅ Full     | Primary target, fully tested   |
| Chrome (Android)     | 100+    | ✅ Full     |                                |
| Chrome (Desktop)     | 100+    | ✅ Full     |                                |
| Firefox              | 100+    | ✅ Full     |                                |
| Edge                 | 100+    | ✅ Full     |                                |
| Safari (macOS)       | 15.4+   | ✅ Full     |                                |

### CSS Features Used & Support

| Feature               | Spec Status | Fallback                   |
| --------------------- | ----------- | -------------------------- |
| `dvh` units           | Baseline    | Falls back to `vh`         |
| `env(safe-area-*)`    | Stable      | Falls back to `0`          |
| CSS Custom Properties | Stable      | N/A (required)             |
| `backdrop-filter`     | Stable      | Prefixed `-webkit-` included |
| `clamp()`             | Stable      | N/A                        |
| `inset` shorthand     | Stable      | N/A                        |
| `loading="lazy"`      | Stable      | Eager load (graceful)      |
| `decoding="async"`    | Stable      | Sync decode (graceful)     |

---

## Customization Guide

### Change the Accent Color

Update both theme blocks in CSS:

```css
/* Light */
:root {
  --accent: #YOUR_COLOR;
  --accent-soft: #YOUR_COLOR1a;
  --accent-glow: #YOUR_COLOR33;
  --timeline-dot: #YOUR_COLOR;
  --map-btn-bg: #YOUR_COLOR;
}

/* Dark — use a slightly lighter/warmer variant */
[data-theme="dark"] {
  --accent: #YOUR_LIGHTER_VARIANT;
  /* ... same tokens */
}
```

### Add a New Stop

Append an object to the `stops[]` array:

```javascript
{
  time: "14:00 – 14:30",
  title: { pt: "Nome em Português", en: "English Name" },
  desc: {
    pt: "Descrição...",
    en: "Description..."
  },
  transport: "drive",   // or "walk"
  address: "Rua Example 123, Montevideo",
  mapQuery: "Place+Name,+Montevideo,+Uruguay",
  img: "https://images.unsplash.com/photo-XXXXX?w=800&h=500&fit=crop&q=80",
  imgAlt: "Alt text"
}
```

Update `heroStops` in `i18n` to reflect the new count.

### Replace Images

Swap the `img` URL in any stop object. Recommended specs:
- **Width**: 800px minimum
- **Aspect ratio**: ~16:10
- **Format**: JPEG or WebP
- **Quality**: 75–85

### Change Fonts

Replace the Google Fonts `<link>` and update the CSS:

```css
body {
  font-family: 'Your Body Font', sans-serif;
}
.card-title, .header-title, .hero h1 {
  font-family: 'Your Display Font', serif;
}
```

---

## Deployment — GitHub Pages

This project is configured for **automatic deployment to GitHub Pages** on every push to `main` via GitHub Actions.

### Live URL

<https://axiomaabsurdo.github.io/mvd-roadtrip-mamoca/>

### Deploy Pipeline

The workflow at `.github/workflows/pages.yml` uses the official `actions/deploy-pages@v4` action and publishes the repository root as-is. No build step is required — the single-file architecture means GitHub Pages serves `index.html` directly.

```text
┌─────────────┐      ┌──────────────────┐      ┌─────────────────┐
│  git push   │ ───► │ GitHub Actions   │ ───► │  Pages CDN      │
│  (main)     │      │ (pages.yml)      │      │  (HTTPS, edge)  │
└─────────────┘      └──────────────────┘      └─────────────────┘
```

### One-time Setup (first deploy)

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment → Source**, select **GitHub Actions**.
4. Push a commit to `main` — the workflow runs automatically.
5. The deployed URL appears in the Actions run summary and under **Settings → Pages**.

### Alternative: Branch Deploy (no Actions)

If you prefer not to use Actions:

1. **Settings → Pages → Source** → **Deploy from a branch**.
2. Select `main` branch, `/` (root) folder, click **Save**.
3. GitHub serves the repo root at `https://<user>.github.io/<repo>/` within ~1 minute.

### Custom Domain

To use a custom domain (e.g. `mvdtour.example.com`):

1. Add a file named `CNAME` (no extension) at the repo root containing just your domain (e.g. `mvdtour.example.com` on a single line, no trailing newline).
2. Configure DNS at your registrar — add a `CNAME` record pointing to `axiomaabsurdo.github.io`.
3. In **Settings → Pages**, enter the domain under **Custom domain** and enable **Enforce HTTPS**.
4. Update `<link rel="canonical">`, `og:url`, and `sitemap.xml` to the new domain.

### Files That Support Pages

| File                    | Purpose                                                            |
| ----------------------- | ------------------------------------------------------------------ |
| `index.html`            | Root document — Pages serves this at `/`                           |
| `404.html`              | Custom not-found page (bilingual), served on any unknown path      |
| `.nojekyll`             | Empty sentinel file that disables Jekyll processing (faster deploy, allows `_` prefixed paths) |
| `manifest.webmanifest`  | PWA install metadata — "Add to Home Screen" on iOS / Android       |
| `robots.txt`            | Allows all crawlers, points to the sitemap                         |
| `sitemap.xml`           | Single-URL sitemap with hreflang hints for PT / EN                 |

### Local Preview

Since there's no build step, any static server works:

```bash
# Python
python3 -m http.server 8080

# Node (npx, no install)
npx serve .

# PHP
php -S localhost:8080
```

Then open <http://localhost:8080>.

---

## SEO & Social Sharing

### Meta Tags

`index.html` includes a complete SEO and social preview payload:

- **`<title>` and `<meta name="description">`** — crawler-facing summary.
- **`<link rel="canonical">`** — tells search engines the authoritative URL (prevents duplicate-content penalties if the site is mirrored).
- **Open Graph** (`og:type`, `og:title`, `og:description`, `og:image`, `og:url`, `og:locale`, `og:locale:alternate`) — rich previews on Facebook, LinkedIn, WhatsApp, Discord, Slack.
- **Twitter Card** (`twitter:card=summary_large_image`) — large-image preview on X / Twitter.
- **JSON-LD structured data** (`TouristTrip` schema) — machine-readable semantics for Google rich results.

### Social Preview Image

The Open Graph image uses a Montevideo cityscape from Unsplash sized at 1200×630 (the recommended OG aspect ratio). Swap the URL in the `og:image` / `twitter:image` tags to customize.

### Verifying the Setup

After deploy, test with:

- **Facebook / LinkedIn**: <https://www.opengraph.xyz/>
- **Twitter Card Validator**: <https://cards-dev.twitter.com/validator>
- **Google Rich Results**: <https://search.google.com/test/rich-results>
- **Lighthouse** (Chrome DevTools → Lighthouse) — targets 100/100 on Performance, Accessibility, Best Practices, SEO.

### PWA Install

With `manifest.webmanifest` linked, users can install the guide:

- **iOS Safari**: Share → Add to Home Screen (uses `apple-mobile-web-app-capable` + `apple-touch-icon`).
- **Android Chrome**: browser shows an "Install app" prompt automatically.
- Installed apps launch in standalone mode with the terracotta theme color in the status bar.

---

## Known Limitations

1. **Images depend on Unsplash CDN** — Requires internet connectivity. For offline use, replace with local images and update paths.
2. **No service worker** — Not a PWA. Could be added for offline caching.
3. **No route mapping** — Google Maps links open individual locations, not a connected route.
4. **localStorage for theme** — Blocked in some privacy-focused browsers; falls back gracefully to system preference.
5. **Single-file constraint** — CSS and JS are not separately cacheable. Acceptable for the project's size and portability goals.
6. **No GPS / geolocation** — The app doesn't track user location or provide navigation.

---

## License

This project is provided as-is for personal and educational use. Image assets are served from Unsplash and are subject to the [Unsplash License](https://unsplash.com/license). Google Maps links use the publicly available Maps URLs API.
