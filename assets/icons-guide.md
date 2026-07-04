# Icons Guide

This document defines the recommended icon libraries, sizing scales, stroke parameters, and styling consistency rules for the **Nexulyt-AI-OS** codebase and documentation.

---

## 1. Recommended Icon Libraries

To maintain visual cohesion, avoid mixing multiple distinct icon sets. We recommend these libraries:

* **Lucide React / Lucide (Default)**: Modern, clean, geometric line-art icons featuring consistent stroke weights and bounding boxes. Lucide is highly readable at small dimensions (16px to 24px).
* **Radix Icons**: Extremely minimalist, high-density icon designs optimized for professional dashboard panels and dense developer tool grids.
* **Heroicons**: Built by the Tailwind team, these icons pair perfectly with standard spacing patterns.

---

## 2. Spacing & Stroke Rules

Consistency in line weight and sizing is critical to maintaining a premium aesthetic.

* **Size Scales**:
  * **Navigation Headers / Inlines**: Sized at **16x16 pixels** (`w-4 h-4` in Tailwind).
  * **Default Control Buttons**: Sized at **20x20 pixels** (`w-5 h-5` in Tailwind).
  * **Sidebar / Section Tabs**: Sized at **24x24 pixels** (`w-6 h-6` in Tailwind).
  * **Hero Highlights / Alerts**: Sized at **32x32 pixels** or **48x48 pixels** (`w-8 h-8` or `w-12 h-12`).
* **Stroke Weight**: Enforce a default stroke weight of **2px** (or `stroke-2`) for all line icons. Avoid mixed stroke weights on the same viewport grid.

---

## 3. Styling & Color Alignment Rules

* **System States Mappings**:
  * **Primary Action / Identity**: Indigo / Violet (`text-indigo-500` / `text-violet-500`).
  * **Warning / Alert**: Amber (`text-amber-500`).
  * **Danger / Failure**: Crimson (`text-red-500`).
  * **Success / Completion**: Emerald (`text-emerald-500`).
  * **Neutral / Muted Info**: Slate (`text-slate-400`).
* **Hover Micro-Animations**: Interactive icons should respond to cursor hover by shifting color or applying a subtle vertical translation (e.g. `hover:-translate-y-0.5`).

---

## 4. Professional Recommendations

1. **Verify Scaling**: Scale icons down to 14px in high-density components (e.g. badge tags) to verify the internal geometry doesn't collapse.
2. **Accessible Icon Labels**: When rendering clickable icon buttons that feature no visual text labels, always include `aria-label` or `title` tags to describe the action to screen readers.
3. **Use SVGs directly**: Avoid exporting icons as heavy PNG raster files; use SVGs or inline components to preserve crisp rendering.
