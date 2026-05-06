# copilot-instructions.md

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

Help build a simple, maintainable, desktop-first static website that can be edited safely by a non-frontend developer relying on AI assistance.

Optimize suggestions for:

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

* Instructors edit only `/source/`.
* `/site/` is generated output.
* Do not manually edit generated HTML.
* Build script generates all pages.
* Keep shared CSS/JS in `/assets/`.

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
instructor: ...
---

# Slide 01: Title
tags: []

// content

---

# Slide 02: Title
tags: [demo, hands-on]

## Instructions

// steps
```

Rules:

* Each slide starts with `# Slide XX: Title`.
* Slides are separated by `---`.
* Tags control behavior: theme, UI, labels.
* Sections are optional but standardized.

---

## Slide Layout Rules

Each generated slide page must include:

* Top area:

  * Slide title
  * Optional tags as pills: demo, quiz, hands-on, homework
* Sidebar:

  * Slide number
  * Clickable list of slides as table of contents
  * Current slide clearly highlighted
* Main content area
* Bottom navigation:

  * Previous
  * Next
  * Progress bar

Progress bar requirements:

* Horizontal segmented bar, one segment per slide.
* Each segment must be clickable and link to its corresponding slide.
* Completed slides shown as filled segments.
* Current slide clearly highlighted.
* Remaining slides shown as muted segments.
* Include slide index indicator, e.g. `10 / 16`.
* Use a linear segmented navigation style.

---

## Build Script Responsibilities

Use Node.js for the build script.

Allowed dependency:

* `marked` for Markdown-to-HTML conversion

The dependency must be build-time only. The generated site must not depend on `marked` or any runtime library in the browser.

Do not use bundlers, frontend frameworks, backend services, or browser-side Markdown rendering.

The build script must:

* Parse course metadata.
* Split slides.
* Generate:

  * Course `index.html`
  * One HTML file per slide
* Generate sidebar navigation.
* Generate clickable progress bar.
* Apply dark theme for tagged slides.
* Apply consistent layout and section styles.

---

## HTML Rules

Suggest simple semantic HTML.

Use elements such as:

* `header`
* `main`
* `section`
* `article`
* `nav`
* `footer`

Each generated page must include:

* Clear page title.
* Main heading.
* Navigation where relevant.
* Link back to the course page or landing page.

Each generated slide page must include:

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

* Light theme by default.
* Dark theme for tagged slides: demo, quiz, hands-on, homework.

Rules:

* Theme must be controlled via a class on the root element, e.g. `body.dark-theme`.
* Do not duplicate layouts for themes; only override colors and contrast.
* Ensure readability in both themes.

Tags as pills:

* Must be visually distinct.
* Must map clearly to slide type.
* Should be reusable components.

---

## CSS Rules

Keep CSS centralized in `/site/assets/css/styles.css`.

Use simple class names that describe purpose, not appearance.

Good examples:

* `.site-header`
* `.course-card`
* `.slide-layout`
* `.slide-nav`
* `.content-grid`
* `.tag-pill`
* `.progress-segment`

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
/* Sidebar */
/* Progress Navigation */
/* Tags */
/* Standard Sections */
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

Do not duplicate styles across generated HTML files.

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

Do not use browser-side JavaScript to generate slides from Markdown.

Acceptable JavaScript uses:

* Highlighting active navigation.
* Simple keyboard navigation between slides.
* Small UI helpers.

Avoid:

* Complex state management.
* Dynamic routing.
* Browser-side Markdown rendering.
* Framework-like abstractions.

---

## Slide Design Rules

Slides are HTML pages, but they should feel like teaching slides.

Each slide should usually contain:

* One main idea.
* A clear title.
* Optional tags: demo, quiz, hands-on, homework.
* Sidebar with slide navigation.
* Bottom navigation with progress.
* Short bullets or concise explanation.
* Optional illustration or diagram.

Bullet style rules:

* All bullet points must start with `//`.
* Keep bullets concise and structured.

Standard slide sections, optional when applicable:

* Takeaways
* References
* Instructions
* Deliverables

Rules for sections:

* Sections are optional; do not force them on every slide.
* When present, they must follow a consistent shared style.
* Use consistent headings, spacing, and typography across all slides.
* Do not mix section styles between slides.

Avoid overcrowding slides.

When adding content, prefer:

* One concept per slide.
* Short paragraphs.
* Structured lists.
* Simple diagrams when helpful.

Special slides tagged demo, quiz, hands-on, or homework:

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
* How to add a new course Markdown file.
* How to add a new slide inside a course Markdown file.
* How slide tags work.
* How standard slide sections work.
* Where to place images.
* How navigation is generated.
* That the generated site uses plain HTML, CSS, and JavaScript only.
* That `marked` is allowed only as a build-time Markdown parser.
* How to install dependencies.
* How to run the build script.

Update the README whenever project structure or conventions change.

---

## Copilot Behavior

When suggesting code:

* Preserve existing structure and conventions.
* Prefer the smallest safe change.
* Do not rewrite unrelated files.
* Do not introduce dependencies except the approved build-time `marked` dependency.
* Do not change folder structure unless asked.
* Reuse existing CSS classes where possible.
* Keep naming consistent.
* Prefer explicit, readable code over compact clever code.
* Treat `/site/` HTML as generated output, not source-of-truth content.

Before suggesting new code, consider:

* Whether the change belongs in `/source/`, `/tools/`, or `/site/assets/`.
* Whether an existing class can be reused.
* Whether the build script should generate the change.
* Whether relative paths are correct.
* Whether the README should be updated.

After suggesting changes, check:

* Markdown parsing.
* Generated relative links.
* Navigation paths.
* Shared CSS reuse.
* README accuracy.

---

## Default Explanation Style

When explaining suggested changes:

* Be concise.
* State what changed.
* State which files changed.
* Mention any follow-up needed.

Do not provide long tutorials unless requested.
