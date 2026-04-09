<div align="center">

# Kutubuddin Juwel

Backend Engineer · Distributed Systems · Financial-Grade Reliability

[Portfolio](https://portfolio-three-omega-72.vercel.app/) &nbsp;·&nbsp; [LinkedIn](https://www.linkedin.com/in/kutubuddin-juwel/) &nbsp;·&nbsp; [Email](mailto:juwelkutubuddin@gmail.com)

</div>

---

## About

CS graduate specializing in backend architecture and distributed systems. I am drawn to problems where the cost of incorrectness is high — financial transactions, concurrent state, and real-time consistency. My recent work spans a financial service engine built around double-entry bookkeeping and pessimistic concurrency control, a real-time collaboration platform with Redis-backed WebSocket scaling, and a chat system architected around cursor-based pagination and optimistic UI. I think carefully about failure modes before writing the first line of code.

Actively seeking backend and software engineering roles.

---

## Projects

### Kori — Mobile Financial Service Engine

`NestJS` `PostgreSQL` `Redis` `Argon2` `TypeScript`

A production-grade financial backend where every architectural decision is driven by one constraint: money must never be created, lost, or duplicated under any concurrency or network failure scenario.

**Why pessimistic locking over optimistic locking:**
In a financial system, concurrent transfers from the same account are not a rare edge case — they are expected traffic. Optimistic locking assumes conflicts are infrequent and resolves them at commit time with a rollback and retry. Under high concurrency on hot accounts, this produces a thundering herd of retries, compounding the very contention it is trying to resolve. Pessimistic locking (`SELECT ... FOR NO KEY UPDATE`) serializes access to account rows at the database level, paying the cost of a short wait once rather than the cost of repeated rollbacks. Correctness is guaranteed without application-level retry logic.

**Why double-entry bookkeeping over a simple debit/credit model:**
A single-entry model has no internal consistency check — a bug that debits without crediting produces silent data corruption detectable only by manual audit. A double-entry ledger requires every transfer to post two offsetting immutable journal entries. The sum of all entries in the system must mathematically equal zero at all times. Corruption becomes immediately detectable. The ledger is also append-only: no balance is ever overwritten, only derived from entry history.

**Transaction flow:**

```
Client Request (with x-idempotency-key)
      │
      ▼
[Interceptor] Redis Idempotency Check
  ├── State: PROCESSING? ───────────────► Throw 409 Conflict (Prevent race conditions)
  ├── State: EXISTS? (Cache Hit) ───────► Return cached JSON response (No DB hit)
  └── State: NEW REQUEST?
      │
      ▼
Set Redis State ──► 'PROCESSING' (30s TTL Lock)
      │
      ▼
[Prisma $transaction] BEGIN
  │
  ├── 1. Sort Wallet IDs Alphabetically      ← Prevents PostgeSQL deadlocks at scale
  ├── 2. SELECT rows FOR NO KEY UPDATE       ← Acquire Pessimistic Row-Level Locks
  ├── 3. Re-fetch fresh balances             ← Ensure math is based on locked state
  ├── 4. Validate sufficient funds
  │
  ├── 5. Insert `Transaction` Record         ← DB Unique Constraint checks 'idempotencyKey' (Fail-safe)
  │
  ├── 6. Update Sender Wallet Balance        (Combined Transfer + Fee)
  ├── 7. Insert `LedgerEntry`: DEBIT         (Source wallet: Total Amount)
  │
  ├── 8. Update Receiver Wallet Balance
  ├── 9. Insert `LedgerEntry`: CREDIT        (Destination wallet: Transfer Amount)
  │
  ├── 10. Update System Wallet Balance       [If Fee > 0]
  └── 11. Insert `LedgerEntry`: CREDIT       [If Fee > 0] (System Revenue wallet: Fee Amount)
      │
COMMIT (DB Locks released automatically)
      │
      ▼
[Interceptor] On Success
      │
      ▼
Cache exact HTTP JSON response in Redis (24h TTL)
      │
      ▼
Return Response to Client
```

**Why Redis for idempotency rather than a database unique constraint:**
A database constraint on the idempotency key would work, but it hits the database on every request including duplicates. Redis gives O(1) in-memory lookup, so duplicate requests are rejected before any database connection is acquired or any lock is contested. Under retry storms from a failed client, this protects the database entirely.

**Additional design decisions:**
- Multi-factor identity layer with device-binding — a stolen credential from one device is non-transferable
- Argon2id for password hashing — resistant to both GPU brute-force and side-channel attacks
- Multi-typology transaction router — Send Money, Cash-In, Cash-Out, and Merchant Payments share one engine with typology-specific fee computation

[View Repository](https://github.com/Kutubuddin-Rasel/kori) — *Core engine operational. Active development.*

---

### Zenith — Agile Project Management Platform

`NestJS` `Next.js` `Socket.io` `Redis` `PostgreSQL` `TypeScript`

A Jira-scale project management platform built for real engineering teams. The two hardest design problems were access control granularity and WebSocket scaling.

**Access control:** Five-tier RBAC (Super-Admin, Project Lead, Developer, QA, Viewer) where permissions are scoped per workspace, not globally. A user can be a Project Lead in one workspace and a Viewer in another. This required a join-table permission model rather than a role column on the user record — user-level roles cannot express workspace-scoped visibility without leaking data across project boundaries.

**WebSocket scaling:** Node.js WebSocket connections are instance-local. Without a shared message bus, a notification triggered on instance A cannot reach a client connected to instance B. Redis pub/sub creates a shared event bus across all instances — when an event is published on any instance, Redis fans it out and each instance delivers it to its locally connected clients. This allows horizontal scaling without sticky sessions.

[View Repository](https://github.com/Kutubuddin-Rasel/zenith) — *Active development.*

---

### Blip — Real-Time Chat Application

`NestJS` `Next.js` `Socket.io` `Redis` `PostgreSQL` `Firebase` `TypeScript`

Built to understand the full engineering surface of real-time backend design. The decision that required the most thought was pagination strategy.

**Why cursor-based pagination over offset pagination:**
`LIMIT n OFFSET k` requires the database to scan and discard the first `k` rows on every page request — O(k) work that grows with scroll depth. More critically, if a new message is inserted while a user is scrolling, every subsequent page shifts by one row, producing either a duplicate or a skipped message. Cursor-based pagination uses the last-seen message ID as a stable pointer: `WHERE id < :cursor ORDER BY id DESC LIMIT n`. The result set is stable regardless of concurrent inserts, and the query uses the primary key index — O(log n) regardless of history depth.

**Additional design decisions:**
- Hybrid authentication: Firebase SMS verification for identity, custom JWT for session control — leverages Firebase's verified phone number infrastructure without ceding session ownership
- Optimistic UI updates: messages render immediately on send and reconcile with server confirmation asynchronously, eliminating perceived round-trip latency

[View Repository](https://github.com/Kutubuddin-Rasel/blip)

---

### Conversa — Android Messaging App

`Kotlin` `Jetpack Compose` `Firebase` `MVVM`

Native Android messaging client built to develop production-grade Android architecture instincts. Clean MVVM separation with reactive state management via StateFlow, real-time message delivery, full conversation history, and profile management.

[View Repository](https://github.com/Kutubuddin-Rasel/conversa)

---

### Super Mario — 2D Platformer

`C++` `OpenGL` `GLUT`

A 2D platformer built to explore systems-level graphics programming and OOP design at the entity level. Characters, obstacles, coins, and the physics engine are independent, composable class hierarchies. Includes collision detection, dynamic camera tracking, scoring, and a lives system.

[View Repository](https://github.com/Kutubuddin-Rasel/SuperMario)

---

## Experience

**Full Stack Developer Intern — Techmak Technology** *(July 2025 – October 2025)*

Sole developer on DhakaVoice, a civic engagement web platform. Designed the database schema from scratch, implemented the full REST API layer (15+ endpoints) with JWT authentication and Argon2 password hashing, built the WebSocket real-time communication system, and integrated Prisma ORM for type-safe database access. Frontend work included Next.js server/client component architecture and bundle optimization via code splitting and lazy loading.

---

## Technical Skills

**Proficient:** TypeScript, NestJS, PostgreSQL, Redis, Node.js, C, C++

**Working Knowledge:** Kotlin, Jetpack Compose, Next.js, MongoDB, Docker, Firebase, PHP

**Learning:** Go, C#/.NET Core

---

## Education

**BSc in Computer Science** — American International University-Bangladesh · Graduated December 2025

Relevant coursework: Algorithms & Data Structures, Operating Systems, Computer Networks, Database Systems

---

## Competitive Programming

**Codeforces:** [Rk899](https://codeforces.com/profile/Rk899) — 193 problems solved

Currently working through the NeetCode 150 on LeetCode, focused on graph algorithms and dynamic programming.
