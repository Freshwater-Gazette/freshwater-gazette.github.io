# The Freshwater Gazette

Independent local news site built with Jekyll, hosted on GitHub Pages at [freshwatergazette.com](https://freshwatergazette.com).

## Tech Stack

| Component | Technology |
|-----------|------------|
| Static Site Generator | [Jekyll](https://jekyllrb.com/) |
| Hosting | [GitHub Pages](https://pages.github.com/) |
| Custom Domain | freshwatergazette.com (via CNAME) |
| Analytics | [GoatCounter](https://www.goatcounter.com/) — needs account setup |
| Comments | [Giscus](https://giscus.app/) — needs repo config |
| Newsletter | Mailchimp — needs account setup |

## Project Structure

```
├── _config.yml          # Jekyll configuration (title, URL, permalinks)
├── _layouts/
│   ├── default.html     # Site wrapper — header with nav, footer
│   ├── post.html        # News article page (headline, body, comments, read more)
│   ├── page.html        # Simple page wrapper
│   └── review.html      # Book review (kept from prior site)
├── _includes/
│   ├── head.html        # <head> — fonts, meta, structured data
│   ├── footer.html      # Footer + GoatCounter script
│   └── comments.html    # Giscus comments embed
├── _posts/              # Articles — YYYY-MM-DD-slug.md
├── assets/
│   ├── css/
│   │   └── styles.css   # All global styles (Gazette design)
│   │   └── *.css        # Project-specific styles (leave alone)
│   ├── js/              # JavaScript (project pages, pageviews)
│   └── images/          # Article images go here
├── projects/            # Interactive project pages (kept from prior site)
├── index.html           # News homepage (featured + grid layout)
├── blog.html            # Article archive at /blog/
├── subscribe.html       # Newsletter signup (Mailchimp placeholder)
├── contact.html         # Contact page
├── CNAME                # Custom domain: freshwatergazette.com
└── feed.xml             # RSS feed
```

## Running Locally

```bash
bundle install
bundle exec jekyll serve --livereload
```

Visit [http://localhost:4000](http://localhost:4000)

## Adding Articles

Create a file in `_posts/` named `YYYY-MM-DD-slug.md`:

```markdown
---
layout: post
title: "Your Headline Here"
date: 2026-04-20
description: "One-sentence summary for SEO and article card excerpt."
image: /assets/images/blog/your-photo.jpg
comments: true
---

Your article content in Markdown...
```

The `image` field is optional — a grey placeholder is shown if omitted. Images should be placed in `assets/images/blog/`.

## Design

- **All white** background, **thin black borders** separating header and footer only
- No borders between articles
- **ITC Cheltenham Bold** for headlines (Adobe Fonts — see setup below), falling back to Playfair Display
- **Source Serif 4** for article body text
- **Inter** for UI elements (nav, dates, meta)
- Homepage: featured article (50% width) → 3-column secondary row → 3-column further articles
- Article page: headline → byline → body (single column, max 720px) → Giscus comments → read more carousel

## Third-Party Service Setup

### ITC Cheltenham Bold (font)
1. Log in to [fonts.adobe.com](https://fonts.adobe.com) (requires Adobe Creative Cloud)
2. Search for "ITC Cheltenham", add it to a web project
3. Copy the kit `<link>` tag URL (looks like `https://use.typekit.net/XXXXXXX.css`)
4. Uncomment and update the line in `_includes/head.html`

### GoatCounter (analytics)
1. Create an account at [goatcounter.com](https://www.goatcounter.com/)
2. In `_includes/footer.html`, uncomment the `<script>` tag and replace the URL with your account's URL

### Giscus (comments)
1. Enable GitHub Discussions on this repo (Settings → Features → Discussions)
2. Visit [giscus.app](https://giscus.app), enter your repo details, and copy the generated values
3. Update `_includes/comments.html` with your `data-repo`, `data-repo-id`, and `data-category-id`

### Mailchimp (newsletter)
1. Create a Mailchimp account and set up an audience
2. Go to Audience → Signup forms → Embedded forms
3. Copy the embed HTML and replace the placeholder block in `subscribe.html`

## Custom Domain & HTTPS

The `CNAME` file at the repo root sets the custom domain to `freshwatergazette.com`. See AGENTS.md for full DNS setup instructions.

HTTPS is provisioned automatically by GitHub Pages (Let's Encrypt) once DNS has propagated and the domain is verified in repo Settings → Pages.

## Deployment

Push to `main` → GitHub Pages builds and deploys automatically.
