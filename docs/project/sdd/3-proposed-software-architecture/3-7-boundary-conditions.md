---
title: SDD - 3.7 Boundary Conditions | OhMyUniversity!
description: Definition of the boundary conditions for the OhMyUniversity! system, including persistent object configuration, component lifecycle (start-up, shutdown), and exceptional use cases, formalized via standard Use Case templates.
head:
  - - meta
    - property: og:title
      content: SDD - 3.7 Boundary Conditions | OhMyUniversity!
  - - meta
    - property: og:description
      content: Definition of the boundary conditions for the OhMyUniversity! system, including persistent object configuration, component lifecycle, and exceptional use cases.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-7-boundary-conditions
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, boundary conditions, startup, shutdown, exceptions, exceptional use cases, configuration, lifecycle, persistence, ACID, fallback
  - - meta
    - name: twitter:title
      content: SDD - 3.7 Boundary Conditions | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Definition of the boundary conditions for the OhMyUniversity! system, including persistent object configuration, component lifecycle (start-up, shutdown), and exceptional use cases.
---

# OhMyUniversity! - SDD: 3.7 Boundary Conditions

## Overview

Boundary Conditions define the system's behavior in **non-ordinary states**—initialization,
termination, and error scenarios. As elsewhere in this document, this section distinguishes
between boundary conditions that correspond to **implemented** behavior today and boundary
conditions for functionality that is **planned but not yet built**. The latter are included for
completeness and traceability, but are explicitly marked as roadmap items rather than current
system behavior.

## Boundary Condition Actors

| Type | Actor | Description | Status |
|---|---|---|---|
| Secondary | System Administrator | Authorized personnel managing system configuration. | Roadmap — no admin panel exists today |
| Secondary | Infrastructure & Operations | Responsible for deploying and monitoring `api-core`, `api-gateway`, `api-fetcher`. | Implemented (manual operations; no automated orchestration yet, see SDD 3.3) |
| Secondary | PostgreSQL | Persistent data store for `api-core` and `api-fetcher`. | Implemented |
| Secondary | Redis | Cache layer for Esse3-derived data, used by `api-core`. | Implemented |
| Secondary | api-gateway | Single entry point; routes requests to `api-core` and `api-fetcher`. | Implemented |
| Secondary | Esse3 | External system holding the source of truth for academic records. | Implemented (the only external integration today) |

---

## Configuration of Persistent Objects (Roadmap)

The following configuration use cases describe administrative capabilities that are part of the
functional requirements (RAD FR-1.6.x and related) but have **no corresponding implementation
today**—there is no administration panel, and none of the entities below (`PartnerOrg`,
`SystemParameter`, `AcademicYear`, `IntegrationCredential`) exist in the current codebase. They are
listed here as a scaffold for future design work, not as a specification of existing behavior.

| Planned Use Case | Purpose | Status |
|---|---|---|
| **UC-Admin-01: Manage Partner Organizations** | Create/review/provision `PartnerOrg` accounts (RAD: Partner Provisioning). | Roadmap — depends on the Partner Convention module (SDD 3.2) |
| **UC-Admin-02: Manage System Parameters** | Tune operational parameters (e.g. cache TTL, timeouts) without a redeploy. | Roadmap — no `SystemParameter` store exists; today such values live in service configuration files |
| **UC-Admin-03: Manage Academic Years and Calendars** | Define semester boundaries, exam windows, holiday periods. | Roadmap — academic calendar structure is owned by Esse3 today; OhMyUniversity does not maintain its own copy |
| **UC-Admin-04: Manage Integration Credentials** | Manage credentials for external integrations. | Partially applicable today only for Esse3 connection configuration; no credential-management UI exists |

When these capabilities are implemented, this section should be expanded with full Use Case
descriptions (actors, event flow, exceptions, exit conditions) grounded in the actual data model at
that time, rather than a model designed speculatively now.

---

## Component Lifecycle (Implemented)

`api-core`, `api-gateway`, and `api-fetcher` are independent Spring Boot processes. Each follows
the standard Spring Boot lifecycle:

- **Start-up:** on launch, each service loads its configuration (`application-{profile}.yaml`),
  establishes its database connection pool, and—for `api-core`—initializes the Kafka producer.
  `api-core` and `api-fetcher` run Flyway migrations against their respective PostgreSQL schemas
  on start-up if pending migrations exist.
- **Termination:** services shut down on receiving a termination signal, completing in-flight
  requests where possible before releasing database and Kafka connections.
- **Configuration reload:** there is currently no live configuration reload mechanism; changing
  configuration requires a restart of the affected service. A dynamic reload mechanism
  (UC-Admin-02 above) is a roadmap item, not current behavior.

---

## Exception and Failure Handling (Implemented Scenarios)

This section covers the two failure scenarios that correspond to integrations and infrastructure
that exist today: loss of connectivity to Esse3, and loss of connectivity to PostgreSQL. Scenarios
involving functionality not yet implemented (e.g. classroom booking conflicts) are intentionally
not specified here—see SDD 3.6 for why a concurrency/conflict strategy for those cases is deferred.

<br>

### UC-Exc-01: Esse3 Unavailable — Fallback to Cached Data

(`<<extend>>` UC-02 View Academic Career, and any other use case that reads Esse3-derived data)

| Section | Content |
|---|---|
| **Actors** | **Primary Actor:** Student • **Secondary Actors:** Redis, Esse3 |
| **Assumptions** | The student has successfully synced at least once before, so a cached response exists in Redis for this student. |
| **Entry Conditions** | The student requests Esse3-derived data (e.g. academic career). • The call from `api-core` to Esse3 times out or returns an error. |

| Event Flow | Student | System |
|---|---|---|
| 1. | The student requests their academic career. | |
| 2. | | `api-core` checks Redis for a cached response; on a miss, it calls Esse3. |
| 3. | | Esse3 does not respond within the configured timeout, or returns an error. |
| 4. | | `api-core` falls back to the last cached value in Redis for this student, if present. |
| 5. | | The response is returned to the client with an indication that the data may not be current. |

| Section | Content |
|---|---|
| **Exceptions** | **No cached value exists:** if the student has never synced successfully before, the system returns an error indicating Esse3 is temporarily unavailable, rather than fabricating a response. |
| **Exit Conditions** | The student sees the last known data with a staleness indicator, or a clear error if no cached data exists. No write operation is attempted against Esse3 while it is unreachable. |

<br>

### UC-Exc-02: PostgreSQL Connection Loss

(`<<extend>>` any use case that requires reading or writing native platform data, e.g. `OmuUser`,
`CalendarEvent`)

| Section | Content |
|---|---|
| **Actors** | **Primary Actor:** `api-core` or `api-fetcher` • **Secondary Actor:** PostgreSQL |
| **Assumptions** | The service is running normally with an active connection pool to PostgreSQL. |
| **Entry Conditions** | The service attempts a database operation. • The connection is no longer valid (network issue or database unavailable). |

| Event Flow | Service |
|---|---|
| 1. | The service attempts to acquire a connection from the pool to execute a query. |
| 2. | The connection is invalid or the acquisition fails. |
| 3. | The error is logged. |
| 4. | The service surfaces an error to the caller (HTTP 5xx) rather than returning a partial or fabricated result. |
| 5. | Once PostgreSQL becomes reachable again, subsequent requests succeed without requiring a restart of the service, since the connection pool re-establishes connections automatically. |

| Section | Content |
|---|---|
| **Exceptions** | **In-flight transaction during outage:** any transaction in progress when the connection is lost is rolled back by PostgreSQL's standard ACID guarantees; no partial write is persisted. |
| **Exit Conditions** | If PostgreSQL recovers, normal operation resumes automatically. If it does not, the affected service continues returning errors until it does; no automatic failover to a replica is implemented today. |

<br>

### UC-Exc-03: Login Failure Handling

(Verified against `AuthController`/`AuthService` — not an `<<extend>>` of a base use case, since
login itself is the entry point)

| Section | Content |
|---|---|
| **Actors** | **Primary Actor:** Student or Professor • **Secondary Actor:** Esse3 |
| **Assumptions** | The user submits institutional credentials and a `universityId` to `api-core`'s login endpoint. |
| **Entry Conditions** | The user submits the login form. |

| Event Flow | User | System |
|---|---|---|
| 1. | The user submits university ID, username, and password. | |
| 2. | | `api-core` validates the request payload and looks up the university by `universityId`. |
| 3. | | `api-core` delegates credential verification to Esse3. |

| Outcome | Condition | Response |
|---|---|---|
| Success | Esse3 confirms the credentials | `200 OK` with access token, refresh token, and career claims |
| Invalid credentials | Esse3 rejects the credentials | `401 Unauthorized` |
| Unknown university | The submitted `universityId` is not registered | `404 Not Found` |
| Esse3 unreachable | Esse3 does not respond or errors out | `503 Service Unavailable` |

| Section | Content |
|---|---|
| **Exit Conditions** | On success, the user holds a valid access token and refresh token. On any failure, no token is issued and the specific HTTP status communicates the cause to the client, without exposing internal error details. |

<br>

### UC-Exc-04: Refresh Token Invalid or Expired

| Section | Content |
|---|---|
| **Actors** | **Primary Actor:** Client application • **Secondary Actor:** Redis |
| **Assumptions** | The client holds a refresh token issued at a previous login. |
| **Entry Conditions** | The client's access token is at or near expiry and it requests a new one using the refresh token. |

| Event Flow | Client | System |
|---|---|---|
| 1. | The client requests a new access token, presenting the refresh token. | |
| 2. | | `api-core` looks up the refresh token in Redis. |
| 3. | | If the token is missing (expired or already invalidated by a previous logout) or otherwise invalid, the request is rejected. |

| Section | Content |
|---|---|
| **Exceptions** | **Refresh token invalid/expired:** `401 Unauthorized` is returned; the client must redirect the user to a full login. |
| **Exit Conditions** | On success, a new access token is issued without requiring the user to re-enter institutional credentials. On failure, the user must log in again via Esse3. |

---

## Traceability to Base Use Cases

| Boundary Condition Use Case | Extends / Relates To | Status |
|---|---|---|
| UC-Admin-01: Manage Partner Organizations | N/A (Configuration) | Roadmap |
| UC-Admin-02: Manage System Parameters | N/A (Configuration) | Roadmap |
| UC-Admin-03: Manage Academic Years | N/A (Configuration) | Roadmap |
| UC-Admin-04: Manage Integration Credentials | N/A (Configuration) | Partially applicable, no UI |
| UC-Exc-01: Esse3 Unavailable — Fallback to Cached Data | `<<extend>>` UC-02 | Implemented |
| UC-Exc-02: PostgreSQL Connection Loss | `<<extend>>` (any data-access use case) | Implemented |
| UC-Exc-03: Login Failure Handling | N/A (entry point) | Implemented — verified against `AuthController` |
| UC-Exc-04: Refresh Token Invalid or Expired | N/A (entry point) | Implemented — verified against `AuthController`/Redis |

---