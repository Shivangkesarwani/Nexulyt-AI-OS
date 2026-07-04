# Badges Guide

This document defines the visual style, color specifications, and usage guidelines for all GitHub repository badges (shields.io tags) representing the **Nexulyt-AI-OS** project.

---

## 1. Badge Style & Format Standards

We utilize **Shields.io** badges to present real-time metadata (e.g. build status, code coverage, licenses, release versions) on the root project page.

* **Preferred Style**: **Flat Square** (`style=flat-square`). This matches our geometric design principles. Avoid utilizing standard rounded or plastic badge styles.
* **Format**: Markdown badges linked to shields.io endpoints, wrapped in relative links pointing to target jobs.
  * **Correct**: `[![Build Status](https://img.shields.io/github/actions/workflow/status/.../ci.yml?style=flat-square)](file:///path/to/ci)`

---

## 2. Recommended Badges

Include these specific badges at the top of the root `README.md` file, organized in a single horizontal row:

* **Release Version**: Displays the latest release tag.
  * *Color*: Primary Indigo (`#6366F1` or label `blue`).
* **CI Build Status**: Displays real-time test run metrics.
  * *Color*: Emerald Green on success (`#10B981` or label `success`).
* **Code Coverage**: Displays percentage of tested files.
  * *Color*: Dynamic green-to-red range matching coverage numbers.
* **License**: Displays repository license (e.g., MIT).
  * *Color*: Neutral Slate (`#475569` or label `inactive`).
* **MCP Compliance**: Highlights Model Context Protocol compatibility.
  * *Color*: Neon Violet (`#8B5CF6`).

---

## 3. When Not to Use Badges

To prevent visual clutter on the README page:
* [ ] **No Personal Badges**: Do not include social profile follower badges, profile views counters, or custom user labels.
* [ ] **No Stale Metrics**: Avoid badges tracking deprecated packages, third-party rankings, or manual build dates.
* [ ] **No Secondary Pages Badges**: Do not place badges on secondary documentation or guidelines files; badges are reserved exclusively for the main repository landing page.
* [ ] **No Inline Badges**: Keep badges isolated to the top hero section; do not inject badges within body paragraph blocks.

---

## 4. Professional Recommendations

1. **Keep Badges Dynamic**: Ensure badges query active APIs (e.g., GitHub Actions workflow status) rather than using static hardcoded badge URLs.
2. **Align Colors**: Map shields.io custom hex parameters (`?color=6366f1`) to align badge colors with our brand palette.
3. **Verify Links**: Ensure clicking a badge routes the developer to the related page (e.g., the code coverage badge should link to the coverage report index).
