---
title: SDD - 4. Subsystems Services | OhMyUniversity!
description: Comprehensive definition of the high-level services and operations provided for the first Agile Sprint, covering Academic Career and Orientation modules.
head:
  - - meta
    - property: og:title
      content: SDD - 4. Subsystems Services | OhMyUniversity!
  - - meta
    - property: og:description
      content: Comprehensive definition of the high-level services and operations provided for the first Agile Sprint, covering Academic Career and Orientation modules.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/4-subsystems-services/4-subsystems-services
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, subsystem services, academic career, orientation, high-level design
  - - meta
    - name: twitter:title
      content: SDD - 4. Subsystems Services | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Comprehensive definition of the high-level services and operations provided for the first Agile Sprint, covering Academic Career and Orientation modules.
---

### OhMyUniversity! - SDD: 4. Subsystems Services

This section defines the services provided by each module. According to system design heuristics, a service is defined as a set of related operations that share a common purpose and collectively form the module's interface. The operations are described at a high level, specifying their behavior, required parameters, and expected return values, without exposing the underlying internal implementation.

As established in SDD 3.2, "Academic & Secretarial Service" is not an independently deployed subsystem—it is a logical grouping of responsibilities within `api-core` (spanning the Auth and Esse3 Integration modules described in 3.2). The name is kept here for continuity with the RAD's functional modules, but it should be read as **module**, not as a separately deployable service.

_Following the adopted Agile methodology, this document is produced incrementally. For the first Sprint, we define the services related to "Module 1 (Academic Career)" and "Module 4 (Orientation)". The operations are intentionally described at a module level and avoid class-level implementation details._

## Current Sprint Scope (Traceability)

The table below records which RAD functional requirements are targeted by the current sprint, as
confirmed by the team, so that the status of each operation in this chapter can be traced back to
an actual sprint commitment rather than inferred from the codebase alone.

| RAD Module | FRs in current sprint | Notes |
|---|---|---|
| Module 1 (Career, Didactics, Secretarial) | FR-1.1.1, 1.1.2, 1.1.3, 1.1.4, 1.1.5, 1.1.6, **1.1.7 (in progress)**, 1.1.8, 1.1.9 | FR-1.1.7 (Exam Session Management — booking) is actively being implemented this sprint, not yet complete. FR-1.1.8 is covered by three distinct verified operations (fee status, invoices, refunds — `FeesService`), not a single generic "documents" operation. The codebase also exposes additional operations not individually named in the RAD (transcript, exam history, recommendations, surveys, avatar, career info) — see Service 1 below. |
| Module 2 (External University Services) | FR-1.2.2 (tentative), 1.2.3, 1.2.4 | FR-1.2.2 (Institutional Email) is tentatively in scope, not fully confirmed. FR-1.2.1 (Moodle) is out of scope (link-only, SDD 1.1). |
| Module 3 (Organization and Logistics) | FR-1.3.3, 1.3.4 | Classroom booking (FR-1.3.2), didactic material (FR-1.3.1), attendance (FR-1.3.5), maps/transport (FR-1.3.6), and canteen (FR-1.3.7/1.3.8) are out of scope for this sprint. |
| Module 4 (Orientation and Future Planning) | FR-1.4.6 | Only Orientation Guidance is in scope; Master's requirements/eligibility (FR-1.4.1/1.4.2), calls/opportunities (FR-1.4.3), job opportunities (FR-1.4.4), and official redirection (FR-1.4.5) are out of scope for this sprint. |

This chapter (4.1) covers Module 1 and Module 4 only, per the RAD-to-SDD traceability convention
described above; Module 2 and Module 3 services will be documented in a future revision of this
chapter once their owning module/service is defined.

#### 4.1 Academic & Secretarial Module

This module encapsulates the core business logic related to the student's academic life. It exposes services to retrieve and compute data regarding transcripts, exam sessions, and administrative documents. The operations below are grouped by implementation status.

**Service 1: Career, Didactics, and Secretarial Operations (RAD Module 1) — Implemented, verified against code**

These operations are backed by `api-core`'s Esse3 Integration module. As of this revision, they
have been verified directly against `CareerService.java`, `ExamsService.java`, `FeesService.java`,
and `ProfileService.java` rather than inferred from the codebase structure alone. Two caching
behaviors differ by service and are called out explicitly: `CareerService`, `ExamsService`, and
`ProfileService` fetch live from Esse3 on every call with **no local caching** (their own Javadoc
states "nothing is persisted locally"); `FeesService` data **is** cached in Redis (see SDD 3.4).

*Career (`CareerService` — no caching, live Esse3 calls):*

- `getTranscript(studentId: UUID): TranscriptResponse`
  - **Behavior:** Retrieves the full exam transcript (libretto) from Esse3, including status, grade, honors, and credits per exam.
  - **Return:** Returns the transcript as an ordered list of exam rows.
- `getGrades(studentId: UUID): GradesResponse`
  - **Behavior:** Retrieves grade averages from Esse3 (arithmetic, weighted, and base-110) and **computes**, in `api-core`, the count of passed/total exams and the percentage of CFU acquired from the transcript. The base-110 figure (closest equivalent to a "graduation projection") is returned as provided by Esse3, not computed by `api-core`.
  - **Return:** Returns arithmetic average, weighted average, base-110 average, passed/total exam counts, CFU acquired/total, and CFU percentage.
- `getStudyPlan(studentId: UUID): StudyPlanResponse`
  - **Behavior:** Retrieves the student's study plan from Esse3.
  - **Return:** Returns the study plan as an ordered list of courses, or an empty plan if none exists.
- `getExamHistory(studentId: UUID): ExamHistoryResponse`
  - **Behavior:** Retrieves the full exam attempt history from Esse3 (all attempts, past and future), grouped by teaching activity.
  - **Return:** Returns exams grouped with their attempt history (date, outcome, grade, withdrawal/absence status where applicable).
- `getRecommendations(studentId: UUID): RecommendationsResponse`
  - **Behavior:** Compares the study plan against the transcript, excludes already-passed activities, and ranks the remaining exams by a priority score based on course year and CFU. Computed in `api-core`.
  - **Return:** Returns an ordered list of recommended exams to take next.

*Exams (`ExamsService` — no caching, live Esse3 calls):*

- `getAvailableExamSessions(courseId: UUID): SessionsResponse`
  - **Behavior:** Returns the active exam sessions available for a specific teaching activity, from Esse3.
  - **Return:** Returns the available sessions (date, room, lecturer, whether already booked).
- `getBookableSessions(studentId: UUID): BookableSessionsResponse`
  - **Behavior:** Returns exam sessions the student is specifically eligible to book, from Esse3.
  - **Return:** Returns bookable sessions.
- `getBookings(studentId: UUID): BookingsResponse`
  - **Behavior:** Returns the student's active upcoming bookings, filtering out past, passed, or withdrawn exams.
  - **Return:** Returns active bookings.
- `getSurveys(studentId: UUID): SurveysResponse`
  - **Behavior:** Retrieves teaching evaluation surveys (questionari) from Esse3, split into pending and completed.
  - **Return:** Returns pending and completed surveys.

*Fees (`FeesService` — cached in Redis, see SDD 3.4):*

- `getFeeStatus(studentId: UUID): FeeStatusResponse`
  - **Behavior:** Retrieves aggregated tuition fee status (amount due, overdue/due items, charges) from Esse3, through the Redis cache.
  - **Return:** Returns the fee status summary.
- `getInvoices(studentId: UUID): InvoiceResponse`
  - **Behavior:** Retrieves issued invoices for the student from Esse3, through the Redis cache.
  - **Return:** Returns the list of invoices.
- `getRefunds(studentId: UUID): RefundResponse`
  - **Behavior:** Retrieves refunds for the student from Esse3, through the Redis cache.
  - **Return:** Returns the list of refunds.

*Profile (`ProfileService` — no caching, live Esse3 calls):*

- `getProfile(studentId: UUID): PersonaResponse`
  - **Behavior:** Retrieves the student's personal profile from Esse3. Falls back to a minimal profile (identity data only, from the JWT/session) if full profile data is not yet cached server-side for this session.
  - **Return:** Returns the full or minimal profile.
- `getCareerInfo(studentId: UUID): CareerInfoResponse`
  - **Behavior:** Retrieves career metadata (degree course, faculty, enrollment status, year) from Esse3.
  - **Return:** Returns the career metadata.
- `getAvatar(studentId: UUID): byte[]`
  - **Behavior:** Retrieves the profile photo as raw image bytes from Esse3.
  - **Return:** Returns the image bytes, if available.
- `getBadge(studentId: UUID): BadgeResponse`
  - **Behavior:** Retrieves the digital university badge for the student (FR-1.1.9) from Esse3.
  - **Return:** Returns the badge data, or none if not available.

**Service 1 (continued) — In Progress (current sprint)**

The following operation requires write access with concurrency control. As of this sprint, it is
**actively being implemented** (FR-1.1.7, included in the current Sprint scope below)—it is not
yet complete, but it is not a distant roadmap item either. The concurrency/overbooking strategy
described in SDD 3.6/3.7 is the target design for this work, not a stable, shipped behavior yet:

- `bookExamSession(studentId: UUID, sessionId: UUID): BookingConfirmation`
  - **Behavior (in progress):** Registers the student for a specific exam session and produces a confirmation of the outcome, enforcing mutual exclusion against the session's seat capacity.
  - **Return (in progress):** Returns a confirmation object for the booking operation.
  - **Status:** in active development this sprint. Verified against `ExamsService.java`: today it exposes only read operations (`getSessions`, `getBookableSessions`, `getBookings`, `getLegacyBookings`, `getSurveys`); no booking-creation method exists yet, not even in an early/partial form.

**Service 2: Orientation and Future Planning (RAD Module 4) — Implemented, but entirely client-side**

Unlike Service 1, this functionality does **not** live in `api-core` or any backend module. It is
implemented entirely in the web frontend, as a self-contained scoring engine operating on static
data bundled with the client (`ORIENTATION_TOPICS`, `UNIVERSITY_ORIENTATION_INFO`, and the
broader `UNIVERSITIES` dataset—see the Orientation feature's own architecture notes). It makes no
calls to `api-core` or any other backend service.

The operations below describe this functionality at the same high level as Service 1, for
traceability to RAD Module 4, but they should be read as **client-side logic**, not as a backend
subsystem interface:

- `searchAndCompareDegreeProgrammes(filters: ProgramSearchCriteria): List<DegreeProgramme>`
  - **Behavior:** Filters and ranks university/programme suggestions from the bundled dataset based on the student's answers, entirely in the browser.
  - **Return:** Returns a list of matching degree programmes.
- `getMastersRequirements(masterProgramId: UUID): RequirementSummary`
  - **Behavior:** Looks up the formal admission requirements for a selected Master's degree programme from static client-side data.
  - **Return:** Returns a summary of the required criteria.
- `getEnrollmentGuide(): List<Guide>`
  - **Behavior:** Returns bundled informational guides for enrollment and degree selection.
  - **Return:** Returns a list of informational guides.

> **`evaluateMasterEligibility(studentId, masterProgramId): EligibilitySummary`** — flagged here,
> not corrected: this operation computes an automatic eligibility result, which contradicts RAD
> FR-1.4.2 and the RAD Glossary, both of which state that OhMyUniversity displays Master's
> requirements **for consultation only**, with **no automatic eligibility verification** (the
> student manually checks their own status). The RAD's own Use Case Model (UC-12), however,
> describes an automatic eligibility percentage calculation—contradicting FR-1.4.2/Glossary. This
> is a pre-existing inconsistency in the RAD itself, not something introduced by this SDD. It
> should be resolved at the RAD level before this operation is specified one way or the other here.