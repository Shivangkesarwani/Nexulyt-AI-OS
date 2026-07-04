# UI Standards

This document establishes the user interface design tokens, layouts, grids, typography, and animation physics requirements within the **Nexulyt-AI-OS** repository.

---

## 1. Grid & Layout

- **8pt Grid System:** All dimensions, padding, margins, and layouts must be increments of 8px (or 4px for fine spacing).
- **Responsive Breakpoints:**
  - Mobile: `< 640px`
  - Tablet: `640px - 1024px`
  - Desktop: `> 1024px`
- **Flexbox & CSS Grid:** Use CSS Flexbox for linear elements and CSS Grid for two-dimensional views.

---

## 2. Typography & Color Systems

- **Font Scales:** Use standard, modern, accessible typography scales:
  - Headers: bold sans-serif (e.g. Inter, Outfit, system-ui).
  - Body: line-height set between 1.5 - 1.6 for readability.
- **Color Palettes:** Rely on functional, contrast-valid color tokens:
  - Primary (Brand indicators)
  - Secondary (Neutral helpers)
  - Error (Red, min 4.5:1 contrast)
  - Success (Green)
  - Warning (Orange)

---

## 3. Micro-Animations & Interaction Physics

- **Transitions:** Use smooth, physics-based transitions. Linear transitions are prohibited on interactive components.
  - Duration: `150ms - 200ms` for micro-interactions (hover, focus states).
  - Easing: Use `cubic-bezier(0.4, 0, 0.2, 1)` (ease-in-out) or custom spring profiles.
- **Visual Depth:** Master depth layers utilizing shadows, low-opacity borders, and glassmorphism.
