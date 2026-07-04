# Agency Template

This document provides specifications for building an agency portfolio website.

---

## 1. Overview & Purpose
- **Objective:** Deploy an agency showcase platform detailing case studies, client reviews, and pricing matrices.
- **Target Users:** Agency clients and marketing teams.
- **Recommended Stack:** Next.js (ISR), Tailwind CSS, Headless CMS (Sanity or Contentful).

---

## 2. Directory Layout

```text
agency-app/
├── src/
│   ├── components/      # CaseStudyDetail, TestimonialCarousel, QuoteForm
│   ├── database/        # CMS schema files
│   └── pages/           # Pages (Home, Work, Services, Contact)
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** Headless CMS integration for dynamic content management.
- **API Requirements:** REST endpoints for lead generation and dynamic case study queries.
- **Authentication Strategy:** Basic auth for CMS editor access.
- **UI Components:** Interactive testimonial carousels, custom pricing grids, case study galleries.
- **Pages:** Home, Case studies, Services, Contact/Quote request.

---

## 4. Performance & Security

- **Performance:** Use ISR to pre-render dynamic case studies. Configure CDN image optimization.
- **Security:** Sanitize contact form entries; run SSL verification on all ingress paths.
- **Deployment:** Deploy to Vercel or Netlify.
