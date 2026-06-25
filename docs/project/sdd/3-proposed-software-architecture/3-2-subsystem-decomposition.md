---
title: SDD - 3.2 Subsystem Decomposition | OhMyUniversity!
description: Decomposing the OhMyUniversity! system into manageable, highly cohesive, and loosely coupled subsystems based on a multi-layer architecture.
head:
  - - meta
    - property: og:title
      content: SDD - 3.2 Subsystem Decomposition | OhMyUniversity!
  - - meta
    - property: og:description
      content: Decomposing the OhMyUniversity! system into manageable, highly cohesive, and loosely coupled subsystems based on a multi-layer architecture.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-2-subsystem-decomposition
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, subsystem decomposition, architecture, high cohesion, low coupling, api gateway, three-tier, layers
  - - meta
    - name: twitter:title
      content: SDD - 3.2 Subsystem Decomposition | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Decomposing the OhMyUniversity! system into manageable, highly cohesive, and loosely coupled subsystems based on a multi-layer architecture.
---

# OhMyUniversity! - SDD: 3.2 Subsystem Decomposition

## Architectural Approach and Heuristics

The OhMyUniversity! backend is decomposed at two levels, kept explicitly distinct throughout this
section:

1. **Services** (independently deployed): `api-core`, `api-gateway`, `api-fetcher` are implemented
   and connected end-to-end. A Classroom service and a Transportation service have already been
   scaffolded as independent modules following the same pattern as `api-core`, but are not yet
   wired into the frontend and are not part of the system's current externally observable
   behavior.
2. **Logical modules** (within `api-core`): to manage complexity inside `api-core` itself, its
   responsibilities are grouped into cohesive modules. These are packages in the same Spring Boot
   application—not separate processes, not independently deployable, and not independently
   scalable.

The primary engineering goal of this decomposition is to manage complexity by ensuring **high
cohesion** and **low coupling**.

- **High Cohesion:** elements within the same module are strongly related and collaborate toward a
  common purpose—for instance, everything related to the Esse3 career integration (career, exams,
  fees, profile) is grouped together.
- **Low Coupling:** `api-gateway` is the single entry point for client applications, which never
  address backend services directly. Within `api-core`, the Esse3 integration is itself isolated
  behind a dedicated client layer (`cineca/esse3/*`), so that the rest of the application never
  calls third-party APIs directly.

## Service-Level Decomposition

| Service | Status | Responsibility |
|---|---|---|
| **api-gateway** | Implemented, connected | Single entry point for client applications. Routes requests, enforces JWT-based authentication at the edge. |
| **api-core** | Implemented, connected | Central hub: authentication, Esse3 integration, calendar, email, and the single publisher of domain events to Kafka. |
| **api-fetcher** | Implemented, connected | Batch ingestion of external/public datasets (MUR statistics, professional orders, timetable sources), independent of user-driven traffic. |
| **Classroom service** | Scaffolded, not yet integrated | Intended to own classroom availability and booking. Not reachable from the frontend in the current sprint. |
| **Transportation service** | Scaffolded, not yet integrated | Intended to own maps/transportation features. Not reachable from the frontend in the current sprint. |

## Logical Module Decomposition (within `api-core`)

| # | Module | Responsibility | Domain Classes | FR Covered |
|---|---|---|---|---|
| **1** | **Auth Module** | Verifies institutional credentials against Esse3, issues the OMU JWT and refresh token, creates/links the local `OmuUser` profile on first login (linked across careers via fiscal code). | `OmuUser`, `UniversityConnection` | NFR Authentication (see 3.5) |
| **2** | **Esse3 Integration Module** | Isolates all outbound calls to Esse3 (career, exams, fees, profile) behind a dedicated client layer; normalizes responses into DTOs for the rest of the application. Covers **consultation/read paths**: career sync, grades, ECTS, viewing exam sessions and existing bookings, fees, profile. Does **not** yet cover creating a new exam booking with overbooking protection (see SDD 3.6 roadmap). | (Esse3 client/DTO classes) | FR-1.1.1 to FR-1.1.6, FR-1.1.8, FR-1.1.9, FR-1.2.4 (read paths); FR-1.1.7 partially (consultation only) |
| **3** | **Agenda Module** | Manages the integrated calendar: imported university events plus user-created events. The calendar is shared across all of a student's university enrollments—events are tied to the OMU user identity, not to a specific career, so switching active career (via `switch-carriera`) does not change which calendar events are visible. | `CalendarEvent` (personal), `UniversityEvent` (university-published), `CalendarEventImport` (bridge tracking which university events a student has imported, with a uniqueness constraint preventing duplicate imports), `CalendarEventType` | FR-1.3.4 |
| **4** | **Email Module** | Provides access to the institutional email inbox and sending capability. | (Email DTOs) | FR-1.2.2 |
| **5** | **Sync Module** | Runs the scheduled background synchronization with Esse3, independent of any single user session; publishes domain events to Kafka when new data is discovered. | `CinecaSyncState` | RAD 3.3.2 (Reliability) |

> **Not yet implemented as `api-core` modules:** classroom booking and transportation logic live
> in the scaffolded services listed above, not inside `api-core`. A chat module and a
> partner/conventions module are part of the functional requirements (RAD FR-1.5.3, FR-1.6.1,
> FR-1.6.2) but have no corresponding implementation yet; they are listed here for traceability
> only and should not be read as existing code.
>
> | Planned module | Responsibility (per RAD) | FR Covered |
> |---|---|---|
> | Portal/Chat Module | University news board, quick access configuration, chat between users | FR-1.5.1, FR-1.5.2, FR-1.5.3 |
> | Partner Convention Module | Partner registration requests, admin provisioning, conventions/job postings | FR-1.6.1, FR-1.6.2 |

## UML Component Diagram

> **[In progress]** — to be redrawn to reflect the service/module split above (the previous
> diagram depicted the logical modules as if they were independently deployed services).