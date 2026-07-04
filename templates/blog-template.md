# Blog Template

This document provides specifications for building static blogs.

---

## 1. Overview & Purpose
- **Objective:** Deploy blogs using Static Site Generation (SSG), markdown files, search indexing, and RSS feeds.
- **Target Users:** Content writers and readers.
- **Recommended Stack:** Astro or Next.js, Tailwind CSS, MDX files, Cloudflare Pages.

---

## 2. Directory Layout

```text
blog-app/
├── src/
│   ├── components/      # ArticleCard, TableOfContents, NewsletterForm
│   ├── content/         # MDX content files
│   └── pages/           # Pages (Index, Post, Archive)
├── public/              # RSS xml assets
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** Local MDX content files.
- **API Requirements:** None, or client-side search logic index.
- **Authentication Strategy:** None.
- **UI Components:** Article progress indicator, reading progress bar, share buttons.
- **Pages:** Home, Article detail page, Tag archive, Search page.

---

## 4. Performance & Security

- **Performance:** Enforce static build parameters. Set cache control headers to maximize browser-side caching.
- **Security:** Sanitize markdown inputs if allowing dynamic comments; serve content over HTTPS.
- **Deployment:** Deploy directly to static hosting networks (Vercel, Cloudflare, Netlify).
