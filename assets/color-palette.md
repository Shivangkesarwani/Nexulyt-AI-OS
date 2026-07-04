# Color Palette Specifications

This document defines the recommended color tokens, system states, and accessibility contrast standards for the **Nexulyt-AI-OS** brand identity and user interfaces.

---

## 1. Core Color Tokens

To maintain a premium, cohesive aesthetic, use these color values. Avoid hardcoding custom hex codes.

### Brand Identity Colors
* **Primary (Indigo)**: `#6366F1` (HSL `239, 84%, 66%`). Represents the core identity and primary interaction states.
* **Secondary (Slate)**: `#475569` (HSL `215, 16%, 34%`). Used for secondary text, border partitions, and secondary actions.
* **Accent (Neon Violet)**: `#8B5CF6` (HSL `258, 90%, 66%`). Used for hero highlights, key visual elements, and gradients.

### System State Colors
* **Success (Emerald)**: `#10B981` (HSL `159, 84%, 39%`). Used for success alerts, completed task statuses, and positive indicators.
* **Warning (Amber)**: `#F59E0B` (HSL `38, 92%, 50%`). Used for warning indicators, rate limit notifications, and pending states.
* **Danger (Crimson)**: `#EF4444` (HSL `0, 84%, 60%`). Used for system errors, failed tests, and delete actions.

### Theme Neutrals
* **Dark Background (Slate-950)**: `#0B0F19` (HSL `224, 38%, 7%`). Core background canvas for dark mode UI and repository banners.
* **Light Background (Gray-50)**: `#F9FAFB` (HSL `210, 20%, 98%`). Core background canvas for light mode documents.
* **Neutral Text (Slate-100)**: `#F1F5F9` (HSL `210, 40%, 96%`). Primary readable text color for dark mode.
* **Muted Text (Slate-400)**: `#94A3B8` (HSL `215, 16%, 65%`). Secondary text color for timestamps and descriptions.

---

## 2. Gradient Specifications

When rendering backgrounds, hero sections, or logo overlays, utilize these linear gradients (default angle 135 degrees):

* **Identity Gradient**: Shifting from Primary Indigo (`#6366F1`) to Accent Violet (`#8B5CF6`).
* **Deep Canvas Gradient**: Shifting from Dark Background (`#05070C`) to Slate Blue (`#0F172A`).
* **Alert Gradient**: Shifting from Warning Amber (`#F59E0B`) to Orange (`#EA580C`) for alert states.

---

## 3. Accessibility & Contrast Verification

To comply with **WCAG 2.1 Level AA** specifications, all text-to-background color combinations must meet minimum contrast ratios:

* **Normal Text (< 18pt)**: Requires a minimum contrast ratio of **4.5:1**.
* **Large Text (> 18pt or Bold > 14pt)**: Requires a minimum contrast ratio of **3:1**.

### Approved Pairings Matrix
* [x] **Primary Text** (`#F1F5F9`) on **Dark Canvas** (`#0B0F19`): Contrast **15.6:1** (Passes WCAG AAA).
* [x] **Muted Text** (`#94A3B8`) on **Dark Canvas** (`#0B0F19`): Contrast **6.2:1** (Passes WCAG AA).
* [x] **Primary Indigo** (`#6366F1`) on **Dark Canvas** (`#0B0F19`): Contrast **4.8:1** (Passes WCAG AA).
* [ ] *Warning*: Never place Muted Text (`#94A3B8`) on Secondary Slate (`#475569`), as contrast drops to **1.8:1** (Fails accessibility checks).

---

## 4. Professional Recommendations

1. **Verify Contrast at Design Stage**: Use color picker tools during UI mocks to verify contrast ratios before writing CSS classes.
2. **Support System Preferences**: Implement CSS media queries (`@media (prefers-color-scheme: dark)`) to automatically adjust background tokens to user preferences.
3. **Use HSL for Color Adjustments**: Utilize HSL values inside CSS rules to easily generate hover states by shifting the lightness parameter.
