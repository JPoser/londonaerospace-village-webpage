# londonaero.space

Homepage for the London Aerospace village at EMF Camp 2026.

## Run it locally

Install [Hugo](https://gohugo.io/) (extended edition, v0.163.3 or later), then:

```
hugo server
```

Open http://localhost:1313 — the page reloads as you edit.

## Edit the words

All copy lives in **`content/_index.md`**: the strapline, workshop facts and
find-us blurb are in the front matter at the top; the "About the village"
paragraphs are the Markdown body below it. Open the file, type words, save.

## Add a link

Add an entry to **`data/links.yaml`**:

```yaml
- title: "Cool thing"
  url: "https://example.com"
  note: "Why it's cool"   # optional
```

## Add photos & videos (gallery)

The gallery page at `/gallery/` shows photos and videos from past camps.

- **Photos:** drop image files into `content/gallery/photos/`. Thumbnails and
  web-sized WebP copies are generated at build time; the originals never
  ship. The filename becomes the alt text, so name files descriptively, e.g.
  `drone-repair-bench-2024.jpg`.
- **Videos:** upload to YouTube, then add an entry to the `videos:` list in
  `content/gallery/index.md`. For a poster thumbnail, drop a still into
  `content/gallery/posters/` and point the entry's `poster:` at it —
  optional; without one the video renders as a plain link.

## Publishing

Push to `main` → Cloudflare Pages builds (`hugo --minify`) and deploys —
live in about a minute. Pull requests get preview deployments automatically.
