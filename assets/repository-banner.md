# Repository Banner Specifications

This document defines the layout rules, pixel dimensions, typography, and visual hierarchy for the **Nexulyt-AI-OS** GitHub repository hero banner.

---

## 1. Banner Dimensions & Resolution

* **Canvas Dimension**: **1280x640 pixels** (2:1 aspect ratio) or **1920x960 pixels** for high-density Retina display optimizations.
* **Format**: Vector SVG is preferred for crisp rendering of shapes and typography. If raster images are exported, use PNG format compressed via lossy-free tools to keep sizes under **250KB**.

---

## 2. Layout Grid & Structure

The repository banner uses a **three-column layout grid** to balance whitespace and organize content:

```
+-------------------------------------------------------------------------+
|                                                                         |
|  [ LEFT COLUMN - 15% ]   [ CENTER COLUMN - 70% ]  [ RIGHT COLUMN - 15% ]|
|  Safe whitespace         Hero branding content    Safe whitespace       |
|                          - System logo icon                             |
|                          - Logotype Title                               |
|                          - Value description                            |
|                          - Key module highlights                        |
|                                                                         |
+-------------------------------------------------------------------------+
```

* **Left/Right Columns (15% each)**: Kept clear of text and primary icon elements to serve as whitespace framing, preventing layout clutter when previewed inside cards.
* **Center Column (70%)**: Houses the main visual elements, centered both horizontally and vertically.

---

## 3. Hero Content & Visual Hierarchy

### System Logo Icon
* Positioned at the top center of the hero section.
* Sized at **128x128 pixels**.
* Renders using the signature neon gradient.

### Logotype Title
* Placed directly below the logo icon with a **24px** vertical spacer.
* Typography: **Outfit Bold** or **Inter Semi-Bold** at **48px** size.
* Color: Pure White (`#FFFFFF`).

### Value Description
* Placed below the title with a **16px** spacer.
* Sized at **20px** with a line height of 1.4.
* Text: `"The Agentic OS for Software Engineering Workflows"`
* Color: Muted Slate Gray (`#94A3B8`).

### Highlight Badges
* A horizontal row of three minimalist tag boxes centered below the description.
* Badges: `[Autonomous Agents]  [Standardized Workflows]  [Model Context Protocol (MCP)]`
* Styling: Thin 1px slate border with a background color matching the dark canvas theme.

---

## 4. Typography & Color Specifications

### Fonts
* **Primary Headers**: Inter or Outfit (Sans-Serif) for modern readability.
* **System Accents**: JetBrains Mono (Monospaced) for tag badges text to reflect developer focus.

### Colors Palette
* **Background Canvas**: Deep Charcoal to Slate Blue linear gradient (`#05070C` to `#0F172A`), shifting from bottom-left to top-right.
* **Primary Gradient Accent**: Indigo (`#6366F1`) to Violet (`#8B5CF6`).
* **Secondary Border Details**: Slate Gray (`#334155`).

---

## 5. Professional Recommendations

1. **Verify Scaling**: Downscale the banner to 640x320px in testing to verify description text remains readable.
2. **Prioritize SVG**: Utilize vector elements rather than heavy background raster images to keep banner crisp and clean.
3. **Keep it Static**: Do not include animated elements inside the primary repository banner to maintain platform compatibility.
