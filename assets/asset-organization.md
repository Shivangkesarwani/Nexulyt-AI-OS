# Asset Organization Guide

This document defines the storage directories, file naming standards, Git Large File Storage (LFS) rules, compression policies, and image optimization pipelines for all graphical assets in the **Nexulyt-AI-OS** repository.

---

## 1. Directory Structure

Store all visual assets inside the root `/assets` folder, organized by category to prevent directory clutter:

```
assets/
├── README.md
├── logo/                   # Multi-resolution SVG and PNG logos
├── banners/                # Repository hero banners and social previews
├── screenshots/            # Product user interface captures
├── icons/                  # SVG and system specific icon arrays
└── illustrations/          # Technical vector illustration drawings
```

* **Do Not Store** custom binary assets directly in the root of the `/assets` folder.
* **Temporary Scratch Files** (e.g. raw mockup files, photoshop layers) must be placed in `.gitignore` paths or external shared drives.

---

## 2. File Naming Conventions

* **Kebab-Case Naming**: Always use lowercase kebab-case naming structures for files (e.g. `logo-light-mode.svg`, `dashboard-desktop-view.png`).
* **Viewport Indicators**: Screenshots must include screen dimensions tags: `[feature]-[viewport].png` (e.g. `checkout-mobile.png`, `settings-desktop.png`).
* **No Spacing or Capitalization**: Never use spaces or uppercase letters in file names.

---

## 3. Git LFS (Large File Storage) Configurations

To prevent binary image files from bloating our primary git history repository, track large assets using Git LFS.

### LFS Tracking Parameters
The following patterns are configured in the root `.gitattributes` file:
```gitattributes
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.ico filter=lfs diff=lfs merge=lfs -text
```

---

## 4. Optimization & Compression Policies

* **Vector SVGs Optimization**: Run all exported SVGs through optimization tools (e.g. SVGO) to strip inline metadata and reduce file sizes by up to 50%.
* **Raster PNGs Compression**: Compress screenshots using lossless compression tools (e.g. OptiPNG, Tinypng) to keep file sizes under **250KB**.
* **Dimensions Matching**: Crop raster images strictly to their display container sizes, avoiding oversized image uploads.

---

## 5. Professional Recommendations

1. **Keep Source Files Separate**: Do not check heavy raw design files (e.g. `.fig`, `.psd`, `.ai`) into git version control. Link to shared folders in design specs instead.
2. **Automate Image Compressions**: Use pre-commit hooks or automated pipeline workflows to verify that added images have been compressed.
3. **Verify Asset Resolvability**: Double-check relative markdown paths when moving files to prevent broken links in documentation.
