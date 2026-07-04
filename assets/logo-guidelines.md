# Logo Guidelines

This document defines the usage rules, spacing requirements, size boundaries, and light/dark mode configurations for the **Nexulyt-AI-OS** logo.

---

## 1. Logo Spacing & Safe Area

To ensure the logo maintains visual impact and remains readable, always surround the mark with a minimum clear space buffer.

### Safe Area Formula
* The safe area boundary is calculated using the height of the logo icon symbol itself (denoted as **X**).
* Enforce a minimum margin of **0.5X** on all four sides of the logo block boundary.
* No text, border lines, graphic patterns, or other visual elements are allowed to enter this safe zone.

```
       +------------------------------------+
       |              0.5X Margin           |
       |         +----------------+         |
       |  0.5X   |  [ LOGO ICON ] |  0.5X   |
       |  Margin |  [ LOGO TEXT ] |  Margin |
       |         +----------------+         |
       |              0.5X Margin           |
       +------------------------------------+
```

---

## 2. Minimum Sizing Targets

To prevent the logo icon details from blurring or losing legibility, enforce these minimum display boundaries:
* **Web / Screen Display**: Minimum width of **120px** for the combined logotype, or **32x32px** for the standalone icon mark.
* **Print Applications**: Minimum width of **35mm** for the combined logotype.
* **Format Requirement**: Always render logos using vector SVG files to prevent pixelation on high-density screens.

---

## 3. Light Mode vs. Dark Mode Configurations

Our logo adapts dynamically to background contrast parameters:

### Dark Mode (Default / Preferred)
* **Background**: Deep Slate (`#0B0F19`) or Jet Black (`#000000`).
* **Icon Color**: Neon Violet gradient shifting to Indigo (`#8B5CF6` to `#6366F1`).
* **Text Color**: Pure White (`#FFFFFF`) with secondary text in Muted Gray (`#94A3B8`).

### Light Mode (Secondary)
* **Background**: Warm White (`#FAFAFA`) or Pure White (`#FFFFFF`).
* **Icon Color**: Deep Purple shifting to Dark Blue (`#6D28D9` to `#4338CA`).
* **Text Color**: Dark Charcoal (`#0F172A`) with secondary text in Slate Gray (`#475569`).

---

## 4. Incorrect Usage & Restrictions

To maintain brand integrity, avoid these common styling alterations:
* [ ] **No Squishing or Stretching**: Always lock aspect ratios during scaling operations.
* [ ] **No Low Contrast Backgrounds**: Do not place the light logo variant on light gray backgrounds, or the dark logo on dark blue backgrounds.
* [ ] **No Shadow Overlays**: Avoid adding drop shadows, glow filters, or 3D emboss layers to the flat vector elements.
* [ ] **No Color Customizations**: Never change the logo gradient colors to non-brand colors (e.g. green or orange).
* [ ] **No Alignment Modifications**: Do not shift the icon placement relative to the logotype text.

---

## 5. Professional Recommendations

1. **Verify Scaling**: Regularly test the logo assets scaled down to navigation headers sizes (e.g. height 24px) to ensure logotype text remains readable.
2. **Use standalone icon for small viewports**: On mobile screens (< 640px), replace the combined logotype with the standalone 32x32px icon mark.
3. **Keep SVGs Clean**: Ensure all exported SVGs are run through optimization tools (e.g. SVGO) to strip redundant XML metadata.
