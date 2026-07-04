# Restaurant Template

This document provides the system architecture and specifications for building a real-time Restaurant SaaS with POS terminals and kitchen displays.

---

## 1. Overview & Purpose
- **Objective:** Deploy order processing networks with real-time WebSocket state syncing between POS inputs, kitchen boards, and table status maps.
- **Target Users:** Waitstaff, kitchen managers, and cashier operators.
- **Recommended Stack:** React, Node.js + Fastify, WebSocket, PostgreSQL, Redis.

---

## 2. Directory Layout

```text
restaurant-app/
├── src/
│   ├── components/      # POSGrid, KitchenTicketCard, TableMap
│   ├── database/        # Menu, orders, tables data tables
│   ├── routes/          # REST routes for menu and settings
│   └── websockets/      # WebSocket message handlers
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** PostgreSQL. Tables for menu items, active tickets, and table maps.
- **API Requirements:** REST endpoints for menu changes; WebSocket streams for order update messages (`order-create`, `ticket-update`, `table-release`).
- **Authentication Strategy:** Short PIN validations for waitstaff terminals; session JWT controls for admin consoles.
- **UI Components:** Touch-friendly menu selection grids, ticket detail panels with status alerts, table layout visual grids.
- **Pages:** POS input terminal screen, Kitchen display board, Management dashboard.

---

## 4. Performance & Security

- **Performance:** Enforce local SQLite fallbacks in POS terminals for offline resilience if network drops. Use Redis pub/sub backplanes for low WebSocket latencies.
- **Security:** Ensure multi-tenant databases isolate order history by restaurant ID context parameters.
- **Deployment:** Deploy backends on AWS or DigitalOcean regions with container redundancy.
