---
title: SDD - 3.4 Persistent Data Management | OhMyUniversity!
description: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
head:
  - - meta
    - property: og:title
      content: SDD - 3.4 Persistent Data Management | OhMyUniversity!
  - - meta
    - property: og:description
      content: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-system/3-4-persistent-data-management
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, persistent data, postgresql, redis, caching, fault tolerance, agile, database, gdpr, ttl
  - - meta
    - name: twitter:title
      content: SDD - 3.4 Persistent Data Management | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Strategy for persistent data management in OhMyUniversity!, detailing the use of PostgreSQL and Redis Cache to ensure data integrity, performance, and fault tolerance.
---

# OhMyUniversity! - SDD: 3.4 Persistent Data Management

## Overview and Strategy

The persistent data management strategy for OhMyUniversity! is designed to satisfy the strict non-functional requirements of **Reliability (Fault Tolerance)** and **Performance**, as defined in the Requirements Analysis Document (RAD). Since the system acts primarily as an aggregator of external services rather than a replacement for official university systems, the persistence layer adopts a hybrid approach. It separates the storage of native platform data from the caching of external academic records.

## 1. Relational Database Management System (PostgreSQL)

A RDBMS has been chosen to guarantee high data integrity, handle concurrent accesses, and provide ACID transactions for the core operations of the app.

- **What is stored:** Only the data generated natively within the OhMyUniversity! platform. This includes `UniversityUser` profiles, `UserSession` data, `PartnerOrganization` details, `NoticeboardItem` (news, job offers, and conventions), and `ChatMessage` objects.
- **What is NOT stored:** Official academic records (such as grades, study plans, or tuition fees). The system does not duplicate the source of truth, which remains strictly within the external Cineca/Esse3 systems.

## 2. In-Memory Caching System (Redis)

To ensure that the system remains usable even during external API downtimes (satisfying the fallback requirement and the offline status signaling), an advanced caching layer is implemented.

- **What is stored:** Temporary JSON representations of external data retrieved from Cineca and Moodle (e.g., `AcademicCareer`, `ExamSession`, `ClassroomAvailability`, `DidacticMaterial`).
- **Purpose:** Redis acts as a high-performance buffer. It drastically reduces the number of API calls made to the university's legacy systems, lowers response times, and allows the application to serve the "last known good state" to the user in offline mode.
- **Data Expiration (TTL):** All sensitive academic data cached from external APIs is assigned a strict Time-To-Live (TTL) of 1 hour. This balances high performance with GDPR Article 32 compliance by minimizing the exposure window of student records in the event of memory compromise.

## 3. Flat Files and Object Storage (Agile Evolution)

The multi-layer architecture is designed to be highly adaptable and scalable. Should future sprints require the management of flat files, the architecture will seamlessly integrate a dedicated Object Storage subsystem (e.g., AWS S3 or a local File System), and this document will be iteratively updated to reflect the new requirements.
