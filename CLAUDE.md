# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
bundle install                  # install dependencies
bundle exec jekyll serve        # dev server at http://localhost:4000
bundle exec jekyll build        # build to _site/
```

No test or lint tooling is configured.

## Architecture

**Jekyll 4.3.3** static site. SCSS compiled by Jekyll. No external CSS frameworks — all styling is custom SCSS. Deployed to AWS S3 + CloudFront via CircleCI on push to `master`.

**Business:** Sea Glass Home Styling — home staging / interior styling.

### Styling (`_sass/base.scss` → `assets/css/main.scss`)
Single SCSS partial imported with `@use`. Uses `@use "sass:color"` for colour functions (never `darken()`/`lighten()` — deprecated in Dart Sass 3). Variables for the sea glass palette (`$color-aqua`, `$color-sage`, `$color-sand`, `$color-teal`) are defined at the top. Responsive breakpoints: `$bp-mobile: 768px`, `$bp-tablet: 960px`.

### Layouts & Includes
- `_layouts/default.html` — thin wrapper: includes `head.html`, `header.html`, `footer.html`
- `_layouts/home.html` — hero + services grid + portfolio preview + CTA band
- `_layouts/portfolio.html` — grid of all `_posts/`
- `_layouts/post.html` — single job: hero image, meta bar (location/date/type), body, photo gallery
- `_layouts/about.html` — banner + two-column (photo + bio)
- `_includes/header.html` — fixed header: top bar (phone + Instagram) + main bar (logo + nav). CSS-only hamburger menu using checkbox toggle (no JS).

### Portfolio / Blog Posts (`_posts/`)
Each post is a Jekyll markdown file named `YYYY-MM-DD-slug.md`. Front matter fields:
```yaml
layout: post
title: "Job title"
location: "Town, County"
date: YYYY-MM-DD
property_type: "e.g. 3-bedroom terrace"
main_image: /assets/images/posts/slug/main.jpg   # hero + card thumbnail
images:                                           # gallery on post page
  - /assets/images/posts/slug/1.jpg
  - /assets/images/posts/slug/2.jpg
```
Post images live in `assets/images/posts/<slug>/`.

### Key content locations
| What | Where |
|------|-------|
| Site title, phone, email | `_config.yml` |
| Homepage tagline/subtitle | `index.md` front matter |
| About me copy | `about.md` |
| Colour variables | `_sass/base.scss` (top of file) |
| Logo | `assets/images/seaglasslogo.png` |
| New portfolio job | Create `_posts/YYYY-MM-DD-slug.md` |

### Deployment
CircleCI (`.circleci/config.yml`) builds on all branches; deploys to S3 + invalidates CloudFront only on `master`. Requires `AWS_BUCKET_NAME` and `CLOUDFRONT_DISTRIBUTION_ID` env vars in CircleCI.
