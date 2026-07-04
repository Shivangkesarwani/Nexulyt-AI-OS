# Frontend Engineer AI Skill

A production-grade AI Skill designed to teach AI assistants to build modern, performant, and accessible frontend interfaces that align with Staff/Principal-level Frontend Architects.

---

## 1. Overview
The Frontend Engineer skill defines the environment configurations, rendering protocols, caching state stores, and layout queries required to implement user interfaces. It prevents AI from producing placeholder-ridden code, inline styling hacks, or inaccessible DOM structures.

---

## 2. Purpose
- **Production-Ready Code:** Deliver fully functional, TypeScript-validated, and structured components.
- **Performance Excellence:** Enforce page rendering pipelines and asset compressions to satisfy Core Web Vitals targets.
- **Accessibility Integration:** Enforce semantic tags, keyboard tab navigations, and contrast ratios matching WCAG 2.2 AA.

---

## 3. Responsibilities
- Implement visual mockups created by the UI/UX Designer skill.
- Fetch, validate (via Zod), and display database payloads.
- Optimize JavaScript bundle sizes, initial loads (LCP), and page responsiveness (INP).
- Verify code health using Vitest component tests and Playwright E2E browser scripts.

---

## 4. Features
- **App Router Integration:** Deep layout structures, dynamic route configs, and server-side data fetching.
- **State stores & caching:** Lightweight client Zustand stores paired with TanStack Query remote caches.
- **Hardware-Accelerated Motion:** Custom GSAP timelines, Framer Motion springs, and React Three Fiber scenes.
- **Self Review Engine:** An automated design and code audit loop verifying code simplicity, types, and usability.

---

## 5. Supported Technologies
- Next.js (App Router)
- React
- Tailwind CSS
- TypeScript

---

## 6. Tech Stack
- **Core:** HTML5, CSS3, JavaScript, TypeScript
- **Framework:** Next.js (App Router), React
- **Styling:** Tailwind CSS, Shadcn UI
- **Animations:** GSAP, Framer Motion, Lenis Scroll
- **3D Graphics:** React Three Fiber, Drei
- **Forms & Validation:** React Hook Form, Zod
- **State & Caching:** Zustand, TanStack Query
- **Testing & Deployment:** Vitest, Playwright, Vercel

---

## 7. Folder Structure
```
skills/frontend-engineer/
├── SKILL.md      # Core AI Identity, principles, and frontend engineering rules
├── CHECKLIST.md  # Professional QA audit checkboxes for interface reviews
└── EXAMPLES.md   # Step-by-step code and setup examples
```

---

## 8. Workflow
1. **Analyze Specifications:** Deconstruct UI mockups, database schemas, and API contracts.
2. **Setup Types & Variables:** Configure Zod schemas, TypeScript interfaces, and environment variables.
3. **Build Components:** Code Atomic atoms, Molecules, and state stores.
4. **Audit & Deploy:** Run Vitest suites, check Core Web Vitals, and deploy to staging.

---

## 9. Expected Outputs
When activated, this skill generates:
- Modular, TypeScript-typed React components.
- Clean Tailwind styles using semantic design tokens.
- Fully configured Next.js page routing directories.
- Automated testing suites (Vitest / Playwright).

---

## 10. Compatible Skills
- **Software Architect:** Designs the data schemas, APIs, and overall system boundaries.
- **UI/UX Designer:** Designs visual styles, color theories, and page layouts.

---

## 11. When to Use
Use this skill when building new frontend pages, integrating APIs, optimizing page load speeds, or updating design tokens.

---

## 12. Best Practices
- **Strict TypeScript:** Enforce zero `any` type definitions.
- **Tree-Shaking imports:** Import specific modules to keep bundles optimized.
- **Semantic Tags:** Use HTML5 semantic elements (e.g., `<article>`, `<main>`) for all page structures.

---

## 13. Example User Requests
- *Request 1:* "Build a dynamic SaaS settings page with password reset forms validated by Zod."
- *Request 2:* "Design a scroll-linked hero animation using GSAP ScrollTrigger."
- *Request 3:* "Create a responsive data grid displaying invoice payments with pagination."

---

## 14. Repository Standards
All frontend code must satisfy the following constraints before PR review:
- Initial JS bundle size: $\le 100\text{KB}$ gzipped.
- Largest Contentful Paint (LCP): $\le 1.2\text{s}$.
- Cumulative Layout Shift (CLS): $\le 0.05$.
- Test coverage: $\ge 80\%$.

---

## 15. License
Licensed under the [MIT License](file:///d:/projects/Nexulyt-AI-OS/LICENSE).
