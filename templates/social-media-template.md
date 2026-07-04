# Social Media Template

This document provides specifications for building social media platforms.

---

## 1. Overview & Purpose
- **Objective:** Deploy platform networks with activity feeds, photo/video uploads, user profiles, and real-time chat.
- **Target Users:** General consumers.
- **Recommended Stack:** React, Node.js + Socket.io, PostgreSQL, Redis, AWS S3 (CDN).

---

## 2. Directory Layout

```text
social-media-app/
├── src/
│   ├── components/      # ActivityFeed, PostComposer, ChatRoom
│   ├── database/        # Posts, users, friendships schemas
│   ├── routes/          # API routes
│   └── websockets/      # WebSocket event controllers
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** PostgreSQL for post graphs; Redis for real-time notification feeds.
- **API Requirements:** REST routes for user profile updates, post CRUD; WebSocket event streams for messaging (`message-send`, `user-presence`).
- **Authentication Strategy:** JWT with refresh token parameters.
- **UI Components:** Infinite scroll feed loaders, media attachment buttons, user profile header tags.
- **Pages:** Home Feed, Profile view page, Direct Messages portal.

---

## 4. Performance & Security

- **Performance:** Paginate activity feeds. Load pictures dynamically using image optimization.
- **Security:** Sanitize inputs to prevent script injections. Configure secure file upload policies (signed URLs).
- **Deployment:** Deploy to Kubernetes clusters with autoscaling.
