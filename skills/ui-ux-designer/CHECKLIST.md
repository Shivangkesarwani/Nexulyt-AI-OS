# UI/UX Designer QA & Review Checklist

Use this checklist to perform a comprehensive audit of user interfaces before promoting code to production. Ensure all checkpoints are verified.

---

## 1. LAYOUT & GRIDS
- [ ] **Spacing Grid:** All paddings, margins, and gaps align strictly to the 8pt spacing grid system (4px, 8px, 12px, 16px, 24px, 32px, 48px, 64px, 96px, 128px).
- [ ] **Structural Alignment:** All parent containers and child components align cleanly to the horizontal grid system.
- [ ] **Negative Space:** Whitespace is used as an active layout asset to separate unrelated sections and create breathing room.
- [ ] **Flexbox & Grid:** Modern CSS Flexbox (1D layouts) or CSS Grid (2D layouts) is used for all layout alignments instead of absolute positioning.
- [ ] **Borders & Dividers:** High-contrast borders are avoided; borders use low-opacity colors (e.g., `rgba(255, 255, 255, 0.08)` on dark mode, `rgba(0, 0, 0, 0.06)` on light mode).

---

## 2. TYPOGRAPHY
- [ ] **Type Scale:** Text sizes follow a mathematical type scale (e.g., 1.250 Major Third) to ensure visual hierarchy.
- [ ] **Line Heights:** Headings use line heights between 1.1 and 1.3, while body copy uses 1.4 to 1.6 to prevent lines from overlapping.
- [ ] **Line Length:** Body paragraphs are constrained to a maximum width of 65 characters (approx. 450px to 600px) to support reading comfort.
- [ ] **Font Pairing:** No more than two font families are used on the project.
- [ ] **Contrast & Weight:** Contrast is established through typographic weights (e.g., Medium, Bold) rather than excessive variations in size.

---

## 3. COLOR
- [ ] **60-30-10 Rule:** Colors are balanced: 60% dominant background, 30% structural components/borders, and 10% accent color.
- [ ] **Canvas Bases:** Pure blacks (`#000000`) and pure white canvases are avoided; dark mode uses deep grays (e.g., `#0a0a0a`) to reduce eye strain.
- [ ] **Status Colors:** Colors for system status (success, warning, error) are used consistently across the application.
- [ ] **Functional Accent:** Saturated accent colors are reserved for actionable controls and status notifications.

---

## 4. ACCESSIBILITY (WCAG 2.2)
- [ ] **Color Contrast:** All text-to-background combinations meet WCAG 2.2 AA contrast ratios (4.5:1 for normal text, 3:1 for large text).
- [ ] **Keyboard Navigation:** Every interactive element can be focused and activated using only the keyboard.
- [ ] **Focus Indicator:** Clear, visible `:focus-visible` styles are provided for all interactive elements.
- [ ] **ARIA & Labels:** All icon-only buttons include descriptive `aria-label` tags for screen readers.
- [ ] **Alt Text:** Descriptive alternative text is provided for all contextual images.
- [ ] **Text Scaling:** The interface remains fully functional and legible when text is zoomed up to 200%.

---

## 5. RESPONSIVE DESIGN
- [ ] **Fluid Scaling:** Relative units (`%`, `rem`, `em`, `vh`, `vw`) are used for container sizing instead of fixed pixel widths.
- [ ] **Parent Container Queries:** CSS container queries (`@container`) are used to let components adapt directly to their parent container's width.
- [ ] **Breakpoints:** Layout changes occur at logical, fluid intervals instead of rigid, device-specific breakpoints.
- [ ] **No Horizontal Scroll:** Layout elements wrap cleanly, preventing horizontal scrollbars on smaller viewports.

---

## 6. USER EXPERIENCE (UX)
- [ ] **Fitts's Law:** Interactive controls are grouped logically, and primary actions are placed in easily reachable viewport positions.
- [ ] **Real-time Validation:** Form validations occur immediately after focus leaves a field (on blur) to correct errors early.
- [ ] **Destructive Action Safety:** Confirm dialogs are provided for irreversible actions like account deletions.
- [ ] **State Preservation:** Form inputs are saved locally in drafts so users don't lose data if they accidentally refresh the page.
- [ ] **Progressive Disclosure:** Advanced options are hidden behind sliders, accordion panels, or menus to prevent cognitive overload.

---

## 7. PERFORMANCE-AWARE DESIGN
- [ ] **JS Bundle Size:** The initial JavaScript bundle size does not exceed 100KB gzipped.
- [ ] **Asset Compression:** All images are compressed and exported in modern, web-optimized formats (e.g., WebP or SVG).
- [ ] **System Font Fallback:** Web fonts are loaded asynchronously, and native system fonts are used as fallbacks to prevent layout shifts.
- [ ] **Core Web Vitals:** The page is optimized to meet the target thresholds: LCP $\le 1.2\text{s}$, INP $\le 40\text{ms}$, CLS $\le 0.05$.

---

## 8. ANIMATIONS & MOTION
- [ ] **Functional Motion:** Motion is used to guide attention or clarify actions; decorative animations that delay user workflows are removed.
- [ ] **Timing Limits:** Micro-interactions (hover, active) complete in 100ms-150ms; larger transitions (modals, slide-ins) complete in 200ms-300ms.
- [ ] **Reduced Motion Support:** All transitions respect system accessibility settings and disable animations when `prefers-reduced-motion` is enabled.
- [ ] **Hardware Acceleration:** Animations are restricted to CSS `transform` and `opacity` properties to prevent browser rendering lags.

---

## 9. CONVERSION OPTIMIZATION
- [ ] **Hero Section CTA:** A primary value proposition and a clear CTA button are visible above the fold.
- [ ] **Visual Dominance:** Primary actions use high-contrast solid backgrounds to stand out from secondary actions.
- [ ] **Minimal Signup Friction:** Registration forms ask only for essential information to simplify user onboarding.
- [ ] **Social Proof:** Customer testimonials, security badges, or trust logos are placed near key checkout actions.

---

## 10. MOBILE AUDIT
- [ ] **Touch Target Size:** All buttons, links, and form elements have a minimum touch target area of $48 \times 48\text{px}$.
- [ ] **Gutter Margins:** Screen gutter padding is set to a minimum of 16px to prevent content from touching mobile screen edges.
- [ ] **Thumb Reach Zone:** Primary actions on mobile are placed within easy reach of the thumb (e.g., lower half of the screen).
- [ ] **Keyboard Layouts:** Appropriate keyboard types (e.g., `type="email"`, `type="number"`) are configured on form fields.

---

## 11. DESKTOP AUDIT
- [ ] **Max Width Constraint:** Content grids use a maximum width constraint (e.g., `max-width: 1280px` or `1440px`) to prevent layouts from expanding too wide on ultra-wide monitors.
- [ ] **Keyboard Shortcuts:** Common operations (such as search inputs, modal close, and saving) support standard keyboard shortcut keys.
- [ ] **Hover Polish:** Visual states animate smoothly when hovered over with a cursor pointer.

---

## 12. SEARCH ENGINE OPTIMIZATION (SEO)
- [ ] **Heading Structure:** The page contains exactly one `<h1>` tag and follows a logical heading hierarchy (`H1` -> `H2` -> `H3`).
- [ ] **Indexable Copy:** All menus, product lists, and text descriptions are written as indexable HTML text rather than embedded in image files.
- [ ] **Metadata Schema:** Meta description tags and JSON-LD structured schemas are defined correctly.

---

## 13. FINAL QA SIGN-OFF
- [ ] **Broken Link Scan:** All layout navigation anchors and external footer links point to valid, live URLs.
- [ ] **Cross-browser Compatibility:** The interface is tested and verified on Chrome, Safari, Firefox, and Edge.
- [ ] **State Coverage:** Skeletons, spinners, empty views, error alerts, and success messages are designed and verified.
