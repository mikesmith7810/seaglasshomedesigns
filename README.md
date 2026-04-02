# Sea Glass Home Designs

Website for Sea Glass Home Designs. Built as a static site and hosted on AWS.

## Tech Stack

- **[Jekyll](https://jekyllrb.com/) 4.3.3** — Ruby-based static site generator
- **SCSS** — compiled automatically by Jekyll
- **Tailwind CSS** — loaded via CDN
- **Hosting** — AWS S3 + CloudFront CDN
- **CI/CD** — CircleCI (auto-deploys on push to `master`)

## Project Structure

```
seaglasshomedesigns/
├── .circleci/config.yml     # CI/CD pipeline config
├── _layouts/default.html   # Main HTML wrapper template
├── _sass/base.scss          # CSS variables (colors, fonts)
├── assets/css/main.scss     # Main stylesheet (imports base.scss)
├── logos/                   # Logo and image assets
├── index.md                 # Homepage content
├── pages/                   # Additional pages
├── _config.yml              # Jekyll site config (title, url, baseurl)
├── Gemfile                  # Ruby dependencies
└── _site/                   # Generated output (do not edit directly)
```

## Running Locally

**Prerequisites:** Ruby and Bundler installed.

```bash
# Install dependencies
bundle install

# Start local dev server
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

## Building

```bash
bundle exec jekyll build
```

Output is generated into the `_site/` directory.

## Deployment

Deployment is fully automated via CircleCI. Pushing to the `master` branch triggers the pipeline which:

1. Builds the Jekyll site
2. Syncs `_site/` to the AWS S3 bucket
3. Invalidates the CloudFront cache

Changes are live within a few minutes of merging to `master`.

### Required CircleCI Environment Variables

| Variable | Description |
|----------|-------------|
| `AWS_BUCKET_NAME` | S3 bucket name for static file hosting |
| `CLOUDFRONT_DISTRIBUTION_ID` | CloudFront distribution ID for cache invalidation |

## Updating Content

| What | Where |
|------|-------|
| Homepage copy/layout | `index.md` |
| Logo/images | `logos/` — update the path in `index.md` |
| Colors and fonts | `_sass/base.scss` |
| Site title and URL | `_config.yml` |
| Add a new page | Create a `.md` file in root or `pages/` |
| Add a blog post | Create `_posts/YYYY-MM-DD-title.md` |