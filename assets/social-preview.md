# Social Preview Guidelines

This document defines the layout specifications and presentation rules for the **Nexulyt-AI-OS** GitHub social preview image (OpenGraph card) in compliance with GitHub platform standards.

---

## 1. GitHub Social Preview Specifications

GitHub social preview images represent the repository when links are shared on communication platforms (such as Twitter/X, LinkedIn, Discord, Slack, and messaging apps).

* **Target Resolution**: **1280x640 pixels** (2:1 aspect ratio).
* **Maximum File Size**: **1MB** (target under **150KB** to ensure fast preview generation).
* **Format**: PNG, JPG, or static WebP (SVG is not supported as a direct social preview format on GitHub).

---

## 2. Content Spacing & Framing Constraints

To prevent search engines, social platforms, and chat bubbles from truncating text or cropping branding details, all critical contents must reside within a **Safe Core Area**:

```
+-------------------------------------------------------+
|  [ Outer Margin - 80px ]                              |
|  +-------------------------------------------------+  |
|  |             [ SAFE CORE AREA ]                  |  |
|  |                                                 |  |
|  |   All logo symbols, repository names, value     |  |
|  |   descriptions, and key attributes must fit     |  |
|  |   inside this 1120x480px boundary.              |  |
|  |                                                 |  |
|  +-------------------------------------------------+  |
|                                                       |
+-------------------------------------------------------+
```

* **Outer Margin (80px)**: The outer border area must be kept completely clear of text, titles, or branding elements. Only background colors or decorative geometric patterns are allowed to bleed into this zone.
* **Safe Core Area (1120x480px)**: All critical branding information must fit within this centered bounding box to ensure it renders correctly on rounded chat cards.

---

## 3. Typography & Styling Controls

To optimize legibility on low-resolution previews:
* **Minimum Font Size**: Ensure description text is at least **24px** and header titles are at least **48px** to remain readable when scaled down to mobile link bubbles.
* **Color Contrast**: Utilize high-contrast styling tokens (e.g., pure white text on deep charcoal backgrounds).
* **Visual Density**: Limit information to the system logo, project title, and a single-sentence value proposition. Avoid cluttered feature lists or complex diagrams on the social preview image.

---

## 4. Professional Recommendations

1. **Verify Card Rendering**: Use OpenGraph check tools (e.g. opengraph.xyz) or Slack/Discord link previews to verify look and text readability before committing.
2. **Export Settings**: When exporting the final PNG from design tools, run it through optimization utilities (e.g., Tinypng) to strip unneeded metadata, keeping file weight small.
3. **Contrast Verification**: Confirm that primary text pairings achieve a minimum contrast ratio of **4.5:1** matching WCAG guidelines.
