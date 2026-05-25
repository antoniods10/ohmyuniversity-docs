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

The global software control defines the sequence of actions within the system and how it reacts to external stimuli. Due to the distributed nature of the _OhMyUniversity!_ architecture, a single monolithic control mechanism is insufficient. Therefore, the system adopts a hybrid control flow strategy: **Event-Driven** for the Presentation Layer and **Thread-Based** for the Application Logic and Backend Services.

## Client-Side Control Flow (Event-Driven)

The client applications (Flutter for mobile, Angular for web) act as the primary boundary objects interacting with the users. These interfaces operate entirely on an **Event-Driven** control mechanism.

- **Mechanism:** The application continuously runs a main event loop awaiting external stimuli (e.g., a student tapping the "Book Exam Session" button or using a pull-to-refresh swipe gesture to update their transcript).
- **Dispatcher:** When an event occurs, an internal dispatcher captures it and routes it to the appropriate Event Handler (Controller/ViewModel) associated with that specific UI component.
- **Execution:** The system does not execute a rigid, pre-defined sequence of operations (as in procedure-driven systems); instead, it reacts dynamically, sending asynchronous REST API calls to the backend and updating the UI state once the academic data response is received.

## Server-Side Control Flow (Thread-Based)

The Spring Boot microservices residing in the Business Logic layer must handle thousands of simultaneous interactions (e.g., hundreds of students checking their grades or booking exam sessions simultaneously during peak periods). To achieve this, the backend adopts a **Thread-Based** control mechanism.

- **Mechanism:** The API Gateway and the internal microservices utilize a highly concurrent model. Upon receiving an HTTP request (event), the server allocates a separate execution thread to process that specific request.
- **Parallelism:** This allows multiple execution flows to proceed in parallel. For instance, the system can actively fetch transcript data from Cineca for User A, while simultaneously processing a new exam booking request for User B, without blocking the overall execution of the application.

## Communication Between Layers

The Event-Driven client and Thread-Based server interact through a structured communication protocol:

- **HTTP REST Requests:** When a user event occurs on the client (e.g., clicking "View My Transcript"), the event handler generates an HTTP request to the Main API Gateway. The server receives this request and assigns it to an available thread from the thread pool for processing. Once the response is generated, it is sent back to the client, and the UI state is updated accordingly.

- **Cache-Local Retrieval:** If the requested data is already present in the local cache (handled by the **Data & Caching Layer** subsystem), the Event Handler retrieves it directly from the cache without issuing a remote HTTP request. This bypasses server-side thread allocation and significantly reduces latency. For example, if a student has recently viewed their courses list, the client can display cached course data immediately while optionally synchronizing with the backend in the background. This mechanism is critical for offline resilience and performance during peak periods.

## Concurrency and Shared Resources Management

The thread-based backend introduces the challenge of concurrency. Multiple threads may attempt to access or modify the same domain entities simultaneously.

- **Independent Objects:** Whenever possible, objects that do not interact with each other are assigned to different control objects to maximize parallel execution. For instance, each student's `AcademicCareer` and `UserSession` are independent—one student's transcript fetch does not contend with another's exam booking.

- **Exclusive Access (Mutually Exclusive Activities):** For shared resources, such as a specific `ExamSession` entity, the system enforces exclusive access. The Control Objects manage transactions using pessimistic or optimistic locking mechanisms at the PostgreSQL database level to prevent race conditions (e.g., preventing the system from overbooking an exam session that has a strict maximum seat capacity).
  - **Pessimistic Locking:** Acquires locks on resources before accessing them and holds locks until the transaction commits. Best for high-contention scenarios (e.g., popular exam session bookings during peak windows). Guarantees no conflicts but reduces throughput if threads frequently wait for locks to become available.
  - **Optimistic Locking:** Allows concurrent reads and writes without explicit locks; validates changes at commit time using version numbers or timestamps. Suitable for lower-contention scenarios. More scalable and reduces blocking, but requires retry logic if conflicts are detected during commit.

## Subsystem-Level Concurrency Model

The OhMyUniversity! architecture comprises 9 subsystems, each with a distinct concurrency strategy based on the nature of its data and the frequency of access patterns. The following table formalizes the concurrency model for each subsystem:

| #   | Subsystem                          | Primary Entities                                      | Concurrency Model | Locking Strategy                     | Rationale                                                                                                                                                                           |
| --- | ---------------------------------- | ----------------------------------------------------- | ----------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3   | **Auth & User Session Service**    | `UniversityUser`, `UserSession`, `UniversityRole`     | Independent       | None (per-user data)                 | Each user's session is isolated; no contention between users. Read-after-write consistency is sufficient.                                                                           |
| 4   | **Academic & Secretarial Service** | `AcademicCareer` (per-user), `StudyPlan` (per-user)   | Independent       | None (per-user data)                 | Each student's academic record is independent. No conflicts between different students' transcripts or study plans.                                                                 |
| 4   | **Academic & Secretarial Service** | `ExamSession`, `ExamBooking`                          | Exclusive         | Pessimistic                          | Critical shared resource. Multiple students attempt to book limited seats simultaneously. Pessimistic locking prevents overbooking and race conditions during peak booking windows. |
| 4   | **Academic & Secretarial Service** | `Course`, `DigitalBadge`, `AdministrativeDocument`    | Independent       | None (mostly read-heavy)             | Courses and badges are published infrequently and accessed for reading. Write operations are rare and non-concurrent.                                                               |
| 5   | **Campus & Logistics Service**     | `Classroom`, `ClassroomBooking`                       | Exclusive         | Pessimistic                          | Classrooms are finite shared resources. Double-booking must be prevented. Pessimistic locking ensures only one booking per timeslot.                                                |
| 5   | **Campus & Logistics Service**     | `CanteenMenu`, `MealOrder`                            | Mixed             | Optimistic                           | Menu is read-heavy and shared. Individual meal orders are mostly independent per user. Optimistic locking suitable for detecting rare conflicts on shared menu updates.             |
| 5   | **Campus & Logistics Service**     | `DidacticMaterial`, `AttendanceRecord`                | Independent       | None (per-course or per-user)        | Didactic materials are published centrally and read frequently. Attendance records are per-student and mostly append-only.                                                          |
| 6   | **Portal & Social Service**        | `NoticeboardItem`, `ChatMessage`                      | Independent       | None (independent per-user messages) | Chat messages between users are independent. Noticeboard items are mostly read-heavy and published by administrators infrequently.                                                  |
| 7   | **Partner Convention Service**     | `PartnerOrganization`, `NoticeboardItem` (Promotions) | Independent       | None (per-partner data)              | Each partner manages their own organization profile and job postings independently. No shared resources requiring mutual exclusion.                                                 |

## Concurrency Design Decisions for OhMyUniversity!

**Threading Model:** Each HTTP request to the API Gateway is handled by a separate thread from the thread pool. Long-running operations (e.g., CINECA synchronization, batch email notifications) are offloaded to background workers using message queues or scheduled tasks, preventing them from blocking user-facing request threads.

**Multi-user Scalability:** The system is designed to handle hundreds of concurrent students. Each student's session runs in its own thread context. Peak periods (e.g., exam booking windows, course enrollment deadlines) require sufficient thread pool capacity (typically sized at 2–4× the expected concurrent user count) to avoid bottlenecks and maintain sub-second response times.

**Decomposition & Parallelism:** Requests can be parallelized internally when multiple independent operations are required. For example, an exam booking request involves: (1) authorization check against `UniversityUser` (independent, no lock), (2) prerequisite validation against `StudyPlan` (independent, no lock), (3) seat availability check against `ExamSession` (exclusive, pessimistic lock), and (4) final persistence of `ExamBooking` (independent, no lock). Steps 1–2 can proceed concurrently; step 3 serializes at the `ExamSession` lock; step 4 commits independently.
