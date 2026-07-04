# Frontend Quality Checklist

This document provides a quality gate checklist to validate frontend components, Next.js configurations, Tailwind styling practices, React state patterns, search engine optimization (SEO), and Core Web Vitals compliance.

---

## 1. Overview

* **Objective**: Ensure frontend components are modular, type-safe, performant, responsive, search-optimized, and WCAG-compliant.
* **When to Use**: During active frontend component engineering, before submitting pull requests, or during performance audits.
* **Prerequisites**: Approved design specifications, typography scales, design token variables, and target performance metrics.

---

## 2. Checklist Items

### Component Design & Type Safety
* [ ] **TypeScript Typing**: Verify all props and data variables are explicitly typed (no usage of `any`).
* [ ] **State Separation**: Business logic and state tracking abstracted into custom hooks, keeping view components presentation-focused.
* [ ] **Props Verification**: Component inputs validate ranges and formats before rendering data.
* [ ] **Component Modularity**: Large components broken down into smaller, reusable blocks.

### Next.js & Server Boundaries
* [ ] **Server vs. Client Components**: Explicitly declare `'use client'` on components that manage interactive state or hooks.
* [ ] **Data Fetching optimization**: Use Server Components (RSC) to fetch data from APIs directly where possible.
* [ ] **Error Boundaries**: Wrap routes and dynamic components in error boundary containers.
* [ ] **Suspense and Loading**: Implement fallback loading skeleton states for asynchronous components.

### Tailwind Styling & Grid Reflows
* [ ] **Tailwind Prefixing**: Use responsive prefixes (`sm:`, `md:`, `lg:`) to reflow grid layouts from mobile up to desktop.
* [ ] **Theme Variable Conformance**: All color classes must map to tailwind config variables (avoid arbitrary values like `bg-[#ff0000]`).
* [ ] **Class Order Consistency**: Class layout ordering follows standard groupings (position, display, layout, typography, visual).

### SEO & Accessibility
* [ ] **Meta Elements**: Include descriptive `<title>` (under 60 chars) and meta description tags (under 160 chars) on all pages.
* [ ] **JSON-LD Schema**: Inject structured metadata schemas (Organization, Article, or Product) matching page types.
* [ ] **Alt Attributes**: Ensure all image elements feature descriptive alt tags.
* [ ] **Keyboard Nav Outlines**: Focus states display outline styles.

### Bundle Performance & Web Vitals
* [ ] **Image Optimization**: Use Next.js `<Image>` component to automate WebP conversions, responsive sizing, and layout shift placeholders.
* [ ] **Code Splitting**: Lazy-load large third-party widgets or dynamic routes using `next/dynamic` or `React.lazy`.
* [ ] **Lighthouse SLA Check**: Score > 95 on Core Web Vitals audits (LCP < 1.5s, CLS < 0.05, INP < 100ms).

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run build commands locally (`npm run build`) to ensure compilation compiles without warnings or errors.
* Audit LCP, INP, and CLS scores using Lighthouse or similar profiling tools.
* Verify accessibility using automated audits (e.g. axe-core).

### Exit Criteria
* **No Lint Errors**: ESLint and Prettier runs exit with zero violations.
* **Core Web Vitals Conform**: Lighthouse score targets achieved on all main templates.
* **Production Build Output**: Compiled frontend files package generated.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Overusing client-side state (`useState`) when server component caching could load data directly.
* Omitting height and width boundaries on media tags, causing Cumulative Layout Shift (CLS) errors during loading.
* Hardcoding styling colors instead of mapping them to Tailwind configuration themes, breaking dark mode support.

### Professional Recommendations
1. **Validate With Real Devices**: Do not trust browser viewport simulators alone; test responsive layouts on actual mobile devices.
2. **Abstract Hook Utilities**: Group common logic (e.g., event listeners, screen dimensions checks) into shared custom hooks folders.
3. **Monitor Bundle Bloat**: Regularly review build configurations using analyzers (e.g. `@next/bundle-analyzer`) to flag oversized packages.
