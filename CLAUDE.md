# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-page static marketing website for **gäbig** — a Swiss warehouse management / fulfillment SaaS. No build step, no framework, no dependencies. Open `index.html` directly in a browser.

## Deployment

Push to `main` → DigitalOcean App Platform auto-deploys (spec in `.do/app.yaml`, Frankfurt region). To create the app for the first time: `doctl apps create --spec .do/app.yaml`.

## Architecture

Everything lives in `index.html` — all CSS (inline `<style>`), all HTML, and all JS (`<script>` at the bottom). Assets are in `assets/`:
- `assets/fonts/` — custom Helvetica variants (CondensedBold, Light, LightOblique) referenced in `@font-face`
- `assets/gaebig-icon.svg` / `assets/gaebig.svg` — logo mark and wordmark, inlined as `<svg>` in the nav and footer

## Design tokens

All colours are CSS custom properties on `:root`. The palette:
- `--b-*` — brand purple (900 darkest → 100 lightest)
- `--n-*` — warm neutrals / eggshell (900 → 000)
- `--egg-*` — eggshell tints (page backgrounds)
- `--sec-*` — olive secondary
- `--sem-*` — semantic (warning, success, error, info)
- `--p-a` through `--p-f` + `--p-*-100` tints — profile palette used for entity avatars and status pills

Never introduce colours outside these tokens.

## Bilingual DE/FR

Language is toggled by setting `lang="de"` or `lang="fr"` on `<html>` (persisted in `localStorage` as `gabig.lang`).

Pattern: place both language variants as siblings and tag them:
```html
<span data-de>German text</span>
<span data-fr>French text</span>
```

The CSS rule `[data-fr]{display:none}` hides FR by default; `html[lang="fr"] [data-de]{display:none}` / `html[lang="fr"] [data-fr]{display:revert}` switches them. **Every user-visible string must have both variants** — this includes mock UI elements inside the hero and screenshot carousel.

## Sections (top → bottom)

`#top` hero → trust strip → `#problem` → `#features` → `#how` → `#screenshots` → `#aichat` → `#swiss` → `#demo` → footer

The screenshot carousel (`#screenshots`) is driven by `goShot(i)` — three `.shot` divs, only one visible at a time, auto-advances every 6.5 s.

The demo form uses `handleSubmit(e)` which fakes a submission after 700 ms and replaces the form with a success state.

## Typography rules

- Headings: `font-weight:700`, `font-family:'Helvetica-Custom'`  
- Body: `font-weight:300`  
- Monospace data (IDs, timestamps, numbers): `ui-monospace, SFMono-Regular, Menlo, monospace`  
- Eyebrow labels: 11px, uppercase, `letter-spacing:.16em`, `color:var(--b-500)`
