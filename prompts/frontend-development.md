# Frontend Development Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Lead Frontend Engineer**. It aligns all frontend implementations with the [Coding Standards](file:///d:/projects/Nexulyt-AI-OS/standards/coding-standards.md), [Performance Standards](file:///d:/projects/Nexulyt-AI-OS/standards/performance-standards.md), and [Accessibility Standards](file:///d:/projects/Nexulyt-AI-OS/standards/accessibility-standards.md).

---

## 1. Overview

* **Purpose**: Enforce component modularity, type safety, optimal rendering strategies, responsive layouts, smooth animations, and search engine optimization.
* **When to Use**: When creating new React components, restructuring page routes in Next.js, applying Tailwind CSS design systems, writing responsive layouts, or optimizing Web Vitals.
* **Inputs**: Component specifications, designs, performance data, state requirements, and target viewport boundaries.
* **Expected Outputs**: React/TypeScript component structures, hooks abstractions, Next.js file layout plans, Tailwind style sheets, and SEO meta structure schemas.
* **Best Practices**:
  - Keep components modular, typing all component props explicitly.
  - Separate state management and API calls from visual rendering components using custom hooks.
  - Implement mobile-first layouts using Tailwind's prefix utilities (`md:`, `lg:`).
* **Common Mistakes**:
  - Over-using client side state (`useState`) where React Server Components could load data directly.
  - Hardcoding CSS pixel sizes instead of using styling system utility tokens (rem, em, percentages).

---

## 2. Prompt Templates

### React Component Architecture Prompt
```text
Role: You are a Senior React Developer.
Context: You are designing a custom component library or refactoring a modular React module for: [Insert component goal, e.g., Filterable Product Grid].
Goal: Plan the component architecture and custom hook abstraction.
Inputs:
- State Requirements: [Insert e.g., multiple filter arrays, sorting order, pagination offset]
- External APIs: [Insert e.g., product search endpoint]
Expected Output Structure:
1. Architecture Schema: Visual representation showing component hierarchies (parent, child components) and state scope (where state is held).
2. Prop Types & Interface Definitions: Define TypeScript interfaces for all props and data models.
3. Custom Hook Blueprint: Define the custom hook signature (`useProductFilter`) listing parameters, return properties, and internal state.
Constraint: Focus on architecture, type safety, and hooks. Do not generate full implementation code.
Cross-Reference: Align styles with:
[Coding Standards](file:///d:/projects/Nexulyt-AI-OS/standards/coding-standards.md)
```

### Next.js Architecture Prompt
```text
Role: You are a Next.js Architect.
Context: You are planning a new route and data fetching strategy in a Next.js application for: [Insert page feature, e.g., User Dashboard with order history].
Goal: Determine the folder routing structure and server/client component division.
Inputs:
- Next.js Version: [Insert e.g., Next.js 14 App Router]
- Data Dynamism: [Insert e.g., user profile changes rarely, order history updates frequently]
Expected Output Structure:
1. Routing Map: Define the folder structure under `app/` directory (e.g., nested folders, dynamic routes `[id]/page.tsx`).
2. Server vs. Client Boundary Plan: Tabulate each sub-component indicating whether it is a Server Component (RSC) or a Client Component ('use client') with technical justification.
3. Data Fetching Strategy: Define which hooks (`fetch` cache controls, Suspense boundary fallback states) should be utilized.
```

### Tailwind CSS Styling Spec Prompt
```text
Role: You are a UI Styling Engineer.
Context: You are mapping designs into clean, responsive Tailwind classes for: [Insert UI layout, e.g., dynamic sidebar nav].
Goal: Define the styling rules, layout grids, and interactive states using Tailwind utility naming.
Inputs:
- Spacing Rules: [Insert e.g., 24px container padding, 16px gap between items]
- Themes: [Insert e.g., dark mode compatibility required]
Expected Output Structure:
1. Grid & Flex Setup: Define the classes applied to parent layouts (e.g., `flex flex-col md:flex-row gap-4`).
2. Interactive State Classes: Define hover, focus, active, and disabled classes (e.g., `hover:bg-primary-600 focus:ring-2 disabled:opacity-50`).
3. Dark Mode Classes: Map the dark variants (e.g., `bg-white dark:bg-slate-900 text-slate-800 dark:text-slate-100`).
```

### Component Design Prompt
```text
Role: You are a Component Design Engineer.
Context: You are defining a reusable design component interface: [Insert component name, e.g., Button or Modal].
Goal: Specify state boundaries, styling variants, and accessibility parameters.
Inputs:
- Component Purpose: [Insert e.g., trigger actions and load states]
- Variants: [Insert e.g., Primary, Secondary, Outline, Danger]
Expected Output Structure:
1. Props Interface: TS interface for variant, size, loadingState, disabledState, and icon hooks.
2. Variant Class Mapping: Detail which Tailwind style tokens map to each visual variant.
3. Focus & WAI-ARIA states: Detail default state properties (e.g., `aria-busy`, `tabIndex`).
```

### Responsive Design Prompt
```text
Role: You are a Responsive Layout Specialist.
Context: You are planning a complex page structure to display cleanly across mobile, tablet, and desktop viewports: [Insert page details].
Goal: Map the fluid grid reflow positions.
Inputs:
- Mobile Layout: [Insert e.g., single-column stack]
- Desktop Layout: [Insert e.g., 3-column dashboard]
Expected Output Structure:
1. Breakpoint Grid Matrix: Table showing component positioning across screen widths (`sm`, `md`, `lg`, `xl`).
2. Column-Span & Order Plan: Specify when layouts should use `order-first` or grid offsets.
3. Fluid Typography Strategy: Specify text scaling parameters (e.g., `text-sm md:text-base lg:text-lg`).
```

### Animations Spec Prompt
```text
Role: You are a Motion Designer.
Context: You are designing micro-animations and page transitions for: [Insert component transition, e.g., slide-out menu drawer].
Goal: Define transitions, timings, and performance limits.
Inputs:
- Tech Choice: [Insert e.g., Framer Motion / CSS Transitions]
- Target Feel: [Insert e.g., snappy, springy, smooth]
Expected Output Structure:
1. State Animations: Define properties for Initial, Animate, Exit, and Hover states.
2. Bezier Timings & Springs: Specify durations (ms) or spring parameters (stiffness, damping).
3. Performance Constraints: Outline rules to prevent layout thrashing (e.g., animate transform/opacity only, avoid animating layout-inducing properties).
```

### Frontend SEO Prompt
```text
Role: You are an SEO Consultant.
Context: You are optimizing a marketing or content page for search visibility: [Insert page URL structure or contents].
Goal: Define the semantic layout structure and meta specifications.
Inputs:
- Target Keyword: [Insert keywords]
- Page Type: [Insert e.g., product page, blog post]
Expected Output Structure:
1. Meta-Tags Document: Output title tags (limit 60 chars), description (limit 160 chars), and OpenGraph (OG) properties.
2. Semantic HTML Hierarchy: Define document structure showing single `<h1>`, sub-headings `<h2>`/`<h3>`, and figure tag arrangements.
3. Structured Schema (JSON-LD): Write the draft schema payload model (e.g., Organization, Product, Article).
```

### Frontend Performance Audit Prompt
```text
Role: You are a Web Vitals Expert.
Context: You are investigating slow loading times on a web page: [Insert page load characteristics].
Goal: Formulate a bundle splitting and optimization roadmap.
Inputs:
- Core Web Vitals Issues: [Insert e.g., LCP is 4.2s, CLS is 0.15]
- Bundle Characteristics: [Insert e.g., large third-party widgets, non-optimized images]
Expected Output Structure:
1. Code Splitting Map: Detail which packages/routes should use lazy loading (`React.lazy` or `dynamic()` imports).
2. Asset Optimization Plan: Specify image dimensions, compression formats (WebP/AVIF), and preloading strategies.
3. Render Blocking Reduction: Identify scripts to defer, async, or run in web workers.
Cross-Reference: Align with performance targets in:
[Performance Standards](file:///d:/projects/Nexulyt-AI-OS/standards/performance-standards.md)
```

---

## 3. Professional Recommendations

1. **Strict Type Contracts**: Ensure all component templates define standard Types/Interfaces. Avoid utilizing `any`.
2. **WCAG Compliance**: Verify that interactive components support keyboard focus controls and correct focus outlines.
3. **Optimistic UI Updates**: For client interactions, write prompts that design optimistic state flows to improve perceived latency.
