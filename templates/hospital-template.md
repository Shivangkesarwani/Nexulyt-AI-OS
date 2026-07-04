# Hospital Template

This document provides specifications for building HIPAA-compliant Hospital Management Systems (EMR).

---

## 1. Overview & Purpose
- **Objective:** Deploy healthcare networks with HIPAA compliance, doctor-patient portals, scheduling engines, and immutable access logs.
- **Target Users:** Medical staff, patients, and hospital administrators.
- **Recommended Stack:** React/Next.js, Go or Python, PostgreSQL (with row-level encryption), Redis, AWS (HIPAA-compliant BAA).

---

## 2. Directory Layout

```text
hospital-app/
├── src/
│   ├── components/      # PatientRecordTimeline, AppointmentScheduler
│   ├── database/        # Patient, appointments, audit tables
│   ├── routes/          # API routes
│   └── services/        # Encryption and audit logging services
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** PostgreSQL. Enforce database-level column encryption for Protected Health Information (PHI). Maintain an immutable audit log table recording all accesses.
- **API Requirements:** REST endpoints for appointment scheduling, patient records queries, and prescription management.
- **Authentication Strategy:** Multi-factor authentication (MFA) required. Enforce context-aware RBAC (Doctors, Nurses, Patients, Admins).
- **UI Components:** Health metrics charts, patient timelines, interactive appointment schedules.
- **Pages:** Doctor workspace dashboard, Patient portal, Appointment calendar.

---

## 4. Performance & Security

- **Performance:** Enforce caching on non-PHI lookup configurations (e.g. facility listings, doctor slots schedules).
- **Security:** Ensure HIPAA Security Rule compliance. Encrypt data in transit (TLS 1.3) and at rest (AES-256).
- **Deployment:** Deploy in private subnets on HIPAA-compliant cloud zones under a signed Business Associate Agreement (BAA).
