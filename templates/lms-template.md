# LMS Template

This document provides specifications for building Learning Management Systems (LMS).

---

## 1. Overview & Purpose
- **Objective:** Deploy course delivery networks with progress tracking, video player integrations, and quiz modules.
- **Target Users:** Students and instructors.
- **Recommended Stack:** React/Next.js, Node.js, PostgreSQL, Redis, Mux/Vimeo API.

---

## 2. Directory Layout

```text
lms-app/
├── src/
│   ├── components/      # VideoPlayer, CourseCurriculum, QuizCard
│   ├── database/        # Courses, modules, enrollments schemas
│   ├── routes/          # API route handlers
│   └── services/        # Progress tracking, video auth services
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** PostgreSQL. Tables for courses, chapters, student progress state, and quiz results.
- **API Requirements:** REST endpoints for course progress updates, video secure streaming requests, and quiz submissions.
- **Authentication Strategy:** JWT session tokens with RBAC (Student, Instructor, Admin).
- **UI Components:** Custom video player panel, curriculum navigation menus, quiz selection grids.
- **Pages:** Student dashboard, Course view panel, Instructor console, Settings.

---

## 4. Performance & Security

- **Performance:** Stream video contents via dedicated CDNs. Cache lesson metadata in Redis.
- **Security:** Secure video assets using signed access tokens to prevent hotlinking and sharing.
- **Deployment:** Deploy to managed containers (AWS ECS or GKE).
