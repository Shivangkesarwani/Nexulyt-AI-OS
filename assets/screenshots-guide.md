# Screenshots Guide

This document defines the resolution standards, file naming rules, annotation practices, and directory layouts for all user interface screenshots included in the **Nexulyt-AI-OS** repository.

---

## 1. Resolution & Device Standards

To ensure screenshots render clearly on high-density displays (such as Retina or 4K screens) without causing massive page load delays, adhere to these resolution specifications:

* **Desktop Layouts**: Standard capture size of **1920x1080 pixels** (16:9 aspect ratio) or **2560x1440 pixels** for high-resolution displays. Export assets at **2x pixel density** for crisp rendering, and downscale output files.
* **Mobile Layouts**: Standard capture size matching modern viewport scales (e.g. **1170x2532 pixels** or standard 9:19.5 aspect ratio).
* **Format**: Always save screenshots as **PNG** (for clean UI line renders) or optimized **WebP** files. Avoid low-quality JPEGs which introduce visual compression artifacts around text.

---

## 2. Spacing & Container Framing

* **Border Frame**: Enforce a thin **1px border** (`border-slate-200` or `border-slate-800` depending on theme) around all screenshots inserted into white-background documents to prevent the image edges from blending into the page.
* **Aspect Ratios Consistency**: Keep screenshots cropped strictly to content boundaries, avoiding unnecessary browser address bars, system clocks, desktop shortcuts, or browser tabs.
* **Shadow Details**: Apply a soft, low-contrast shadow overlay (`box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1)`) on image files inserted into web pages to create depth.

---

## 3. Annotation & Highlight Standards

When illustrating specific features or interface elements within screenshots, follow these styling rules:
* **Highlight Borders**: Use a bold **2px border outline** with high-contrast colors (e.g., Neon Violet `#8B5CF6` or Crimson `#EF4444`) to frame the target area.
* **Annotation Labels**: Include clean text boxes using sans-serif typography (e.g., Inter) set in white text on dark backgrounds.
* **Arrow pointers**: Connect labels to targets using straight lines or 90-degree elbows rather than freehand annotations.

---

## 4. Directory Organization & Naming Rules

To keep the repository clean, store and name screenshots systematically:
* **Directory Path**: Save all screenshots inside the `assets/screenshots/` folder.
* **Naming Pattern**: Use lowercase kebab-case naming structures containing category and viewport identifiers:
  * Format: `[feature-name]-[viewport-type].png`
  * Example: `dashboard-analytics-desktop.png`, `billing-modal-mobile.png`.

---

## 5. Professional Recommendations

1. **Optimize Before Commit**: Run screenshots through optimization tools (e.g., OptiPNG, SVGO) to strip metadata, reducing file size by up to 60%.
2. **Hide Sensitive Data**: Always blur, crop, or use fake placeholder text to mask API keys, user emails, personal names, or credentials prior to capture.
3. **Capture in Dark Mode**: When documenting developer panels, run the capture in dark mode configurations to maintain consistency with the repository visual style.
