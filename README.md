# alkashef.github.io

A simple static blog site for `alkashef.ai` / `blog.alkashef.ai`.

This repository uses Markdown articles as the source of truth and generates a static website suitable for GitHub Pages.

## Project structure

- `/source/articles/`
  - Markdown article source files only
  - Each file uses `YYYY-MM-DD-short-slug.md`
  - Each file includes metadata and Markdown body
- `/site/`
  - Generated public website output
  - `index.html` is the home page
  - `/articles/` contains generated article pages
  - `/assets/css/styles.css` contains shared CSS
  - `/assets/js/main.js` contains optional browser helpers
  - `/assets/images/` stores site images
- `/tools/build-site.js`
  - Build script that reads Markdown source and generates the static site

## How this site works

1. Author article content in Markdown under `/source/articles/`.
2. Run the build script to convert Markdown into static HTML under `/site/`.
3. Publish the generated files from `/site/` to GitHub Pages.

## Adding a new article

1. Create a new Markdown file in `/source/articles/`.
2. Name it like `2026-05-06-first-test-article.md`.
3. Start the file with metadata:

```md
---
title: Weekly AI World Report — 2026-05-06
date: 2026-05-06
summary: A practical weekly summary of major AI updates for practitioners.
tags: [AI, weekly report, tools, research]
status: published
---
```

4. Add the article body in Markdown below the metadata.
5. Use `status: draft` for work-in-progress articles; drafts are excluded from the published site.

## Build the site

Run the static site build script from the repository root:

```powershell
node tools/build-site.js
```

This script should:

- Read Markdown files from `/source/articles/`
- Parse required metadata
- Ignore drafts
- Convert Markdown to HTML
- Generate `/site/index.html`
- Generate one HTML file per article in `/site/articles/`
- Copy shared CSS/JS assets to `/site/assets/`
- Use relative links only

## Preview locally

Open `/site/index.html` in your browser, or use a simple local HTTP server if you prefer.

Example with Python:

```powershell
python -m http.server --directory site 8000
```

Then visit `http://localhost:8000`.

## Publishing to GitHub Pages

The generated `/site/` folder is the public site output.

- Publish from `/site/` when GitHub Pages is configured for the `site` folder.
- If the repository is published from the repository root, copy the generated files to the root only when explicitly requested.

## Images

Place site images in `/site/assets/images/` and use relative URLs from generated HTML.

If images are part of article content, keep paths relative and ensure the build process preserves them in the generated `/site/` output.

## Important notes

- Do not manually edit generated HTML files in `/site/`.
- The Markdown files in `/source/articles/` are the source of truth.
- `marked` is allowed only as a build-time dependency for Markdown conversion.
- The public site must remain static and use no runtime Markdown rendering.
