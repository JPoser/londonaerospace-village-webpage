# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static website for the **London Aerospace** village at EMF Camp 2026 (Eastnor, 16–19 July 2026), served at `https://londonaero.space/`. One homepage plus a `/gallery/` page of photos and videos from previous camps (SPEC.md §11).

**`SPEC.md` is the source of truth** — read it before building or changing anything. The site is built and live; deploys happen on push to `main`.

## Commands

- `hugo server` — run locally with live reload
- `hugo --minify` — production build (output to `public/`); this is the Cloudflare Pages build command

Hugo **extended edition**, with the version pinned in config/CI (`HUGO_VERSION` env var on Cloudflare Pages).

## Hard constraints (from SPEC.md)

- **Zero JavaScript shipped.** No frameworks, no bundling, no analytics/tracking, no contact forms.
- **Custom minimal theme lives in `layouts/`** (~3 template files) — never add a third-party theme or theme submodule.
- **One hand-written CSS file** (`static/css/site.css`) — no preprocessors, no Tailwind. System font stack only, no webfonts.
- **Claude must not write final copy.** All prose is authored by the site owner; use clearly marked `TODO:` placeholders.
- **Homepage** weight < 100KB total including logo; Lighthouse 100/100/100 is the bar — treat less as a bug. The gallery page is exempt from the byte target (it's images) but everything else still applies: zero JS, no video embeds/self-hosting (YouTube links with static posters only), Lighthouse 100s.
- Keep the repo hosting-agnostic (Cloudflare Pages deploys it, but nothing Cloudflare-specific in the site).

## Architecture

Content and presentation are deliberately separated so the owner edits words without touching HTML:

- `content/_index.md` — all homepage copy (front matter + body sections)
- `data/links.yaml` — drives the "cool stuff" links section; adding a link = adding YAML, no HTML
- `content/gallery/index.md` — gallery page: video (YouTube) entries in front matter; photos are just files dropped into `content/gallery/photos/` (posters for videos in `content/gallery/posters/`). Hugo image processing makes the WebP thumbnails; originals aren't published.
- `layouts/index.html`, `layouts/gallery/single.html`, `layouts/_default/baseof.html`, `layouts/partials/head.html` — the entire theme
- `static/` — CSS, logo, favicon

Page sections top to bottom: hero (logo + strapline), about, workshop callout card (the one boxed/accented element on the page), find-us (links to the EMF map pin), links list, footer. Canonical external URLs (village page, map pin, workshop schedule page) are listed in SPEC.md §1 — the EMF schedule page is the source of truth for workshop timing; don't hardcode a time.

## Deployment

Push to `main` → Cloudflare Pages auto-builds and deploys (~1 min). PR preview deployments are enabled. `baseURL = "https://londonaero.space/"` in `hugo.toml`.
