# UI/UX Designer Examples

This document provides detailed visual and structural examples demonstrating how to apply the UI/UX Designer skill to build premium interfaces.

---

## 1. Portfolio Website

### User Request
Create a portfolio website for a Senior Creative Developer showcasing hardware-accelerated WebGL visuals and interactive case studies.

### Design Thinking
- **Aesthetic:** Framer-inspired motion physics, dark mode, high-quality images, and generous whitespace.
- **Narrative Flow:** Guide visitors from a clean introduction directly into high-fidelity visual case studies.

### Layout Planning
- **Layout:** Standard single-column hero section followed by an asymmetrical 2-column project grid.
- **Section Breaks:** Generous margins (`margin-bottom: 96px`) to give projects breathing room.

### Typography
- **Heading Font:** *Outfit* (bold sans-serif) - Major Third scale.
- **Body Font:** *Inter* (lightweight, regular sans-serif) - line-height: 1.6.

### Color Palette
- Canvas (60%): `hsl(0, 0%, 4%)` (Deep near-black)
- Cards & Borders (30%): `hsl(0, 0%, 10%)` with `border: 1px solid rgba(255,255,255,0.08)`
- Accent (10%): `hsl(280, 75%, 65%)` (Warm purple neon)

### Components
- Custom navigation header with a sliding pill-shaped background indicator.
- Grid cards that scale slightly (`scale(1.02)`) and show high-fidelity screenshot previews on hover.

### UX Decisions
- Case study sections include clear, action-oriented links detailing the exact technologies used and metrics achieved.
- All image mouse-scovers utilize GSAP FLIP transformations for fluid expansion.

### Mobile Strategy
- Shift the asymmetrical desktop grid into a clean, single-column scroll layout.
- Expand touch target areas on mobile buttons to $48 \times 48\text{px}$.

### Accessibility
- Set image contrast values high and include descriptive `alt` tags.
- Provide a clear, high-contrast `:focus-visible` outline ring around all project links.

### Final Recommendation
Deliver as a statically generated page (SSG) with client-side GSAP transitions to maximize both load speed and interaction polish.

---

## 2. SaaS Dashboard

### User Request
Design a financial analytics dashboard tracking real-time ledger payments and conversion trends.

### Design Thinking
- **Aesthetic:** Vercel-inspired monochromatic grid system with high information density.
- **Goal:** Minimize visual noise to let financial metrics remain the focal point.

### Layout Planning
- **Shell:** Sidebar navigation (sticky 240px width) with a top global search bar and a main 3-column dashboard grid.
- **Spacing:** Strict 8pt spacing system (16px padding on metrics cards).

### Typography
- **Heading Font:** *SF Pro* (sans-serif) - Compact spacing scale.
- **Body Font:** *JetBrains Mono* (monospaced) for metric values to align numbers cleanly.

### Color Palette
- Canvas (60%): `hsl(0, 0%, 98%)` (Soft white)
- Cards & Borders (30%): `hsl(0, 0%, 100%)` with `border: 1px solid hsl(0, 0%, 92%)`
- Accent (10%): `hsl(142, 76%, 36%)` (Emerald green for positive trends)

### Components
- Compact key metrics cards with micro line charts showing 7-day trend profiles.
- Paginated data table featuring left-aligned names, right-aligned values, and hover highlights.

### UX Decisions
- Avoid displaying heavy, complex charts on initial load. Use progressive disclosure tooltips to show exact details on hover.
- Keep primary transaction filters (date, status) pinned at the top of the view.

### Mobile Strategy
- Collapses the navigation sidebar into a bottom navigation tab bar.
- Defer secondary table columns dynamically using CSS container queries.

### Accessibility
- Ensure trend color indicators are supplemented by text labels (e.g., "+12.4% Up" instead of just green text).
- Keyboard-accessible filters with visible focus states.

### Final Recommendation
Build using React Server Components (RSC) to stream data ledger tables, minimizing Time to First Byte (TTFB).

---

## 3. AI SaaS

### User Request
Design an interface for an AI-powered image generation tool where users write prompts and edit canvases.

### Design Thinking
- **Aesthetic:** OpenAI-inspired extreme minimalism focused on a single chat input bar.
- **Goal:** Make AI interactions feel simple, fast, and highly interactive.

### Layout Planning
- **Layout:** Centralized layout shell with a sticky prompt bar at the bottom, and a split-screen workspace showing options on the left and visual results on the right.

### Typography
- **Heading Font:** *Inter* (bold sans-serif).
- **Body Font:** *Inter* (regular) - 14px size for parameters, line-height: 1.5.

### Color Palette
- Canvas (60%): `hsl(220, 15%, 8%)` (Deep slate gray)
- Workspace Panels (30%): `hsl(220, 15%, 12%)`
- Accent (10%): `hsl(180, 70%, 50%)` (Cyan/teal neon for AI active states)

### Components
- Centered input prompt box featuring file upload controls and custom preset tags.
- Skeleton screens that shimmer while images generate.

### UX Decisions
- Provide real-time generation previews, showing the image sharpening as processing nears completion.
- Include quick "Variation" and "Undo" buttons immediately below the generated image.

### Mobile Strategy
- Slide workspace controls into a bottom sheet panel.
- Stack the prompt input above the visual canvas.

### Accessibility
- All slider controls include text inputs to support keyboard editing.
- Clear screen-reader alerts trigger when generations finish.

### Final Recommendation
Deploy using Edge caching for prompt presets, paired with WebSockets to handle real-time generation status updates.

---

## 4. Restaurant Website

### User Request
Design a website for an upscale fine dining restaurant handling reservations and displaying seasonal menus.

### Design Thinking
- **Aesthetic:** Apple-inspired visual storytelling with high-contrast food photos, clean margins, and elegant fonts.
- **Goal:** Highlight the culinary atmosphere and simplify booking.

### Layout Planning
- **Layout:** Centered single-column layout with sections separated by 128px margins to support visual storytelling on scroll.

### Typography
- **Heading Font:** *Playfair Display* (elegant serif) for menus and story headings.
- **Body Font:** *Inter* (clean sans-serif) for legibility.

### Color Palette
- Canvas (60%): `hsl(30, 20%, 98%)` (Warm cream)
- Borders & Accents (30%): `hsl(30, 10%, 15%)` (Charcoal black)
- Action Accent (10%): `hsl(35, 45%, 45%)` (Warm gold)

### Components
- Online menu card featuring inline dietary badges (Vegan, Gluten-free).
- Three-step reservation form modal (Date/Time -> Guests -> Contact).

### UX Decisions
- Keep reservation actions pinned to the header navigation at all times.
- Format the online menu as text instead of a PDF file to assist mobile layout and search engine optimization.

### Mobile Strategy
- Keep phone, booking, and address buttons easily accessible at the bottom of the screen.
- Ensure the menu card adapts to a readable single-column list.

### Accessibility
- Ensure contrast ratios for gold button text over cream backgrounds are AA compliant.
- Provide descriptive `alt` tags for all food imagery.

### Final Recommendation
Build as a static site (SSG) with dynamic integration for the booking calendar API to ensure fast load speeds.

---

## 5. Agency Website

### User Request
Create a website for a high-end digital design and development agency.

### Design Thinking
- **Aesthetic:** Stripe-inspired metallic gradients, clean typographic grids, and smooth scroll transitions.
- **Goal:** Position the agency as a modern technology and design leader.

### Layout Planning
- **Layout:** Standard 12-column grid container. Centered hero section, followed by a 3-column service grid.

### Typography
- **Heading Font:** *Outfit* (bold sans-serif).
- **Body Font:** *Inter* (regular sans-serif).

### Color Palette
- Canvas (60%): `hsl(0, 0%, 3%)` (Deep black)
- Cards & Borders (30%): `hsl(0, 0%, 8%)` with `border: 1px solid rgba(255,255,255,0.06)`
- Accent (10%): `hsl(210, 100%, 50%)` (Cobalt blue)

### Components
- Card items featuring subtle glowing borders that follow the mouse pointer on hover.
- Accordion lists showing step-by-step agency workflows.

### UX Decisions
- Hero sections include high-fidelity interactive screens showcasing real work deliverables.
- Keep pricing structures transparent and include clear links to book calls.

### Mobile Strategy
- Collapses the horizontal desktop navigation into a full-screen mobile menu.
- Stack the 3-column service grid into a single-column layout.

### Accessibility
- Ensure all hover states have matching focus outlines.
- Keep transition speeds under 300ms to prevent motion sickness.

### Final Recommendation
Use Next.js with localized Framer Motion animations to deliver a fast, responsive user interface.

---

## 6. E-commerce Store

### User Request
Design a checkout and product details experience for a minimal streetwear brand.

### Design Thinking
- **Aesthetic:** Notion-inspired minimalism focusing entirely on product details and spacing.
- **Goal:** Simplify checkout flows to minimize cart abandonment.

### Layout Planning
- **Layout:** Standard two-column desktop details shell. Left column displays the image gallery, while the right column remains fixed, showing sizes, product info, and CTAs.

### Typography
- **Heading Font:** *Inter* (bold sans-serif).
- **Body Font:** *Inter* (regular sans-serif) - 15px size, line-height: 1.5.

### Color Palette
- Canvas (60%): `hsl(0, 0%, 100%)` (Pure white)
- Cards & Borders (30%): `hsl(0, 0%, 96%)` with `border: 1px solid hsl(0, 0%, 90%)`
- Accent (10%): `hsl(0, 0%, 0%)` (Pure black for primary CTAs)

### Components
- Side-out shopping cart drawer showing product details and price summaries.
- One-page checkout flow with inline fields.

### UX Decisions
- Provide a guest checkout option to speed up onboarding.
- Display shipping details, return policies, and taxes clearly before payment steps.

### Mobile Strategy
- Pinned "Add to Cart" sticky footer CTA bar on mobile product detail pages.
- Large $52 \times 52\text{px}$ touch targets for size selections.

### Accessibility
- Ensure all size selection options are keyboard-focusable and readable.
- Check that alt text is provided for all product color variations.

### Final Recommendation
Implement dynamic image lazy loading and check edge network latencies to ensure fast product page loads.

---

## 7. Travel Website

### User Request
Design a booking directory interface for unique architectural rental cabins.

### Design Thinking
- **Aesthetic:** Airbnb-inspired spatial warmth, bold typographic headers, and large visual cards.
- **Goal:** Inspire adventure and make booking cabins simple.

### Layout Planning
- **Layout:** 12-column grid container. Top filter header, followed by a responsive 4-column card grid.

### Typography
- **Heading Font:** *Outfit* (bold sans-serif).
- **Body Font:** *Inter* (regular sans-serif).

### Color Palette
- Canvas (60%): `hsl(20, 20%, 99%)` (Off-white)
- Card Containers (30%): `hsl(0, 0%, 100%)`
- Accent (10%): `hsl(12, 80%, 50%)` (Warm terracotta orange)

### Components
- Search bar component combining location inputs, date pickers, and guest selectors.
- Grid cards displaying maps, reviews, prices, and photo galleries.

### UX Decisions
- Save search criteria in URL parameters to support sharing and returning to previous search results.
- Implement search updates dynamically as map parameters change.

### Mobile Strategy
- Keep search filters easily accessible in a sticky mobile header bar.
- Use horizontal swipe panels for mobile photo galleries.

### Accessibility
- Ensure input placeholder labels are easy to read and focusable.
- Map coordinates and locations must include text-based descriptors.

### Final Recommendation
Use Incremental Static Regeneration (ISR) to cache cabin listings on edge hosts, keeping search and page load speeds high.

---

## 8. Landing Page

### User Request
Create a conversion-focused landing page for a developer-oriented database tool.

### Design Thinking
- **Aesthetic:** Vercel/Linear dark theme featuring clean typographic grids and glowing visual details.
- **Goal:** Capture signups and drive downloads of the CLI tool.

### Layout Planning
- **Layout:** Centered single-column layout. Hero section features a bold value statement and code snippets, followed by a structured 3-column features table.

### Typography
- **Heading Font:** *JetBrains Mono* (monospaced sans-serif) for code features.
- **Body Font:** *Inter* (regular sans-serif) for copy.

### Color Palette
- Canvas (60%): `hsl(0, 0%, 2%)` (Deep black)
- Cards & Borders (30%): `hsl(0, 0%, 7%)` with `border: 1px solid rgba(255,255,255,0.05)`
- Accent (10%): `hsl(160, 100%, 50%)` (Mint green neon for code highlights)

### Components
- Single-click CLI command copy component featuring success indicators.
- Interactive pricing card grid with transparent tier options.

### UX Decisions
- Put a live interactive code console shell directly on the page to let developers test queries.
- Keep pricing structures clear and list value differences transparently.

### Mobile Strategy
- Stack features grids into readable single-column lists.
- Optimize code block formatting for mobile viewports to prevent layout breaks.

### Accessibility
- Ensure contrast values for green highlights are AA compliant over dark backgrounds.
- All copy components support keyboard navigation and include ARIA labels.

### Final Recommendation
Build using static generation (SSG) to ensure instant edge deployments and minimize page loading latencies.

---

## 9. Admin Dashboard

### User Request
Design a secure admin console to monitor system health and process support tickets.

### Design Thinking
- **Aesthetic:** Monochromatic, high-density dashboard layouts optimized for long-term operational workflows.
- **Goal:** Highlight alerts, simplify task execution, and reduce cognitive load.

### Layout Planning
- **Layout:** Sticky navigation sidebar, header tools, and a main 2-column layout grid.

### Typography
- **Heading Font:** *SF Pro* (sans-serif).
- **Body Font:** *Inter* (regular sans-serif).

### Color Palette
- Canvas (60%): `hsl(0, 0%, 96%)` (Cool gray)
- Containers & Cards (30%): `hsl(0, 0%, 100%)` with `border: 1px solid hsl(0, 0%, 88%)`
- Accent (10%): `hsl(220, 90%, 50%)` (System blue)

### Components
- System alerts panel showing alert levels (critical error, normal, warning) using clean status dots.
- Compact data table displaying task logs, sorting tools, and actions.

### UX Decisions
- Provide robust global search and filters to find tickets and logs quickly.
- Destructive admin actions (e.g., stopping database instances) are isolated and require confirmation.

### Mobile Strategy
- Admin dashboards are primarily optimized for desktop viewports; mobile fallbacks use simplified single-column task feeds.

### Accessibility
- All status dots and alert elements must include screen-reader labels (e.g., `aria-label="Critical Error Alert"`).
- Keyboard-accessible table rows support quick focus and navigation.

### Final Recommendation
Deploy using mTLS endpoints inside private VPC networks to ensure secure access.
