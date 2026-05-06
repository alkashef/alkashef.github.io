# CLAUDE.md

## Project Context

This is a frontend-only courses website for students.

Content authoring follows a build-based system:

* Instructors write course content in a single Markdown file per course.
* A build script converts Markdown into static HTML pages.

The website has:

* One landing page listing available courses.
* Multiple courses.
* Each course is defined by a single `.md` file.
* Each course contains a sequence of slides extracted from that file.
* Each slide is generated as a separate HTML page.

Use only plain HTML, CSS, and JavaScript for the generated site output.

A small local Node.js build script is allowed for Markdown-to-HTML conversion.

Allowed build-time dependency:

* `marked` for Markdown parsing

Do not introduce frontend frameworks, bundlers, transpilers, backend code, browser-side Markdown rendering, runtime dependencies, or unnecessary npm packages.

---

## Primary Goal

Build a simple, maintainable, desktop-first static website that can be edited safely by a non-frontend developer relying on AI assistance.

Optimize for:

* Clarity over cleverness.
* Consistency over novelty.
* Maintainability over advanced frontend patterns.
* Readable file structure.
* Easy manual editing.

---

## Required Project Structure

```text
/source/
  /courses/
    course-name.md

/site/
  index.html
  /courses/
    /course-name/
      index.html
      slide-01.html
      slide-02.html
  /assets/
    /css/
      styles.css
    /js/
      main.js
    /images/

/tools/
  build-site.js
```

Rules:

* Instructors edit only `/source/`
* `/site/` is generated output
* Do not manually edit generated HTML
* Build script generates all pages
* Keep shared CSS/JS in `/assets/`

---

## Design System Rules

Maintain one shared visual system across the whole site.

Do not create a new visual style per course unless explicitly requested.

The shared design system must cover:

* Page layout.
* Course cards.
* Slide layout.
* Typography.
* Colors.
* Buttons.
* Links.
* Navigation.
* Spacing.
* Illustration treatment.

Prefer a clean academic/training style:

* Large readable headings.
* Clear hierarchy.
* Generous whitespace.
* High contrast.
* Minimal visual clutter.
* Desktop-first layout.
* Layouts that scale cleanly with desktop screen size.
* Content width should adapt to large screens without becoming unreadably wide.

Avoid:

* Overly decorative effects.
* Complex animations.
* Dense visual layouts.
* Mobile-first complexity unless requested.

---

## Course Authoring Format

Each course is defined in a single Markdown file.

Structure:

* Top section: course metadata
* Body: slides separated by `---`

Example:

```md
---
course_title: ...
description: ...
audience: ...
time: ...
instructor-name: ...
instructor-title: ...
instructor-bio: ...
---

# Slide 01: Title
tags: []

- content

---

# Slide 02: Title
tags: [demo, hands-on]

## [Instructions]

- steps
```

Rules:

* Each slide starts with `# Slide XX: Title`
* Slides are separated by `---`
* Tags control behavior (theme, UI)
* Sections are optional and can have any name
* Section headings must use `## [Name]` syntax (square brackets)
* Plain `## heading` without brackets renders as a regular content heading

---

---

## Slide Layout Rules

Each slide page must include:

* Top area:

  * Slide title
  * Optional tags (pills): demo, quiz, hands-on, homework
* Sidebar:

  * Slide number
  * Clickable list of slides (table of contents)
  * Current slide clearly highlighted
* Main content area
* Bottom navigation:

  * Previous
  * Next
  * Progress bar

Progress bar requirements:

* Horizontal segmented bar (one segment per slide)
* Each segment must be clickable and link to its corresponding slide
* Completed slides shown as filled segments
* Current slide clearly highlighted
* Remaining slides shown as muted segments
* Include slide index indicator (e.g., `10 / 16`)
* Layout inspired by linear segmented navigation (like step progress UI)

---

## Build Script Responsibilities

Use Node.js for the build script.

Allowed dependency:

* `marked` for Markdown-to-HTML conversion

The dependency must be build-time only. The generated site must not depend on `marked` or any runtime library in the browser.

Do not use bundlers, frontend frameworks, backend services, or browser-side Markdown rendering.

The build script must:

* Parse course metadata
* Split slides
* Generate:

  * Course `index.html`
  * One HTML file per slide
* Generate sidebar navigation
* Generate clickable progress bar
* Apply dark theme for tagged slides
* Apply consistent layout and sections

---

## HTML Rules

Write simple semantic HTML.

Use elements such as:

* `header`
* `main`
* `section`
* `article`
* `nav`
* `footer`

Each page must include:

* Clear page title.
* Main heading.
* Navigation where relevant.
* Link back to the course page or landing page.

Each slide page must include:

* Course name.
* Slide number.
* Slide title.
* Main slide content.
* Previous slide link where applicable.
* Next slide link where applicable.
* Navigate up link to the course overview.
* Link back to the main landing page where appropriate.

Use relative links only.

---

## Theme Rules

Slides must support two themes:

* Light theme (default)
* Dark theme (for tagged slides: demo, quiz, hands-on, homework)

Rules:

* Theme must be controlled via a class on the root element (e.g., `body.dark-theme`).
* Do not duplicate layouts for themes; only override colors and contrast.
* Ensure readability in both themes.

Tags (pills):

* Must be visually distinct.
* Must map clearly to slide type.
* Should be reusable components.

---

## CSS Rules

Keep CSS centralized in `/assets/css/styles.css`.

Use simple class names that describe purpose, not appearance.

Good examples:

* `.site-header`
* `.course-card`
* `.slide-layout`
* `.slide-nav`
* `.content-grid`

Avoid vague or fragile names:

* `.box1`
* `.blue-section`
* `.thing`
* `.new-style`

CSS should be organized by sections:

```css
/* Base */
/* Layout */
/* Header */
/* Course Cards */
/* Slides */
/* Navigation */
/* Utilities */
```

Use CSS custom properties for design tokens:

```css
:root {
  --color-bg: ...;
  --color-text: ...;
  --color-primary: ...;
  --space-md: ...;
  --font-main: ...;
}
```

Do not duplicate styles across HTML files.

Avoid common frontend code smells:

* Duplicate HTML structures that should use the same shared classes.
* Duplicate CSS rules with minor variations.
* Very large CSS files without clear sections.
* Very large JavaScript functions.
* Unused CSS classes.
* Unused JavaScript.
* Inline styles except for rare one-off exceptions.
* Hardcoded repeated values instead of design tokens.
* Inconsistent naming conventions.
* Overly clever JavaScript for simple static behavior.

---

## JavaScript Rules

Use JavaScript only when it meaningfully improves navigation or usability.

Keep JavaScript simple and readable.

Do not use JavaScript to generate the slides unless explicitly requested.

Acceptable JavaScript uses:

* Highlighting active navigation.
* Simple keyboard navigation between slides.
* Small UI helpers.

Avoid:

* Complex state management.
* Dynamic routing.
* Rendering page content from JSON.
* Framework-like abstractions.

---

## Slide Design Rules

Slides are HTML pages, but they should feel like teaching slides.

Each slide should usually contain:

* One main idea.
* A clear title.
* Optional tags (demo, quiz, hands-on, homework).
* Sidebar with slide navigation.
* Bottom navigation with progress.
* Short bullets or concise explanation.
* Optional illustration or diagram.

Bullet style rules:

* Use regular Markdown bullets (`- item`)
* The build script renders them as styled `//` list items in HTML
* Keep bullets concise and structured

Section rules:

* Section headings use `## [Name]` syntax (square brackets)
* Any name is accepted — sections are not restricted to a fixed list
* Plain `## heading` without brackets is a regular content heading
* Sections are optional; do not force them on every slide
* When present, they must follow a consistent, shared style

Avoid overcrowding slides.

When adding content, prefer:

* One concept per slide.
* Short paragraphs.
* Structured lists.
* Simple diagrams when helpful.

Special slides (demo, quiz, hands-on, homework):

* Must use dark theme.
* Must emphasize interaction or action.
* Must be visually distinguishable from normal slides.

---

## Accessibility and Usability

Maintain basic accessibility:

* Use semantic HTML.
* Use meaningful link text.
* Add `alt` text to images.
* Ensure readable contrast.
* Do not rely on color alone to convey meaning.
* Keep font sizes readable on desktop screens.

---

## README Requirements

Maintain a `README.md` file at the project root.

The README must explain:

* What the website is.
* The folder structure.
* How to add a new course.
* How to add a new slide.
* Where to place images.
* How navigation should be updated.
* That the generated site uses plain HTML, CSS, and JavaScript only.
* That `marked` is allowed only as a build-time Markdown parser.
* How to install dependencies.
* How to run the build script.

Update the README whenever project structure or conventions change.

---

## AI Coding Behavior

Before making changes:

* Inspect the relevant existing files.
* Preserve the existing structure and conventions.
* Identify the smallest safe change.

When making changes:

* Keep changes minimal and focused.
* Do not rewrite unrelated files.
* Do not introduce dependencies.
* Do not change folder structure unless asked.
* Do not create duplicate CSS rules if an existing class can be reused.
* Keep naming consistent.

After making changes:

* Check relative links.
* Check navigation paths.
* Check that shared CSS is reused.
* Check that the README remains accurate.

---

## Default Response Style

When explaining changes to the user:

* Be concise.
* State what changed.
* State which files changed.
* Mention any follow-up needed.

Do not provide long tutorials unless requested.
