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

## Publishing

Push to `main` → Cloudflare Pages builds (`hugo --minify`) and deploys —
live in about a minute. Pull requests get preview deployments automatically.
