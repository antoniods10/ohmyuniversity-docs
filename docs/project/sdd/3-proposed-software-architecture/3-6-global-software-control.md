---
title: SDD - 3.6 Global Software Control | OhMyUniversity!
description: Definition of the global control flow mechanisms, including event-driven client interactions and thread-based backend concurrency for the OhMyUniversity! system.
head:
  - - meta
    - property: og:title
      content: SDD - 3.6 Global Software Control | OhMyUniversity!
  - - meta
    - property: og:description
      content: Definition of the global control flow mechanisms, including event-driven client interactions and thread-based backend concurrency for the OhMyUniversity! system.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-6-global-software-control
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, global software control, event-driven, thread-based, concurrency, control objects, architecture
  - - meta
    - name: twitter:title
      content: SDD - 3.6 Global Software Control | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Definition of the global control flow mechanisms, including event-driven client interactions and thread-based backend concurrency for the OhMyUniversity! system.
---

# OhMyUniversity! - SDD: 3.6 Global Software Control

## Overview of the Control Flow

The system adopts a hybrid control flow: **Event-Driven** on the client side, **Thread-Based** on
the backend side.

## Client-Side Control Flow (Event-Driven)

The client applications (Flutter for mobile, Angular for web) react to user-triggered events (e.g.
tapping to refresh a transcript). An internal dispatcher routes each event to the appropriate
handler, which issues an asynchronous REST call to `api-gateway` and updates the UI once a response
is received. The Angular web client manages this state with Signals rather than a rigid, pre-defined
sequence of operations (see SDD 1.2 Usability).

## Server-Side Control Flow (Thread-Based)

`api-gateway` and `api-core` use Spring's standard thread-per-request model: each incoming HTTP
request is handled by a thread from the application's thread pool, allowing multiple requests
(e.g. two different students syncing their transcripts) to proceed concurrently without blocking
each other. `api-fetcher`'s batch jobs run on their own schedule, independent of this request-driven
thread pool.

## Communication Between Layers

- **HTTP REST Requests:** a client event generates an HTTP request to `api-gateway`, which routes
  it to `api-core` (or `api-fetcher` for batch-trigger endpoints). The response flows back through
  the same path.
- **Cache-First Retrieval:** for Esse3-derived data, `api-core` checks Redis before calling Esse3
  (cache-aside, see 3.4). On a cache hit, no outbound call to Esse3 is made, reducing latency for
  the request.

## Concurrency and Shared Resources Management

Multiple threads inside `api-core` may operate on data belonging to different students
concurrently; this is the common case and requires no special coordination, since each student's
`OmuUser`, `UniversityConnection`, and Esse3-derived data are independent of one another.

The one area where concurrent writes to genuinely shared state could occur is the background sync
process: `CinecaSyncService` updates `CinecaSyncState` while, in principle, a user-triggered sync
could be requested for the same connection. This is handled at the database level by PostgreSQL's
standard transactional guarantees; no additional distributed locking mechanism has been introduced
beyond this.

> **Not applicable yet:** scenarios such as classroom seat booking or exam session booking, which
> would require explicit mutual-exclusion handling (e.g. pessimistic locking to prevent
> overbooking, per RAD FR-1.3.2), are described in the requirements but have no corresponding
> implementation today. A concurrency strategy for these cases will be defined once the Classroom
> service is connected and exam booking is implemented, rather than specified speculatively here.

## Concurrency Model per Module — Implemented (within `api-core`)

| Module | Primary Entities | Concurrency Model | Locking Strategy | Rationale |
|---|---|---|---|---|
| **Auth Module** | `OmuUser`, `UniversityConnection` | Independent | None (per-user data) | Each user's profile and connections are isolated; no contention between users. |
| **Esse3 Integration Module** | `ExamSession`/`ExamBooking` data **read** via Esse3 (consultation only: viewing available sessions, existing bookings, fees, profile) | Independent | None (per-user data) | Reading exam sessions, existing bookings, and fees is per-student and stateless; `api-core` proxies Esse3 and does not coordinate access across students for these read paths. |
| **Agenda Module** | `CalendarEvent`, `CalendarEventImport` | Independent | None (per-user data) | Each user's calendar is independent; imported university events are written by the Sync Module, not concurrently by users. |
| **Sync Module** | `CinecaSyncState` | Effectively serialized per connection | None (relies on PostgreSQL transactions) | Background sync for a given connection does not need to run concurrently with itself; no custom locking scheme has been introduced. |

**Threading Model (implemented):** each HTTP request to `api-gateway`/`api-core` is handled by a
thread from the Spring-managed thread pool. The scheduled Esse3 sync runs as a background task,
decoupled from user-facing request threads, so a long-running sync does not block other requests.

---

## Concurrency Model per Module — Roadmap (planned modules/services)

The functional requirements (RAD 3.2) call for additional capabilities that have not been
implemented yet (see SDD 3.2 for their planned home: the Classroom service, the Transportation
service, or future modules within `api-core`). The concurrency strategy for each was designed
ahead of implementation and is preserved here as a design decision to be honored when the
corresponding feature is built—**none of the rows below describe current system behavior.**

| # | Planned Module/Service | Primary Entities | Concurrency Model | Locking Strategy | Rationale |
|---|---|---|---|---|---|
| 4 | **Academic & Secretarial — new exam booking (write path)** | `ExamSession`, `ExamBooking` | Exclusive | Pessimistic | Critical shared resource. Multiple students attempt to book limited seats simultaneously. Pessimistic locking prevents overbooking and race conditions during peak booking windows. **Note:** consultation of existing sessions/bookings is already implemented (see "Implemented" table above); it is specifically the creation of a new booking with overbooking protection that is not yet built. |
| 4 | **Academic & Secretarial (read-heavy data)** | `Course`, `DigitalBadge`, `AdministrativeDocument` | Independent | None (mostly read-heavy) | Courses and badges are published infrequently and accessed for reading. Write operations are rare and non-concurrent. |
| 5 | **Classroom service** | `Classroom`, `ClassroomBooking` | Exclusive | Pessimistic | Classrooms are finite shared resources. Double-booking must be prevented. Pessimistic locking ensures only one booking per timeslot. |
| 5 | **Canteen module** | `CanteenMenu`, `MealOrder` | Mixed | Optimistic | Menu is read-heavy and shared. Individual meal orders are mostly independent per user. Optimistic locking is suitable for detecting rare conflicts on shared menu updates. |
| 5 | **Campus & Logistics (materials/attendance)** | `DidacticMaterial`, `AttendanceRecord` | Independent | None (per-course or per-user) | Didactic materials are published centrally and read frequently. Attendance records are per-student and mostly append-only. |
| 6 | **Portal/Chat module** | `NoticeboardItem`, `ChatMessage` | Independent | None (independent per-user messages) | Chat messages between users are independent. Noticeboard items are mostly read-heavy and published by administrators infrequently. |
| 7 | **Partner Convention module** | `PartnerOrganization`, `NoticeboardItem` (Promotions) | Independent | None (per-partner data) | Each partner manages their own organization profile and job postings independently. No shared resources requiring mutual exclusion. |

### Concurrency Design Decisions (Roadmap)

These decisions were defined as part of the original concurrency design and apply once the
corresponding features are implemented:

**Threading Model (planned):** as with the implemented modules, each HTTP request would be
handled by a separate thread from the thread pool. Long-running operations (e.g. batch email
notifications) are intended to be offloaded to background workers using scheduled tasks or message
queues, preventing them from blocking user-facing request threads.

**Multi-user Scalability (planned):** the system is designed to handle hundreds of concurrent
students. Each student's session is expected to run in its own thread context. Peak periods (e.g.
exam booking windows, course enrollment deadlines) would require sufficient thread pool capacity
(originally estimated at 2–4× the expected concurrent user count) to avoid bottlenecks and
maintain sub-second response times. This sizing has not been validated against real traffic, since
the booking features it applies to are not yet live.

**Decomposition & Parallelism (planned — exam booking example):** an exam booking request was
designed to involve: (1) authorization check against `OmuUser`/career data (independent, no lock),
(2) prerequisite validation against the study plan (independent, no lock), (3) seat availability
check against `ExamSession` (exclusive, pessimistic lock), and (4) final persistence of
`ExamBooking` (independent, no lock). Steps 1–2 can proceed concurrently; step 3 serializes at the
`ExamSession` lock; step 4 commits independently. This sequencing should be re-validated against
the actual data model at the time exam booking is implemented, rather than assumed unchanged.