# londonaero.space — Village Homepage Spec

Spec for a small static homepage for the **London Aerospace** village at EMF Camp 2026. To be built by a coding agent; content will be written and maintained by Joe.

---

## 1. Purpose & context

- **What:** A single-page (plus optional stubs) website for the London Aerospace village at Electromagnetic Field 2026, Eastnor Castle, 16–19 July 2026.
- **Who it's for:** EMF attendees deciding whether to visit the village or come to our workshop, plus anyone who scans a QR code / hears about us on site.
- **Village blurb (official):** "The ever increasingly misnamed and geographically disparate London Aerospace village. Swing by if you'd like to learn more about drone building, drone flying, drone crashing, and drone repairing."
- **Domain:** `londonaero.space` (already owned; DNS managed by site owner).
- **Canonical external links:**
  - Village page: https://www.emfcamp.org/villages/2026/44-london-aerospace
  - Village map pin: https://map.emfcamp.org/#18.5/52.04047510342522/-2.376049358148265/m=52.04047510342522,-2.376049358148265
  - Workshop: https://www.emfcamp.org/schedule/2026/263-tiny-drones-in-the-big-top

## 2. Non-goals

- No CMS, no JavaScript frameworks, no build-time JS bundling.
- No analytics, cookies, or tracking of any kind.
- No blog (structure shouldn't preclude adding one later, but don't build it now).
- No contact forms — links out only.

## 3. Tech stack

| Concern | Choice |
| --- | --- |
| Static site generator | **Hugo** (extended edition, pinned version in config/CI) |
| Templating | Custom minimal theme **in the repo** (`layouts/`), not a third-party theme |
| Styling | One hand-written CSS file, no preprocessors, no Tailwind |
| JavaScript | None (zero JS is a feature; progressive enhancement only if ever needed) |
| Hosting | **Cloudflare Pages** (primary recommendation) — see §8 |
| Repo | GitHub, `main` branch auto-deploys |

Rationale for a custom mini-theme over an off-the-shelf one: the site is one page; a theme dependency is more surface area than the site itself. Total template code should be ~3 files.

## 4. Content model

All copy is authored by the site owner in Markdown / data files. **The coding agent must not write final copy** — use clearly marked `TODO:` placeholder text and lorem-free stub sentences so it's obvious what needs replacing.

```
content/
  _index.md          # homepage front matter + main body copy
data/
  links.yaml         # "cool stuff" links list
static/
  logo.png           # provided logo (black silhouette drone, transparent/white bg)
  favicon.ico / .svg # derived from logo
```

### `content/_index.md` front matter

```yaml
title: "London Aerospace"
description: "The London Aerospace village at EMF Camp 2026"
```

Body sections are authored as Markdown under headings the template renders in order (or use front-matter params per section — agent's choice, but keep it simple enough that editing is just "open one file, type words").

### `data/links.yaml`

```yaml
- title: "Example link"
  url: "https://example.com"
  note: "One-line description of why it's cool"   # optional
```

The links section renders this list. Adding a link = adding three lines of YAML, no HTML.

## 5. Page structure (single page)

Top to bottom:

1. **Header / hero**
   - Village logo (provided PNG) + wordmark "London Aerospace".
   - One-line strapline (TODO copy — placeholder: the official village blurb above works as a stand-in).
   - Eyebrow line: "EMF Camp 2026 · Eastnor · 16–19 July".
2. **About the village**
   - 1–3 paragraphs, Markdown-authored (TODO copy).
3. **Tiny Drones in the Big Top** (workshop callout)
   - Rendered as a visually distinct card/panel.
   - Facts to include (author will refine copy):
     - Workshop on **Stage A**, one evening only, **free**, all skill levels, no equipment needed.
     - FPV goggles for ridealongs, radios and simulators to try flying.
     - Bring-your-own rules: nothing suitable unless you'd fly it into your own face; **do not power up gear on site until briefed and allocated a frequency**.
     - Content note: cameras flying around.
   - Link to the EMF schedule page for date/time (don't hardcode a time unless the author supplies one — the schedule page is the source of truth).
4. **Find us on site**
   - Short paragraph (TODO copy) + prominent link/button to the EMF map pin above.
   - Optional: small static map thumbnail is *not* required; a link is fine.
5. **Cool stuff / links**
   - List rendered from `data/links.yaml` (title → link, optional note).
6. **Footer**
   - Link to the official EMF village page.
   - "Content provided by village attendees, not EMF" style disclaimer (mirrors EMF's own wording).
   - No copyright drama needed; a simple "London Aerospace, 2026" is fine.

## 6. Design

Brief: **super simple, fast, legible.** The design should feel like it belongs to a drone-flying hacker-camp village, not a SaaS landing page.

- **Palette:** monochrome-first, driven by the logo (solid black silhouette on white). Base: near-black text `#111` on white `#fff`. One accent colour for links/buttons and the workshop card — suggest a high-vis "prop guard orange" (e.g. `#ff5a1f`) or safety-tape yellow; agent to pick one and use it consistently. Dark mode via `prefers-color-scheme` is a nice-to-have, not required.
- **Typography:** system font stack only (`system-ui, -apple-system, Segoe UI, Roboto, sans-serif`) — no webfonts, keeps the page at ~zero payload. Generous line-height (≥1.5), max text measure ~65ch.
- **Layout:** single centred column, max-width ~42–48rem, comfortable padding on mobile. No grid gymnastics. Sections separated by whitespace and simple rules, not boxes everywhere — the workshop callout is the one boxed/accented element on the page (spend the boldness in one place).
- **Logo usage:** logo at a sensible size in the hero (it's a chunky silhouette; ~120–180px works). Also derive favicon (SVG or 32px ICO) and a simple Open Graph image (logo on white, 1200×630).
- **Responsive:** fluid down to 320px wide; no horizontal scroll; tap targets ≥44px. Test at 375px and 1440px.
- **Motion:** none required. If any is added, respect `prefers-reduced-motion`.

## 7. Quality bar

- Valid HTML5, one `<h1>`, semantic sectioning (`<main>`, `<section>`, `<footer>`).
- Lighthouse: 100 performance / 100 accessibility / 100 best practices should be trivially achievable — treat anything less as a bug.
- Total page weight target: **< 100KB** including logo (optimise/compress the PNG, or trace it to SVG if quality allows — the logo is a flat silhouette so SVG tracing should be near-lossless and tiny).
- Alt text on the logo ("London Aerospace tiny-whoop drone logo" or similar).
- Visible keyboard focus styles.
- Meta: title, description, canonical URL (`https://londonaero.space/`), Open Graph + Twitter card tags.

## 8. Hosting & deployment

**Recommended: Cloudflare Pages.**

- Free tier, connects to the GitHub repo, builds Hugo natively (set `HUGO_VERSION` env var to pin).
- Custom domain: add `londonaero.space` in Pages → Cloudflare handles the cert automatically. If DNS is already on Cloudflare this is one click; if not, either move DNS there or CNAME the apex via their instructions.
- Build config: build command `hugo --minify`, output directory `public`.
- Preview deployments on PRs come free — handy for reviewing copy changes.

**Acceptable alternative: GitHub Pages** with a GitHub Actions workflow (`peaceiris/actions-hugo` + `actions/deploy-pages`), CNAME file for the custom domain. Slightly more YAML, same result. Agent should implement Cloudflare Pages unless told otherwise, but keep the repo hosting-agnostic (nothing Cloudflare-specific in the site itself).

`baseURL = "https://londonaero.space/"` in `hugo.toml`.

## 9. Repo layout & authoring workflow

```
.
├── hugo.toml
├── content/
│   └── _index.md
├── data/
│   └── links.yaml
├── layouts/
│   ├── index.html          # the page
│   ├── _default/baseof.html
│   └── partials/head.html  # meta tags
├── static/
│   ├── css/site.css
│   ├── logo.svg (or .png)
│   └── favicon.svg
└── README.md
```

`README.md` must include, in plain language:
- How to run locally (`hugo server`).
- How to edit copy (`content/_index.md`).
- How to add a link (`data/links.yaml`).
- How deploys happen (push to `main` → live in ~1 min).

## 10. Acceptance checklist

- [ ] `hugo server` runs locally with pinned Hugo version; no theme submodules.
- [ ] All copy is placeholder text clearly marked `TODO`, editable in `content/_index.md` only.
- [ ] Links section is fully driven by `data/links.yaml`.
- [ ] Workshop card links to the EMF schedule page; village section links to the EMF map pin.
- [ ] Renders correctly at 320px, 375px, and 1440px with no horizontal scroll.
- [ ] Zero JavaScript shipped.
- [ ] Page weight < 100KB; Lighthouse 100/100/100.
- [ ] Deployed to Cloudflare Pages, serving on https://londonaero.space with HTTPS.
- [ ] README explains the edit → publish loop in under a screen of text.

## 11. Amendment: gallery page (July 2026)

Agreed deviation from §5's "single page": a second page at `/gallery/` — a
single flat gallery of photos and videos from previous London Aerospace
villages at EMF.

- **Photos:** originals live in `content/gallery/photos/`; Hugo image
  processing generates WebP thumbnails (grid, lazy-loaded) linking to
  web-sized WebP copies. Originals are not published
  (`build.publishResources: false`).
- **Videos:** never embedded and never self-hosted — hosted on YouTube and
  linked out via a static poster image (`content/gallery/posters/`)
  with a CSS-only play badge. No iframes, no autoplay, still zero JS.
- **Homepage impact:** one small "Previous camps" section linking to
  `/gallery/`. The <100KB page-weight bar in §7 continues to apply to the
  homepage; the gallery page is exempt from the byte target but keeps every
  other bar (zero JS, Lighthouse 100s, semantic HTML).
- Config in `content/gallery/index.md` front matter; template in
  `layouts/gallery/single.html`.
