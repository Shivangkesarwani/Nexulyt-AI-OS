# Frontend Engineer Review & QA Checklist

Use this checklist to perform a comprehensive code and performance audit of frontend applications before staging commits.

---

## 1. REQUIREMENT REVIEW
- [ ] **Functional Alignment:** The component logic perfectly matches the architectural boundaries specified by the Software Architect.
- [ ] **UX Mockup Fidelity:** Spacing, layouts, and page transitions match the UI/UX Designer specifications.
- [ ] **Data Contracts:** Fetch models match the JSON schemas and API parameters defined by backend endpoints.
- [ ] **Scope Boundaries:** Presentational components contain zero custom database queries or business operations logic.

---

## 2. PROJECT SETUP
- [ ] **TypeScript Settings:** The `tsconfig.json` runs in strict mode (`strict: true`) with no implicit `any` compiler warnings.
- [ ] **Dependencies Pinning:** All external library packages are pinned to exact semantic versions in `package.json` lockfiles.
- [ ] **Linter configurations:** ESLint rules are configured to check import formatting and sorting rules automatically.
- [ ] **Environment templates:** A valid template env configuration file (`.env.example`) is documented in the project root.

---

## 3. FOLDER STRUCTURE
- [ ] **Root Separation:** Global presentational components are organized inside `components/ui`, while page structures stay inside `app/`.
- [ ] **Domain Grouping:** Domain-specific hooks, utilities, and widgets are grouped together inside feature folders.
- [ ] **Consistent Pathing:** Path aliases (e.g., `@/*`) are configured to keep relative import statements clean.

---

## 4. COMPONENTS
- [ ] **RSC Strategy:** React Server Components are used by default for static content; Client Components are used only for interactive states.
- [ ] **Props Contracts:** All React components define explicit TypeScript interfaces for their props.
- [ ] **List Key Stability:** List items use unique data keys (`item.id`) rather than array index positions.
- [ ] **Single Responsibility:** Components focus on a single task, and state logic is isolated using custom hooks.

---

## 5. STYLING
- [ ] **Token Alignment:** Colors, sizing margins, and padding values align to CSS variables defined in styling stylesheets.
- [ ] **Relative Sizing:** Component widths and heights use relative units (`%`, `rem`, `vh`, `vw`) rather than hardcoded pixel dimensions.
- [ ] **Hover Interactions:** Clickable elements have smooth, active mouse-hover background animations.

---

## 6. TAILWIND
- [ ] **Consistent Spacing:** Tailwind spacing classes follow the 8pt grid scale (e.g., `p-2`, `m-4`, `gap-8`).
- [ ] **Hover States:** Hover states use Tailwind dynamic overrides (e.g., `hover:bg-neutral-800`).
- [ ] **Purging Config:** The tailwind config file matches target directories, ensuring unused classes are purged at build time.

---

## 7. RESPONSIVE DESIGN
- [ ] **Container Queries:** CSS container queries (`@container`) are used to let components adapt directly to their parent container's width.
- [ ] **No Scroll Overflows:** Element widths fit cleanly, preventing horizontal scrollbars on smaller viewports.
- [ ] **Breakpoint Adaptability:** Elements wrap, grids collapse, and layouts adapt smoothly between viewports.

---

## 8. MOBILE FIRST
- [ ] **Mobile Defaults:** Base Tailwind utility layouts are written for mobile viewports, using min-width queries for desktop overrides.
- [ ] **Touch Target Size:** Action buttons, links, and dropdown triggers are at least $48 \times 48\text{px}$ in size.
- [ ] **Keyboard Optimization:** Forms configure correct keyboard types (e.g., `type="email"`, `type="number"`) on mobile.

---

## 9. ACCESSIBILITY (WCAG 2.2)
- [ ] **Color Contrast:** All text-to-background combinations meet WCAG 2.2 AA contrast requirements.
- [ ] **Focus Styles:** Interactive components have clear, visible `:focus-visible` styles.
- [ ] **ARIA labels:** Screen-reader labels are provided for all standalone icon buttons.
- [ ] **Keyboard Traps:** Keyboard tab focus moves in a logical order and does not get trapped inside modals.
- [ ] **Alt text:** Alternative descriptors are defined for all contextual images.

---

## 10. FORMS
- [ ] **Resolver Integration:** Forms use React Hook Form paired with Zod validation.
- [ ] **Accessibility:** Form fields are paired with HTML `<label>` tags.
- [ ] **Submit Protections:** Submit buttons are disabled during active loading states to prevent duplicate submissions.

---

## 11. VALIDATION
- [ ] **Strict Schemas:** All user inputs are validated against strict Zod schemas.
- [ ] **Error Placements:** Error messages are displayed immediately below the invalid input field.
- [ ] **Real-time Checks:** Form fields validate inputs in real-time as users fill out forms.

---

## 12. STATE MANAGEMENT
- [ ] **Local State Proximity:** State is kept as close to where it is used as possible.
- [ ] **Zustand Cleanliness:** Global client state stores are lightweight and clear state on unmount.
- [ ] **Shared Context:** React Context is used only for global themes or authentication states.

---

## 13. API INTEGRATION
- [ ] **Query caching:** Asynchronous API fetches are managed using TanStack Query custom hooks.
- [ ] **Payload Sanitization:** API data payloads are validated against Zod schemas upon retrieval.
- [ ] **Timeout Handling:** Network requests configure timeouts and retry backoff limits.

---

## 14. ERROR HANDLING
- [ ] **Error boundaries:** Dynamic widgets and components are wrapped in React Error Boundaries.
- [ ] **Safe Fallbacks:** Error states display user-friendly troubleshooting messages instead of raw stack traces.
- [ ] **Retry Triggers:** Failed API requests include a manual retry button.

---

## 15. LOADING STATES
- [ ] **Interactive spinners:** Button submissions display a spinner and enter a disabled state.
- [ ] **Skeleton loaders:** Page route loading states render skeleton screens matching the content layout.
- [ ] **Visual Updates:** Page loaders are delayed for 300ms to prevent visual flickering on fast queries.

---

## 16. EMPTY STATES
- [ ] **Actionable Help:** Empty states explain why data is missing and include a primary CTA button.
- [ ] **Context alignment:** Empty state illustrations match the application theme.

---

## 17. ANIMATIONS
- [ ] **Functional motion:** Motion transitions guide attention; decorative animations that delay workflows are removed.
- [ ] **GSAP timelines:** GSAP timelines clean up and kill active processes on component unmount.
- [ ] **Spring physics:** Framer Motion springs use natural physics curves (`stiffness`, `damping`) for UI cards.
- [ ] **GPU Acceleration:** Animations are restricted to CSS `transform` and `opacity` properties.

---

## 18. PERFORMANCE
- [ ] **Code Splitting:** Dynamic page segments use code splitting (`next/dynamic` or `React.lazy`).
- [ ] **Image optimization:** Images use the Next.js Image component with WebP/AVIF formats.
- [ ] **Asset caching:** Edge servers cache static page assets.

---

## 19. SEO
- [ ] **Single H1 Tag:** Each page has exactly one `<h1>` tag and follows a logical heading hierarchy.
- [ ] **Anchor indexability:** Navigation menus use standard anchor links (`<a>`).
- [ ] **Metadata configuration:** Document layouts define page titles, descriptions, and OpenGraph tags.

---

## 20. CORE WEB VITALS
- [ ] **LCP Target:** Largest Contentful Paint (LCP) compiles under $1.2\text{s}$.
- [ ] **CLS Target:** Cumulative Layout Shift (CLS) stays under $0.05$.
- [ ] **INP Target:** Interaction to Next Paint (INP) stays under $40\text{ms}$.

---

## 21. TESTING
- [ ] **Unit Tests:** Critical business logic and hooks pass Vitest unit tests.
- [ ] **E2E Tests:** Primary user journeys pass Playwright automated browser tests.
- [ ] **Coverage Limits:** Coverage checks block commits if coverage falls below 80%.

---

## 22. DEPLOYMENT
- [ ] **Automatic builds:** CI pipelines build, lint, and test code before staging deployments.
- [ ] **Staging URLs:** Staging builds compile to preview URLs for verification.
- [ ] **Secrets security:** API keys and environment variables are active on production hosts.

---

## 23. CODE QUALITY
- [ ] **No Dead Code:** console logs, debuggers, and unused imports are removed from production branches.
- [ ] **TypeScript Safety:** No implicit `any` type definitions or disabled type checks are present.
- [ ] **Readable logic:** Variable and function names explain their purpose clearly.

---

## 24. FINAL REVIEW
- [ ] **Cross-browser:** The application is tested on Chrome, Safari, Firefox, and Edge.
- [ ] **Responsive flow:** Grids and text scale correctly on page zoom levels up to 200%.
- [ ] **QA Sign-off:** Skeletons, spinners, empty views, error alerts, and success messages are verified.
