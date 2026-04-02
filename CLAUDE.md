# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
bundle install

# Start local dev server (available at http://localhost:4000)
bundle exec jekyll serve

# Build site (output to _site/)
bundle exec jekyll build
```

No test or lint tooling is configured.

## Architecture

This is a **Jekyll 4.3.3** static site with SCSS styling and Tailwind CSS (via CDN). The build output in `_site/` is deployed to AWS S3 + CloudFront via CircleCI on push to `master`.

**Styling:** `_sass/base.scss` defines CSS variables (colors, fonts); `assets/css/main.scss` is the entry point that imports it. Tailwind is loaded via CDN in individual pages.

**Templates:** `_layouts/default.html` is the single layout template using Liquid. Pages use front matter to declare `layout: default`.

**Content locations:**
- Homepage: `index.md`
- Additional pages: root or `pages/` directory as `.md` files
- Blog posts: `_posts/YYYY-MM-DD-title.md`
- Images/logos: `logos/`
- Site metadata (title, URL): `_config.yml`

**Deployment:** CircleCI (`.circleci/config.yml`) builds on all branches but only deploys to S3/CloudFront on `master`. Requires `AWS_BUCKET_NAME` and `CLOUDFRONT_DISTRIBUTION_ID` env vars set in CircleCI.
