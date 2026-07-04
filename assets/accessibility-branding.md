# Accessibility in Branding

This document defines the accessibility constraints and implementation requirements for all visual designs, logos, typography, and icon sets in the **Nexulyt-AI-OS** design system.

---

## 1. Color Contrast Rules

All text, icons, and status indicators must achieve minimum contrast metrics to remain readable for users with visual impairments:

* **WCAG 2.1 AA Compliance Target**: Enforce a minimum **4.5:1** contrast ratio for body text, and **3:1** for large header titles.
* **UI Controls & Borders**: Ensure interactive element boundaries and icon symbols maintain a minimum **3:1** contrast ratio against their surrounding background.
* **Approved Pairings**: Ensure developers use only validated contrast combinations (e.g. Slate-100 text on Slate-950 backgrounds) and avoid low-contrast overlays.

---

## 2. Typography & Readability Constraints

* **Minimum Readable Size**: Never use font sizes below **12px** (0.75rem) for subtext or captions. Default body text must remain at least **16px** (1rem).
* **Line Heights**: Set line heights to at least **1.5** for body copy blocks, and **1.3** for heading elements to prevent text lines from overlapping.
* **Readable Column Widths**: Restrict reading text lines to a maximum of **75 characters** (650px) to prevent layout-based reading exhaustion.
* **Avoid Justified Text**: Never set text-align parameters to `justify`, as the uneven spacing can confuse dyslexic readers. Use left-aligned text blocks instead.

---

## 3. Accessible Icons & Alt Attributes

* **Descriptive Alternative Text**: All branding images, mockups, and diagrams must include descriptive alternative text (via the `alt="..."` HTML property).
* **Descriptive Icon Labels**: Clickable icon buttons with no visible text labels must include `aria-label` or `title` tags to describe the action (e.g. `aria-label="Submit Invoice"`).
* **Scale Independence**: Ensure all SVGs define viewport size constraints (`viewBox="0 0 24 24"`) to enable scaling without visual distortion.

---

## 4. Professional Recommendations

1. **Test Under Simulated Visual Deficiencies**: Use browser developer tools to view user interfaces in simulated color-blindness modes (e.g., protanopia, deuteranopia).
2. **Automate Accessibility Auditing**: Run automated audits (e.g. axe-core, Lighthouse CI) in development pipelines to flag accessibility issues.
3. **Use Semantic HTML First**: Use native `<button>` and `<a>` elements for interactive elements to ensure built-in keyboard navigation support.
