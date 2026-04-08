<div align="center">

# Kutubuddin Juwel

**Backend Engineer · Distributed Systems · CS Graduate (CGPA 3.89/4.00)**

[Portfolio](https://portfolio-three-omega-72.vercel.app/) · [LinkedIn](https://www.linkedin.com/in/kutubuddin-juwel/) · [Email](mailto:juwelkutubuddin@gmail.com) · [Codeforces](https://codeforces.com/profile/Rk899)

</div>

---

## About

CS graduate from American International University-Bangladesh specializing in backend architecture, distributed systems, and financial-grade reliability engineering. I focus on systems where correctness is non-negotiable: transactional integrity, concurrency safety, and fault tolerance. I have built across the full backend surface — REST APIs, real-time WebSocket layers, mobile clients, and 2D game engines — but my deepest interest is in designing systems that stay correct under load and failure.

Currently seeking backend and software engineering roles where scale and correctness are taken seriously.

---

## Technical Skills

**Proficient:** TypeScript, NestJS, PostgreSQL, Redis, Node.js, C, C++

**Working Knowledge:** Kotlin, Jetpack Compose, Next.js, MongoDB, Docker, Firebase, PHP

**Learning:** Go, C#/.NET Core

---

## Projects

### Kori — Mobile Financial Service Engine

`NestJS` `PostgreSQL` `Redis` `Argon2` `TypeScript`

A production-grade financial backend engineered around strict correctness guarantees. The design prioritizes preventing money creation, double-spending, and race conditions at the architecture level — not at the application level.

**Core design decisions:**

- **Immutable double-entry ledger** — every transfer posts two offsetting journal entries, making money creation or silent loss a mathematical impossibility
- **Pessimistic locking** (`SELECT ... FOR NO KEY UPDATE`) on account rows during transfers, eliminating lost-update anomalies under high concurrency
- **ACID-compliant transaction isolation** — core transfer logic is wrapped in serialized database transactions to prevent phantom reads and dirty reads
- **Distributed idempotency layer** via Redis — every transfer request is keyed to a client-supplied idempotency token, making retried or duplicate network requests safe at the infrastructure level
- **Multi-typology transaction routing** — a single transaction engine handles Send Money, Cash-In, Cash-Out, and Merchant Payments, with fee amounts dynamically computed and routed to a dedicated system revenue wallet
- **Multi-factor identity layer** — device-binding tied to cryptographic Argon2 hashing, making credential replay attacks across devices non-viable

[View Repository](https://github.com/Kutubuddin-Rasel/kori) — *Core engine operational. Active development.*

---

### Zenith — Agile Project Management Platform

`NestJS` `Next.js` `Socket.io` `Redis` `PostgreSQL` `TypeScript`

A Jira-scale project management platform built for real engineering teams. Designed with a five-tier RBAC model (Super-Admin, Project Lead, Developer, QA, Viewer) where each role has granular, independently scoped permissions per workspace. Real-time collaboration on Kanban and Scrum boards is delivered via persistent WebSocket connections with Redis-backed pub/sub for horizontal scaling.

Features: sprint planning, backlog management, issue tracking, live notifications, and full team management. Authentication via JWT with refresh token rotation.

[View Repository](https://github.com/Kutubuddin-Rasel/zenith) — *Active development.*

---

### Blip — Real-Time Chat Application

`NestJS` `Next.js` `Socket.io` `Redis` `PostgreSQL` `Firebase` `TypeScript`

A scalable chat system built to understand the full surface of real-time backend design. Key architectural choices: hybrid authentication combining Firebase SMS verification with custom-issued JWTs; cursor-based pagination for message history (avoiding offset pagination's consistency problems at scale); optimistic UI updates for perceived zero-latency; and WebSocket connection state managed through Redis for multi-instance compatibility.

[View Repository](https://github.com/Kutubuddin-Rasel/blip)

---

### Conversa — Android Messaging App

`Kotlin` `Jetpack Compose` `Firebase` `MVVM`

Native Android messaging client built to learn production-grade Android architecture. Clean MVVM separation with reactive state management, real-time message delivery, conversation history, and profile management. Firebase handles auth and message persistence.

[View Repository](https://github.com/Kutubuddin-Rasel/conversa)

---

### Super Mario — 2D Platformer

`C++` `OpenGL` `GLUT`

A 2D platformer built in C++ with OpenGL to explore systems-level graphics programming. The entity system is fully OOP-designed: characters, obstacles, coins, and physics are implemented as composable class hierarchies. Includes collision detection, dynamic camera tracking, a scoring system, and a lives indicator.

[View Repository](https://github.com/Kutubuddin-Rasel/super-mario)

---

## Experience

**Full Stack Developer Intern — Techmak Technology** *(July 2025 – October 2025)*

Built DhakaVoice, a civic engagement web platform, as the sole developer on the project. Responsibilities spanned the full backend: database schema design, 15+ REST API endpoints with JWT authentication and Argon2 password hashing, WebSocket-based real-time communication, and Prisma ORM integration for type-safe database access. Frontend work included server/client component architecture in Next.js and bundle optimization via code splitting and lazy loading.

---

## Education

**BSc in Computer Science** — American International University-Bangladesh

CGPA: 3.89 / 4.00 · Graduated December 2025

Dean's Award for Academic Excellence · Science Poster Contest Award (Fall 2022–23)

Relevant coursework: Algorithms & Data Structures, Operating Systems, Computer Networks, Database Systems

---

## Competitive Programming

**Codeforces:** [Rk899](https://codeforces.com/profile/Rk899) — Specialist · 193 problems solved

Currently working through the NeetCode 150 problem set on LeetCode, focused on closing gaps in graph algorithms and dynamic programming.
