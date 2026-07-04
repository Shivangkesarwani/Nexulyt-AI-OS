# Typography Specifications

This document defines the recommended fonts, sizing hierarchies, line heights, and letter-spacing guidelines for the **Nexulyt-AI-OS** codebase and user interfaces.

---

## 1. Recommended Fonts

To maintain clean, modern legibility across displays, we utilize three specialized typefaces:

* **Primary Sans-Serif (Headings & UI)**: **Inter** or **Outfit** (Google Fonts). These fonts offer neutral geometries and are optimized for high-density dashboard layouts and navigation text.
* **Secondary Body Text**: **Inter** (Regular, 400). Designed for reading comfort, offering excellent letter spacing at default body sizes.
* **Code & Monospace Accents**: **JetBrains Mono** or **Fira Code**. Designed with clear character distinctions (e.g. differentiating `l`, `1`, and `I`), perfect for terminal outputs, code blocks, and standard configurations.

---

## 2. Typography Sizing Hierarchy

All user interfaces and markdown files must adhere to this typography scale. Avoid setting arbitrary font sizes:

| Element | Rem Size | Pixel equivalent (16px base) | Font Weight | Line Height |
|---|---|---|---|---|
| **H1 Hero Title** | 2.25rem | 36px | 700 (Bold) | 1.2 |
| **H2 Section Header** | 1.5rem | 24px | 600 (Semi-Bold) | 1.3 |
| **H3 Block Header** | 1.25rem | 20px | 600 (Semi-Bold) | 1.4 |
| **Body Large** | 1.125rem | 18px | 400 (Regular) | 1.5 |
| **Body Default** | 1rem | 16px | 400 (Regular) | 1.5 |
| **Subtext / Captions** | 0.875rem | 14px | 400 (Regular) | 1.4 |
| **Monospace / Code** | 0.875rem | 14px | 400 (Regular) | 1.4 |

---

## 3. Letter-Spacing & Vertical Rhythm

* **Header Letter-Spacing**: Apply slightly tighter letter-spacing on large headings to keep titles cohesive:
  * H1: `tracking-tight` (approx. `-0.025em`).
  * H2: `tracking-tight` (approx. `-0.015em`).
* **Body Spacing**: Keep default body text tracking normal (`tracking-normal` or `0`).
* **Paragraph Spacing**: Separate paragraphs with vertical margin spacers equal to the font size (e.g., `mb-4` or `1rem` vertical margins).
* **Line Heights**: Never drop line heights below **1.5** for body text blocks to maintain readability.

---

## 4. Professional Recommendations

1. **Verify Line Wrapping**: Limit paragraph reading line lengths to a maximum of **75 characters** (or max-width 650px) to prevent eye strain during reading.
2. **Prioritize System Fonts Fallbacks**: Always specify system-level fallbacks in CSS rules to ensure pages render cleanly if external font networks experience lag:
   `font-family: 'Inter', system-ui, -apple-system, sans-serif;`
3. **Use Web-Safe Formats**: Load web fonts using optimized WOFF2 structures to reduce bundle weights.
