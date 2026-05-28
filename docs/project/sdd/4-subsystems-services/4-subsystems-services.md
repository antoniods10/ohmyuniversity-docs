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

This section defines the services provided by each subsystem. According to system design heuristics, a service is defined as a set of related operations that share a common purpose and collectively form the subsystem's interface. The operations are described at a high level, specifying their behavior, required parameters, and expected return values, without exposing the underlying internal implementation.

_Following the adopted Agile methodology, this document is produced incrementally. For the first Sprint, we define the services related to "Module 1 (Academic Career)" and "Module 4 (Orientation)", which are architecturally managed by the "Academic & Secretarial Service" subsystem. The operations are intentionally described at a subsystem level and avoid class-level implementation details._

#### 4.1 Academic & Secretarial Service

This subsystem encapsulates the core business logic related to the student's academic life. It exposes services to retrieve and manipulate data regarding transcripts, exam sessions, administrative documents, and future degree planning.

**Service 1: Career, Didactics, and Secretarial Operations (RAD Module 1)**
This service exposes the operations necessary for managing academic progress, the official study plan, exam booking, and the virtual secretarial office.

- `syncAcademicCareer(studentId: UUID): boolean`
  - **Behavior:** Triggers a synchronization of the student's academic record with the external university platform.
  - **Return:** Returns whether the synchronization completed successfully.
- `getCalculatedAverages(studentId: UUID): AverageSummary`
  - **Behavior:** Computes the student's current academic averages from the registered exam results.
  - **Return:** Returns a summary of the calculated averages.
- `getGraduationProjection(studentId: UUID): float`
  - **Behavior:** Calculates the projected graduation score based on the current academic career.
  - **Return:** Returns the projected score.
- `getCareerProgress(studentId: UUID): ProgressSummary`
  - **Behavior:** Evaluates progression by comparing completed exams and credits against the expected study path.
  - **Return:** Returns a summary of the career progress.
- `getStudyPlan(studentId: UUID): List<Course>`
  - **Behavior:** Retrieves the list of compulsory and optional courses associated with the student's degree program.
  - **Return:** Returns the study plan as an ordered list of courses.
- `getAvailableExamSessions(courseId: UUID): List<ExamSession>`
  - **Behavior:** Returns the active exam sessions available for a specific course.
  - **Return:** Returns the available exam sessions.
- `bookExamSession(studentId: UUID, sessionId: UUID): BookingConfirmation`
  - **Behavior:** Registers the student for a specific exam session and produces a confirmation of the outcome.
  - **Return:** Returns a confirmation object for the booking operation.
- `getAdministrativeDocuments(studentId: UUID): List<Document>`
  - **Behavior:** Retrieves the official administrative and support documents relevant to the student's career.
  - **Return:** Returns the available documents and resources.

**Service 2: Orientation and Future Planning (RAD Module 4)**
This forecasting and orientation service supports students in future academic planning, evaluating their eligibility for master's degree programs, and providing institutional guidelines.

- `searchAndCompareDegreeProgrammes(filters: ProgramSearchCriteria): List<DegreeProgramme>`
  - **Behavior:** Allows prospective students to browse available degree programs by area of interest, location, and admission constraints.
  - **Return:** Returns a list of matching degree programmes.
- `getMastersRequirements(masterProgramId: UUID): RequirementSummary`
  - **Behavior:** Retrieves the formal admission requirements for a selected Master's degree program.
  - **Return:** Returns a summary of the required criteria.
- `evaluateMasterEligibility(studentId: UUID, masterProgramId: UUID): EligibilitySummary`
  - **Behavior:** Compares the student's current academic profile with the admission requirements of the selected Master's degree.
  - **Return:** Returns a summary of the eligibility result and missing requirements.
- `getEnrollmentGuide(): List<Guide>`
  - **Behavior:** Retrieves support resources and step-by-step guidance for enrollment and degree selection.
  - **Return:** Returns a list of informational guides.
