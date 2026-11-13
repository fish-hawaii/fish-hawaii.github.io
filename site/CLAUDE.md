# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
bundle install

# Serve locally with live reload
bundle exec jekyll serve

# Build site to _site/
bundle exec jekyll build
```

The local dev server runs at `http://localhost:4000`. The `_site/` directory is gitignored — GitHub Actions builds and deploys it automatically on push to `main`.

## Architecture

This is a Jekyll 4 static site deployed to GitHub Pages at `https://fish-hawaii.github.io`. There is no JavaScript framework, no build pipeline beyond Jekyll, and no Sass — all styles live in a single flat CSS file.

**Content layer** — All regulation content is authored in plain `.md` files at the repo root (e.g. `regulated_areas_hawaii_island.md`, `regulations_pages4-11.md`). These files use standard Jekyll front matter with `layout: page` and a `permalink`. They contain a mix of Markdown and raw HTML blocks (for `<figure>`, callout boxes, and tables).

**Layout layer** — Three layouts in `_layouts/`:
- `default.html` — wraps everything; contains the site header, nav, and footer
- `home.html` — extends `default`; adds the hero section with the satellite map image
- `page.html` — extends `default`; adds optional breadcrumb nav and wraps content in `.content-card`

**Styling** — `assets/css/main.css` is a single hand-written CSS file (~600 lines). It uses CSS custom properties defined in `:root` for the color palette. No preprocessor. Key component classes: `.card`, `.island-card`, `.map-fig`, `.species-fig`, `.note`/`.warning`/`.permitted`/`.prohibited` (callout boxes), `.hero`, `.page-toc`.

**Images** — All images live in `images/`. Map images follow the pattern `map_<island>_<location>.png`; species diagrams follow `species_<name>.png/jpg`. Referenced in markdown via `{{ '/images/filename.ext' | relative_url }}`.

**Deployment** — `.github/workflows/deploy.yml` builds with `bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"` and deploys via `actions/deploy-pages`. The `baseurl` in `_config.yml` is `""` (root org site).

**Content structure** — The `areas/index.md` page links to individual island pages. Island pages (e.g. `regulated_areas_oahu.md`) have `permalink: /areas/<island>/`. The `README.md` contains a full index of what regulation content lives in each `.md` file — consult it when looking for a specific regulation topic.
