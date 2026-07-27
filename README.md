# Kutubuddin Rasel

Backend engineer. I care about what happens when two requests hit the same row at the same time — locking, idempotency, and ledgers that can't go out of balance.

Currently building Android at Kitalon Labs · writing backend systems everywhere else · looking for backend SWE roles.

[Email](mailto:juwelkutubuddin@gmail.com) · [LinkedIn](https://www.linkedin.com/in/kutubuddin-juwel/) · [GitHub](https://github.com/Kutubuddin-Rasel)

---

## Stack

| Area | Tools |
|---|---|
| Languages | TypeScript, Kotlin, C++ |
| Backend | NestJS, Prisma, TypeORM |
| Data | PostgreSQL, Redis |
| Currently learning | AWS — deploying Kori there next |

---

## Projects

### [Kori](https://github.com/Kutubuddin-Rasel/Kori) — Mobile Financial Service backend
![Status](https://img.shields.io/badge/status-in%20development-orange?style=flat-square)
`NestJS` `PostgreSQL` `Prisma` `Redis`

A from-scratch MFS backend (bKash/Nagad-style) built around one constraint: never corrupt a balance under concurrent load.

- **Double-entry ledger** — every transaction writes paired DEBIT/CREDIT rows atomically inside one DB transaction; funds are moved, never created or destroyed.
- **Deadlock-safe transfers** — wallet UUIDs are sorted before acquiring `FOR NO KEY UPDATE` pessimistic locks inside a Prisma transaction, removing circular wait chains between concurrent transfers.
- **Two-phase idempotency** — a Redis state machine rejects in-flight duplicate requests with `409` and replays the cached response for 24h, so a client retry can never double-charge.
- **BigInt-precision money** — no floats anywhere near a balance.

> [!NOTE]
> Not yet load-tested. Concurrency benchmark pending: `N concurrent overlapping-wallet transfers, p99 latency, deadlock count under load`.

---

### [PulseNews](https://github.com/Kutubuddin-Rasel/PulseNews) — AI-personalized news platform
![Status](https://img.shields.io/badge/status-live%20%7C%20pre--release-blue?style=flat-square)
`Android (Kotlin/Compose)` `NestJS` `Rust ingestion worker` — deployed on Azure

Three services, one hard constraint: the ranking backend and the embedding worker both had to run on a 1GB VM.

- Split the ONNX embedding pipeline into its own worker process specifically so the backend never has to load a 137MB model into its own memory budget.
- Chose an INT8-quantized embedding model and a persistent `embed_service` HTTP endpoint — backend calls it over an SSH tunnel — instead of loading the model per request.
- Composite ranking (embedding similarity + freshness decay + source quality + geo boost) with hashed A/B arm assignment for weight experiments.
- Self-configured the Azure deployment end to end: systemd services, Nginx reverse proxy, Docker memory limits and V8 heap caps to survive on a 1GB box.

**Status:** live at [pulsenewsbackend.me](https://pulsenewsbackend.me) — pre-release, no external traffic yet.
**Note:** the Rust worker's implementation was AI-ported from a Node reference I wrote and specified — see [AI-Assisted Development](#ai-assisted-development). I'm not claiming Rust fluency from it; everything else above is mine.

---

### [Zenith](https://github.com/Kutubuddin-Rasel/ZENITH) — Solo project management platform
`NestJS` `TypeORM` `PostgreSQL`

Started as a university assignment, still running a year later as an ongoing systems-design sandbox — a place to build backend patterns I don't get to use in my day job yet.

- **TOTP-based 2FA** — implemented and verified end-to-end. It's the one subsystem in this codebase I've fully tested and would defend without notes.
- The rest of the backend is still being built and tested. I'm not listing what's in there beyond that until I can stand behind it the same way — this section grows as pieces get proven, not as pieces get written.

**Status:** long-running, backend incomplete.

---

## AI-Assisted Development

I use Claude as a pair programmer — mainly to sanity-check whether an approach is idiomatic or violates SOLID, to compare design options before committing to one, and to unblock me as a junior engineer without a senior on hand to ask.

One specific case, disclosed because it matters: PulseNews's Rust ingestion worker started as logic I wrote and specified in Node, which I then had Claude port to Rust for the memory and performance profile a 1GB VM required. I designed the pipeline and made the architectural calls; I did not write that Rust myself and wouldn't want to be graded on Rust idiom from that file. Everything else here — the locking strategy in Kori, the deployment configs, the 2FA flow in Zenith — I wrote and can defend unaided.

---

## Background

**Experience:** Android Engineer @ Kitalon Labs (2026–present) · Full-Stack Developer Intern @ Techmak Technology (2025)
**Education:** BSc Computer Science, American International University-Bangladesh — CGPA 3.89/4.00, Dean's Award for Academic Excellence
**Competitive programming:** Codeforces Specialist, max rating 1448
