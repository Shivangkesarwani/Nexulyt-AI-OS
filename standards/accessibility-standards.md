# Accessibility Standards

This document establishes the accessibility requirements (Web Content Accessibility Guidelines - WCAG 2.1 AA) for all user interfaces built within the **Nexulyt-AI-OS** ecosystem.

---

## 1. Semantic HTML Guidelines

- **HTML5 Tags:** Utilize correct structural tags (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`, `<article>`) instead of nested default `<div>` blocks.
- **Heading Nesting:** Enforce correct hierarchy (H1 → H2 → H3) without skipping levels for document outlines.
- **Button vs Link:** Use `<button>` for actions that trigger changes on the current page; use `<a>` for actions that navigate to a new URL.

---

## 2. Keyboard Navigation

- **Focus Indicators:** Interactive elements must have a visible, high-contrast `:focus` indicator. Custom outline removals are prohibited unless replaced with equivalent styles.
- **Logical Tab Order:** Ensure elements can be traversed using the tab key in a logical, reading-order flow.
- **Keystroke support:** Modal overlays must close on `Escape` key trigger; interactive elements must execute on `Enter` or `Space`.

---

## 3. ARIA & Contrast Rules

- **Contrast ratio:** Ensure text satisfies a minimum contrast ratio of 4.5:1 (or 3:1 for large text headings).
- **ARIA Roles & States:** Inject appropriate accessibility state properties (e.g. `aria-expanded`, `aria-hidden`, `aria-describedby`) for custom complex elements.
- **Form Labels:** Every `<input>` field must have a corresponding, programmatically linked `<label>` or explicit `aria-label`.
