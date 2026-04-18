# Agent Context — The Freshwater Gazette

This file gives AI agents the context they need to work on this project without re-deriving everything from scratch.

## What This Is

A news publication website for **The Freshwater Gazette**, an independent local news outlet. Built with Jekyll, hosted on GitHub Pages at `freshwatergazette.com`.

Migrated from a personal portfolio site (Sam Jacobs / s-jac.github.io) in April 2026. The Jekyll directory structure and build infrastructure were preserved; the design and content were completely replaced.

## Owner

- GitHub user: `s-jac`
- Contact email: `freshwatergazette@gmail.com`
- Custom domain: `freshwatergazette.com`

## Architecture

Static Jekyll site. No dynamic backend. All content is Markdown files in `_posts/`. GitHub Pages builds and deploys on every push to `main`.

Key files:
- `_config.yml` — site title, URL, permalink structure
- `_layouts/default.html` — full page shell (header + nav + footer)
- `_layouts/post.html` — individual article page
- `_includes/head.html` — `<head>` with font loading, meta tags, structured data
- `_includes/footer.html` — footer + GoatCounter (currently commented out pending setup)
- `_includes/comments.html` — Giscus (currently needs repo config — see below)
- `assets/css/styles.css` — all global styles for the Gazette design

## Design System

- Background: `#fff`, text: `#000`
- Borders: `1px solid #000` — only used to separate header/footer from content. No borders between articles.
- Font stack (CSS variables in `styles.css`):
  - `--font-display`: `'ITC Cheltenham', 'Cheltenham BT', 'Playfair Display', Georgia, serif`
  - `--font-body`: `'Source Serif 4', Georgia, serif`
  - `--font-ui`: `'Inter', system-ui, sans-serif`
- Page max width: 1200px (`.page-container`)
- Article page max width: 720px (`.article-wrap`)

## Homepage Layout (`index.html`)

Renders `site.posts` in this order:
1. **Featured** — `site.posts.first`, 50% page width centered, large headline + excerpt
2. **Secondary** — next 3 posts in a `repeat(3, 1fr)` grid
3. **Further** — all remaining posts in a `repeat(3, 1fr)` grid

Each card: image (or grey `aspect-ratio: 16/9` placeholder if `image` is empty), headline, date.

## Article Page (`_layouts/post.html`)

1. Headline (`.article-headline`)
2. Date byline (`.article-byline`)
3. Hero image (skipped if `image` front matter is empty/absent)
4. Article body (`.article-body`) — rendered Markdown
5. Giscus comments section (`.article-comments`)
6. "Read More" horizontal scroll carousel — up to 6 other posts

## Navigation

Three links in `.site-nav` (in `_layouts/default.html`):
- **News** → `/`
- **Subscribe** → `/subscribe/`
- **Contact** → `/contact/`

The archive at `/blog/` exists but is not in the main nav.

## Adding Articles

Create `_posts/YYYY-MM-DD-slug.md`:

```yaml
---
layout: post
title: "Headline here"
date: 2026-04-20
description: "One-sentence summary — shown on article cards and in SEO meta."
image: /assets/images/blog/filename.jpg   # optional; omit for grey placeholder
comments: true
---

Article body in Markdown...
```

## Services Still Needing Setup

1. **ITC Cheltenham font** — uncomment the Adobe Fonts `<link>` in `_includes/head.html`, replace `XXXXXXX` with the owner's Typekit kit ID (requires Adobe Creative Cloud subscription; create web project at fonts.adobe.com)
2. **GoatCounter** — uncomment the `<script>` in `_includes/footer.html`, replace URL with the owner's GoatCounter account URL
3. **Giscus** — update `data-repo`, `data-repo-id`, `data-category-id` in `_includes/comments.html` (go to giscus.app to generate values; requires GitHub Discussions enabled on the repo)
4. **Mailchimp** — replace the placeholder `<div class="mailchimp-placeholder">` in `subscribe.html` with Mailchimp embedded form HTML
5. **Banner image** — add a banner image to `assets/images/`, add `banner_image: /assets/images/banner.png` to `_config.yml`; the `_layouts/default.html` header will automatically switch from text wordmark to image when `site.banner_image` is set
6. **Article images** — add to `assets/images/blog/`, set `image` field in post front matter

## DNS / Custom Domain

The `CNAME` file at the repo root contains `freshwatergazette.com`.

DNS records needed at the domain registrar:

**A records** (for `@`):
- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

**AAAA records** (for `@`):
- `2606:50c0:8000::153`
- `2606:50c0:8001::153`
- `2606:50c0:8002::153`
- `2606:50c0:8003::153`

**CNAME record**: `www` → the GitHub Pages URL for this repo (e.g. `s-jac.github.io` or `freshwater-gazette.github.io` — check repo Settings → Pages to confirm)

After DNS propagates: go to repo Settings → Pages → set Custom domain → tick **Enforce HTTPS**. GitHub Pages provisions a Let's Encrypt certificate automatically.

## Jekyll Config Notes

- Permalink for posts: `/blog/:title/` (no date in URL — avoid duplicate slugs)
- `future: true` — posts with future dates will be built
- `baseurl` is empty — use `{{ '/path/' | relative_url }}` for all internal links
- Markdown renderer is kramdown
- `_data/external_articles.yml` is a leftover from the old portfolio; it is no longer used

## Legacy Content (Preserved, Not Linked)

Still in the repo, functional, but not linked from the main nav:
- `projects/` — interactive project pages (fretboard tools, manuscript triage, etc.)
- `projects.html` — projects index
- `_reviews/` + `reviews.html` — book reviews collection
- Old CSS classes for these pages are kept in `styles.css` under the legacy section — do not remove them

## Local Dev

```bash
bundle install
bundle exec jekyll serve --livereload
# → http://localhost:4000
```
