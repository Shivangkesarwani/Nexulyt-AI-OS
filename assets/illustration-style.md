# Illustration Style Guidelines

This document defines the visual style, color balance, geometric overlays, and rendering guidelines for all technical illustrations and graphical overlays representing the **Nexulyt-AI-OS** brand.

---

## 1. Core Illustration Style

Our illustration style is designed to be **minimalist, vector-based, and highly structured**. We avoid generic flat-design cartoons, hand-drawn styles, or heavy textures.

### Key Visual Attributes
* **Flat Vector Geometry**: Use crisp, clean geometric shapes (circles, hexagons, lines, nodes) rather than organic or freeform drawings.
* **Subtle Gradients**: Apply smooth linear gradients matching primary brand color tokens (Indigo to Neon Violet) to give illustrations depth.
* **Isometrics & Grids**: Technical system illustrations must be modeled on isometric or flat layouts to maintain spatial consistency.
* **Sleek Shadows**: Use soft shadow parameters to isolate foreground shapes from dark backgrounds.

---

## 2. Color Balance & Spacing Boundaries

* **Background Colors**: Keep illustration canvases dark slate or charcoal (`#0B0F19` or `#05070C`), matching dark mode parameters.
* **Primary Accent Weights**: Enforce a strict **60-30-10 color balance rule**:
  * **60% Dominant**: Dark background neutrals (slate, charcoal, black).
  * **30% Secondary**: Structural lines, border boundaries, and grouping grids (slate-600, muted gray).
  * **10% Accent**: Bright brand colors (indigo, neon violet, success emerald) reserved for highlighting flow nodes or active state signals.
* **Whitespace Margins**: Maintain a minimum **10% padding boundary** around illustrations to prevent graphics from clipping against layout margins.

---

## 3. Diagram & Overlay Styles

* **Flow Lines**: All process flow arrows and relationship lines must be straight, vertical, or horizontal, using 90-degree elbows rather than freehand curves.
* **Glow Filters**: Apply subtle neon glows (`box-shadow: 0 0 12px rgba(139, 92, 246, 0.4)`) on active nodes to denote state triggers.
* **Font Conformance**: Any text embedded inside vector illustrations must use the primary sans-serif font (Inter) or monospace (JetBrains Mono) for code elements.

---

## 4. Professional Recommendations

1. **Keep Illustrations Functional**: Illustrations must serve to clarify processes or architecture; do not include graphics solely for decorative fillers.
2. **Export as Vector SVGs**: Ensure all illustrations are stored as SVGs to prevent pixelation on high-density displays.
3. **Audit Contrast**: Verify text labels embedded inside illustration shapes maintain a minimum **4.5:1** contrast ratio against their container background colors.
4. **Scrub Metadata**: Clean exported vector files using SVGO optimization scripts before checking them into git repositories.
