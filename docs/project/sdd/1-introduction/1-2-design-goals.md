---
title: SDD - 1.2 Design Goals | OhMyUniversity!
description: Design goals for OhMyUniversity establish measurable, prioritized objectives across data integrity, performance, reliability, usability, and scalability to guide architectural decisions.
head:
  - - meta
    - property: og:title
      content: SDD - 1.2 Design Goals | OhMyUniversity!
  - - meta
    - property: og:description
      content: Design goals define the measurable objectives that OhMyUniversity must achieve, prioritized by importance to the university and students.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-2-design-goals
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, design goals, performance, reliability, scalability, data integrity, usability, gdpr, accessibility, legal compliance
  - - meta
    - name: twitter:title
      content: SDD - 1.2 Design Goals | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Measurable design goals guiding OhMyUniversity architecture.
---

# OhMyUniversity! - SDD: 1.2 Design Goals

Design Goals translate system requirements into concrete, measurable objectives that drive all architectural decisions. Each goal has a priority, specific metric, and rationale. Trade-offs between conflicting goals are explicitly resolved.

---

## Design Goals (Prioritized)

| Priority | Goal | Metric | Rationale |
|----------|------|--------|-----------|
| **P1** | **Data Integrity** | 100% correspondence with CINECA records; zero calculation errors | Students trust system with academic records; errors impact future opportunities (Master's eligibility, graduation date) |
| **P1** | **Performance** | Sync from CINECA in < 2.0 seconds; 3-tap access for expert users | Expert users expect instant access; reduces friction vs current fragmented portals |
| **P1** | **Reliability** | 99.9% monthly uptime; graceful fallback when external systems unavailable | Students access platform for critical decisions (exam booking, Master's planning); unexpected downtime causes lost registrations |
| **P1** | **Usability** | New users complete classroom booking in < 60 seconds without documentation; intuitive interface | Platform must reduce cognitive load; complexity drives users back to fragmented portals |
| **P2** | **Scalability** | New domain services can be added and scaled independently of `api-core` | Each service scales independently based on demand (e.g. a future Classroom/Transport service overloaded ≠ api-core scales) |
| **P2** | **Maintainability** | Clear separation of concerns via independent services; consistent coding standards; meaningful test coverage | Team continuity requires clean architecture; future developers must onboard quickly |
| **P2** | **Authentication** | Authentication delegated to CINECA/Esse3 institutional credentials; session represented as a short-lived JWT (90 min) with client-side refresh | Student data is highly sensitive (grades, personal data); the institution remains the sole source of truth for identity |
| **P1** | **Legal Compliance & Data Protection** | GDPR-compliant data handling; data subject rights (access, export, delete) supported | Student data is sensitive; Italian universities must comply with EU GDPR |

> **Roadmap note:** SPID/CIE login, AWS/Kubernetes-based deployment, and the Classroom/Transport
> services' frontend integration are planned evolutions of the system, not implemented in the
> current sprint. They are listed here for traceability but are **not** to be read as current
> architectural facts—see the "Roadmap" callouts below for each affected goal.

---

## Trade-offs & Resolution

### Trade-off 1: Performance (P1) vs Legal Compliance (P1)
**Conflict**: Caching CINECA data in Redis achieves the Performance goal (sync target < 2.0s) by
avoiding repeated calls to external APIs, but it stores sensitive student data outside of the
system of record (PostgreSQL), which must be reconciled with the data minimization and storage
limitation principles required by GDPR (see RAD 3.3.8 Legal).

**Solution**: Cached entries use a bounded, time-limited expiration so that cached sensitive data
never persists in Redis longer than strictly necessary to serve the performance goal. PostgreSQL
remains the system of record; Redis is treated purely as a disposable, regenerable cache layer,
never as the authoritative store for student data.

### Trade-off 2: Maintainability (P2) vs Performance (P1)
**Conflict**: Optimizing code for performance (tight loops, manual caching, complex
data structures) sacrifices readability and maintainability. Conversely, writing clean,
readable code (clear logic, comments, simple abstractions) may add computational overhead
that impacts the Performance Design Goal (sync < 2.0s) on critical paths like CINECA sync
and grade calculation.

**Solution**: Apply a data-driven optimization strategy. Default to readable, maintainable
code. Profile actual bottlenecks (not developer guesses) before optimizing. Optimize only
measurable critical paths (CINECA sync, grade calculation) where overhead impacts Design
Goals. Accept a small performance trade-off on non-critical paths to preserve code
maintainability.

### Trade-off 3: Data Integrity (P1) vs Scalability (P2)

**Conflict**: Independent services enable horizontal scaling per-service (Scalability Design
Goal), but independent scaling inherently creates eventual consistency. `api-core` receives and
processes data from CINECA; domain services that consume this data asynchronously may
temporarily observe stale data relative to what `api-core` just processed. Maintaining
instantaneous Data Integrity across independent services would require synchronous calls from
every consumer back to `api-core`, which undermines the Scalability Design Goal.

**Solution**: `api-core` is the single point of integration with CINECA/Esse3 and the single
publisher of domain events to Kafka. Other services (e.g. the Classroom and Transport services,
once integrated) consume these events asynchronously rather than calling CINECA directly or
querying `api-core`'s database. This hub-and-spoke event flow keeps `api-core` as the sole
source of truth for CINECA-derived data, bounds the propagation lag to the time needed to
process and publish an event, and allows downstream services to scale independently without
weakening data integrity at the source.

---

## Design Goals Impact on Key Architectural Decisions

| Design Goal | Architectural Impact | Driven By (NFR from RAD) |
|---|---|---|
| **Data Integrity** | PostgreSQL as system of record (ACID transactions) for all persistent data (e.g. calendar, user data); `api-core` as sole writer of CINECA-derived data | RAD 3.3.2 (Reliability) |
| **Performance** | Redis cache-aside layer in front of CINECA/Esse3 calls, to avoid repeated external API calls | RAD 3.3.3 (Performance) |
| **Reliability** | Scheduled background sync (`CinecaSyncService`/`CinecaSyncState`) decoupled from user sessions; Kafka used to decouple `api-core` from downstream consumers | RAD 3.3.2 (Reliability) |
| **Usability** | SPA (Angular, Signals-based state), mobile app (Flutter), logical navigation hierarchy | RAD 3.3.1 (Usability) |
| **Scalability** | Service-based architecture: `api-core`, `api-gateway`, `api-fetcher` today; Classroom and Transport services already scaffolded as independent modules for future integration | RAD 3.3.4 (Supportability) |
| **Maintainability** | Consistent coding conventions per platform (Java/Dart/TypeScript), linting enforcement in CI | RAD 3.3.5 (Implementation) |
| **Authentication** | Login delegated to CINECA/Esse3; `api-core` issues a short-lived JWT (90 min) plus a refresh token, refreshed client-side before expiry; `OmuUser` is created/linked on first login and associated across multiple academic careers via fiscal code | RAD 3.3.6 (Interface) + RAD 3.3.8 (Legal) |

> **Roadmap (not yet implemented):** SPID/CIE as an additional/primary authentication method;
> deployment on AWS or Kubernetes with autoscaling; MongoDB-backed chat feature (Kafka is not
> involved in chat at this stage); frontend integration of the Classroom and Transport services.

---