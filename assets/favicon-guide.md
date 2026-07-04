# Favicon Guide

This document defines the resolution specifications, formats, file configurations, and web manifest patterns for the **Nexulyt-AI-OS** project favicon.

---

## 1. Favicon File Formats & Dimensions

To support multiple device configurations, operating systems, and browser viewport boundaries, provide the favicon in these specific formats:

* **Classic Favicon (ICO)**: Sized at **16x16, 32x32, and 48x48 pixels** packaged inside a single multi-resolution `favicon.ico` file placed in the public root. This ensures compatibility with legacy browsers.
* **Modern Web Icons (PNG)**: Sized at **32x32 pixels** (`favicon-32x32.png`) and **16x16 pixels** (`favicon-16x16.png`) to support modern browser tab bars.
* **Apple Touch Icon (PNG)**: Sized at **180x180 pixels** (`apple-touch-icon.png`) for iOS home screen shortcuts. Enforce square layout parameters without rounded corners (iOS applies the mask automatically).
* **Android / Chrome Icon (PNG)**: Sized at **192x192 and 512x512 pixels** (`android-chrome-192x192.png`) for Android shortcuts.
* **Vector Favicon (SVG)**: Sized at **scalable vector dimensions** (`favicon.svg`) with `type="image/svg+xml"`. This is the preferred format for modern browsers.

---

## 2. HTML Meta Integration & Web Manifest

To configure the browser to load these files correctly, include the following tags in the main HTML `<head>`:

```html
<link rel="icon" type="image/x-icon" href="/favicon.ico">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/svg+xml" href="/favicon.svg">
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="manifest" href="/site.webmanifest">
```

### Web Manifest Schema (`site.webmanifest`)
Enforce standard JSON properties to configure shortcuts behaviors:
```json
{
  "name": "Nexulyt-AI-OS",
  "short_name": "Nexulyt",
  "icons": [
    {
      "src": "/android-chrome-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/android-chrome-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "theme_color": "#0b0f19",
  "background_color": "#0b0f19",
  "display": "standalone"
}
```

---

## 3. Design & Spacing Rules

* **Geometry simplification**: Reduce the complex logo mark to its primary geometric icon shape. Avoid using small text lines inside favicons, as they blur at 16x16px.
* **Safe Margins**: Enforce a **10% padding margin** inside PNG assets to prevent details from clipping against container edges.
* **Alpha Transparency**: Keep background pixels transparent on PNG/SVG files to ensure the mark renders cleanly on customized browser tab themes.

---

## 4. Professional Recommendations

1. **Test Legibility**: View the 16x16px favicon tab icon on dark, light, and private browser tabs to verify visual contrast.
2. **Optimize SVG Code**: Compress the vector SVG favicon to strip inline metadata, keeping file sizes under **5KB**.
3. **Use PWA Manifest Validation**: Run web configuration checkers (e.g. Lighthouse PWA audits) to verify manifest fields are correct.
