# CLAUDE.md

## Project Context

This is a frontend-only static AI articles website for `alkashef.ai` / `blog.alkashef.ai`.

The site is a simple Substack-style blog/archive:

* Articles are provided manually as Markdown files.
* A build script converts Markdown articles into static HTML pages.
* The generated site is published by GitHub Pages.
* The website has one main page with a sidebar listing articles.
* Clicking an article in the sidebar opens the generated static article page.
* No backend is required for the public website.
* Article generation is outside the scope of this repository.

Use only plain HTML, CSS, and JavaScript for generated site output.

A small local Node.js build script is allowed for Markdown-to-HTML conversion.

Allowed build-time dependency:

* `marked` for Markdown parsing

Do not introduce frontend frameworks, bundlers, transpilers, backend code, browser-side Markdown rendering, runtime dependencies, databases, CMS platforms, AI API calls, prompt-generation scripts, or unnecessary npm packages.

---

## Repository Scope

This repository is responsible only for:

```text
Manual Markdown articles → static HTML/CSS/JS website → GitHub Pages publication
```

Out of scope:

* Generating article content with AI.
* Calling OpenAI, Anthropic, or any other AI API.
* Managing prompts for report generation.
* Sending email newsletters.
* Subscriber management.
* Backend publishing workflows.

The repository should treat Markdown article files as the source of truth.

---

## Primary Goal

Build a simple, maintainable, automation-friendly static blog that can publish Markdown articles with minimal intervention.

Optimize for:

* Free hosting.
* Fast publishing.
* Markdown-first authoring.
* Clean article reading experience.
* Maintainable plain HTML/CSS/JS.
* Safe AI-assisted editing.
* Easy GitHub Pages deployment.

Avoid:

* CMS complexity.
* Runtime rendering.
* Heavy JavaScript.
* Vendor lock-in.
* Manual HTML editing.
* Over-engineering.

---

## Required Project Structure

```text
/source/
  /articles/
    2026-05-06-first-test-article.md

/site/
  index.html
  /articles/
    2026-05-06-first-test-article.html
  /assets/
    /css/
      styles.css
    /js/
      main.js
    /images/

/tools/
  build-site.js

README.md
CLAUDE.md
```

Rules:

* Edit article content only in `/source/articles/`.
* `/site/` is generated output.
* Do not manually edit generated HTML in `/site/`.
* `build-site.js` generates all public HTML pages.
* Keep shared CSS/JS in `/site/assets/`.
* Generated article files must use stable filenames.
* Use relative links only.

---

## Article Authoring Format

Each article is a single Markdown file in `/source/articles/`.

Filename pattern:

```text
YYYY-MM-DD-short-slug.md
```

Each article starts with YAML-like metadata followed by Markdown body.

Example:

```md
---
title: Weekly AI World Report — 2026-05-06
date: 2026-05-06
summary: A practical weekly summary of major AI updates for practitioners.
tags: [AI, weekly report, tools, research]
status: published
---

# Weekly AI World Report — 2026-05-06

## Executive Summary

- **Most important update:** ...
- **Most useful tool release:** ...
- **Trend to watch:** ...

## Key Developments

Content goes here.
```

Metadata rules:

* `title` is required.
* `date` is required.
* `summary` is optional but recommended.
* `tags` are optional.
* `status` is optional; default is `published`.
* Articles with `status: draft` must not be published.

Body rules:

* Use normal Markdown.
* Use one `#` heading at the top.
* Use `##` for major sections.
* Prefer lists over tables unless comparison genuinely needs a table.
* Keep article structure consistent across generated reports.

---

## Site Layout Requirements

The generated website must have a single simple reading interface.

Main layout:

* Left sidebar: article list.
* Main content: selected article.
* Header area: site title and short description.
* Footer: simple static footer.

Sidebar requirements:

* List all published articles in one chronological list.
* Sort articles by date descending.
* Show article title and date.
* Highlight the currently selected article.
* Use simple links to static article pages.

Article page requirements:

* Reuse the same sidebar on every article page.
* Display article title, date, summary, tags, and body.
* Include link back to home.
* Use readable article width.
* Avoid clutter.

Home page requirements:

* `site/index.html` should show the latest article by default.
* Sidebar should list all published articles.
* If no articles exist, show a clear empty-state message.

---

## Design System Rules

Maintain one shared visual system across the whole site.

The design should feel like a clean AI research/newsletter publication:

* Desktop-first.
* Readable typography.
* Calm professional tone.
* High contrast.
* Generous whitespace.
* Strong article hierarchy.
* Minimal decorative effects.
* Sidebar that remains usable with many articles.

Avoid:

* Overly decorative visuals.
* Complex animation.
* Dense layouts.
* Magazine-style clutter.
* Mobile-first complexity unless requested.

Design system must cover:

* Page layout.
* Sidebar.
* Article typography.
* Tags.
* Links.
* Buttons if needed.
* Empty states.
* Responsive fallback.

---

## Build Script Responsibilities

Use Node.js for the build script.

Allowed dependency:

* `marked`

The dependency is build-time only. The generated site must not depend on `marked` or any runtime Markdown parser.

`tools/build-site.js` must:

* Read all Markdown files from `/source/articles/`.
* Parse article metadata.
* Ignore draft articles.
* Convert Markdown body to HTML.
* Generate `site/index.html`.
* Generate one HTML file per article in `/site/articles/`.
* Generate the sidebar article navigation.
* Copy or ensure shared CSS/JS assets are available.
* Use a single reusable page template.
* Use relative links.
* Fail clearly when required metadata is missing.

Do not:

* Render Markdown in the browser.
* Fetch articles at runtime.
* Generate content dynamically in client-side JavaScript.
* Use a router.
* Use a database.
* Use a CMS.

---

## HTML Rules

Write simple semantic HTML.

Use elements such as:

* `header`
* `main`
* `article`
* `aside`
* `nav`
* `section`
* `footer`

Each generated page must include:

* Clear `<title>`.
* Main heading.
* Sidebar navigation.
* Article metadata where relevant.
* Relative links only.
* Accessible link text.

Do not use inline styles except rare one-off exceptions.

---

## CSS Rules

Keep CSS centralized in:

```text
/site/assets/css/styles.css
```

Use simple class names that describe purpose, not appearance.

Good examples:

* `.site-shell`
* `.site-header`
* `.article-sidebar`
* `.article-list`
* `.article-content`
* `.article-meta`
* `.tag-list`

Avoid vague or fragile names:

* `.box1`
* `.blue-section`
* `.thing`
* `.new-style`

CSS should be organized by sections:

```css
/* Base */
/* Tokens */
/* Layout */
/* Header */
/* Sidebar */
/* Articles */
/* Tags */
/* Footer */
/* Utilities */
/* Responsive */
```

Use CSS custom properties for design tokens:

```css
:root {
  --color-bg: ...;
  --color-text: ...;
  --color-muted: ...;
  --color-border: ...;
  --color-primary: ...;
  --space-md: ...;
  --font-main: ...;
}
```

Avoid frontend code smells:

* Duplicate CSS rules with minor variations.
* Repeated hardcoded values instead of tokens.
* Unused CSS classes.
* Excessive specificity.
* Inline styles.
* Layout-specific hacks.

---

## JavaScript Rules

Use JavaScript only when it improves navigation or usability.

Acceptable uses:

* Sidebar collapse on small screens.
* Active-link enhancement.
* Simple search/filter for articles.
* Keyboard shortcut for focusing search.

Avoid:

* Rendering article content with JavaScript.
* Client-side routing.
* Complex state management.
* Framework-like abstractions.
* Large functions.
* Unused helpers.

The site must work without JavaScript for basic reading and navigation.

---

## Article Navigation Rules

Article navigation is generated from Markdown files in `/source/articles/`.

Rules:

* Do not hardcode article links in HTML.
* Sort articles by `date` descending.
* Omit draft articles.
* Keep article titles readable.
* Use the article filename slug for the output HTML filename.
* Highlight the current article in the sidebar.

---

## GitHub Pages Rules

Generated output must be compatible with GitHub Pages.

Rules:

* Static files only.
* Relative links only.
* No server-side routing.
* No backend assumptions.
* No environment variables in browser code.
* Public site must not expose secrets.

The deployment target is:

```text
/site/
```

If GitHub Pages is configured to publish from repo root, keep an alternative option to copy generated files to root only when explicitly requested.

---

## README Requirements

Maintain a `README.md` file at the project root.

The README must explain:

* What the website is.
* The folder structure.
* How to add a new article manually.
* How to build the site.
* How to preview locally.
* How publishing to GitHub Pages works.
* Where images go.
* That generated HTML in `/site/` should not be manually edited.
* That `marked` is build-time only.

Update the README whenever project structure or conventions change.

---

## AI Coding Behavior

Before making changes:

* Inspect relevant existing files.
* Preserve the existing structure and conventions.
* Identify the smallest safe change.
* Confirm whether the task affects source content, build logic, styling, or deployment.

When making changes:

* Keep changes minimal and focused.
* Do not rewrite unrelated files.
* Do not introduce dependencies unless explicitly requested.
* Do not change folder structure unless explicitly requested.
* Reuse existing CSS classes where possible.
* Keep naming consistent.
* Prefer readable code over clever abstractions.

After making changes:

* Run or explain the build command.
* Check relative links.
* Check sidebar navigation.
* Check that drafts are excluded.
* Check that generated pages are static.
* Check that README remains accurate.

---

## Default Response Style

When explaining changes to the user:

* Be concise.
* State what changed.
* State which files changed.
* Mention the build/test command.
* Mention any follow-up needed.

Do not provide long tutorials unless requested.
