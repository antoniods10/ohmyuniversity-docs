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
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-system/3-2-subsystem-decomposition
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

The OhMyUniversity! system has been decomposed into subsystems following a layered architecture based on the Multi-layer / 3-tier pattern. The architecture is logically divided into Presentation, Application Logic, Caching & Database, and External Services layers.

The primary engineering goal of this decomposition is to manage system complexity by ensuring **high cohesion** and **low coupling** among modules.

- **High Cohesion:** Elements within the same subsystem are strongly related and collaborate towards a common, specific purpose. For instance, functionalities related to the academic career, exams, and degree planning have been grouped into a single highly cohesive subsystem.
- **Low Coupling:** Subsystems are designed to be as independent as possible, minimizing the dependencies and interactions across module boundaries. To achieve this, the architecture introduces a **Main API Gateway** within the application logic: the client applications do not need to know the specific addresses of the internal services, as the Gateway acts as a single entry point and routes the requests.

Furthermore, we applied key software engineering heuristics, such as grouping by use cases and actor responsibilities. For instance, we separated the `Portal & Social Service` (dedicated to student interactions and university news) from the `Partner Convention Service` (dedicated to corporate partners and job offers). Finally, to avoid a direct, strongly coupled access to third-party APIs (like CINECA, Esse3, or Moodle), an `External Integration Gateway` acts as an external proxy.

## Subsystem Decomposition Table

| #     | Subsystem                          | Layer (Tier)       | Responsibility                                                                                                                                                     | Domain Classes                                                                                                                                                                                           | FR Covered                                                        |
| :---- | :--------------------------------- | :----------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------- |
| **1** | **Presentation & UI App**          | Presentation Layer | Manages the multi-platform user interface (Mobile, Web, Desktop), intuitive dashboards, and captures input from both students and partners.                        | (All _Boundary / View_ classes)                                                                                                                                                                          | All FRs (Visual access point)                                     |
| **2** | **Main API Gateway**               | Application Logic  | Acts as the single entry point for the frontend, routing incoming REST requests to the appropriate internal services.                                              | (All _Gateway / Router_ classes)                                                                                                                                                                         | _N/A (Infrastructure & Routing)_                                  |
| **3** | **Auth & User Session Service**    | Application Logic  | Handles secure user authentication via government gateways (SPID/CIE), RBAC controls, and maintains active sessions.                                               | `UniversityUser`, `Student`, `UserSession`                                                                                                                                                               | _N/A (Satisfies security non-functional requirements)_            |
| **4** | **Academic & Secretarial Service** | Application Logic  | Calculates grade averages, tracks CFUs, projects base graduation grades, manages exam bookings, notices, tuition fees, and evaluates Master's degree requirements. | `AcademicCareer`, `Exam`, `StudyPlan`, `Course`, `ExamSession`, `ExamBooking`, `AdministrativeDocument`, `TuitionFee`, `DigitalBadge`, `MasterDegreeProgram`, `AdmissionRequirement`, `OrientationGuide` | FR-1.1.1 to 1.1.9, FR-1.2.4, FR-1.4.1 to 1.4.3                    |
| **5** | **Campus & Logistics Service**     | Application Logic  | Manages didactic materials, classroom availability and bookings, integrated calendar, maps/transportation, attendance, and canteen menus.                          | `Classroom`, `ClassroomBooking`, `CanteenMenu`, `MealOrder`, `DidacticMaterial`, `CalendarEvent`, `AttendanceRecord`                                                                                     | FR-1.3.1 to 1.3.8                                                 |
| **6** | **Portal & Social Service**        | Application Logic  | Aggregates University News, manages quick access configurations, and provides the internal chat system for students.                                               | `NoticeboardItem`, `ChatMessage`                                                                                                                                                                         | FR-1.5.1, FR-1.5.2, FR-1.5.3                                      |
| **7** | **Partner Convention Service**     | Application Logic  | Manages partner organization registrations and provides private dashboards for corporate partners to publish conventions and job offers.                           | `PartnerOrganization`, `NoticeboardItem` (Promos)                                                                                                                                                        | FR-1.6.1, FR-1.6.2                                                |
| **8** | **External Integration Gateway**   | External Services  | Acts as a Proxy/Adapter orchestrating API calls to third-party systems (Cineca, Moodle, Map Providers) and ensuring proper data formatting.                        | (All _Adapter / Facade_ classes for external systems)                                                                                                                                                    | FR-1.2.1, FR-1.2.2, FR-1.2.3                                      |
| **9** | **Data & Caching Layer**           | Caching & Database | Manages local data persistence and caching (e.g., Redis) to provide high fault tolerance in the absence of network connectivity.                                   | (All _Entity_ classes saved locally / Cache)                                                                                                                                                             | _N/A (Satisfies NFR-3.3.2 Reliability and NFR-3.3.3 Performance)_ |
