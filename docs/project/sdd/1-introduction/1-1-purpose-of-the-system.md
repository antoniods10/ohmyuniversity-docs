---
title: SDD - 1.1 Purpose of the System | OhMyUniversity!
description: OhMyUniversity is a multi-platform student portal that centralizes access to fragmented university data through a service-based backend, ensuring data integrity, performance, and scalability.
head:
  - - meta
    - property: og:title
      content: SDD - 1.1 Purpose of the System | OhMyUniversity!
  - - meta
    - property: og:description
      content: OhMyUniversity is a multi-platform student portal that centralizes access to fragmented university data through a service-based backend, ensuring data integrity, performance, and scalability.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-1-purpose-of-the-system
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, purpose, system design, microservices, architecture, centralization, scalability, spring boot, flutter, angular
  - - meta
    - name: twitter:title
      content: SDD - 1.1 Purpose of the System | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: System design overview of OhMyUniversity! distributed architecture, technology stack decisions, and strategic vision for the platform.
---

# OhMyUniversity! - SDD: 1.1 Purpose of the System

**OhMyUniversity!** is designed for **Italian university students** navigating the complex
landscape of institutional information systems. While initially targeting **Università degli studi del Molise (UNIMOL)**,
the system is architected to be portable across Italian universities that use CINECA/Esse3
as their academic data management platform. This document is intended for the **technical team
responsible for designing and implementing** the system, including architects, backend developers,
frontend developers, and DevOps engineers.

### The Problem

Today, students navigate multiple separate portals (Esse3, Moodle, email, library, canteen, etc.)
to access information spread across different systems. This fragmentation causes:

- **Manual re-entry** of data (grade averaging, progress calculation)
- **Inconsistent information** across systems
- **Cognitive overload**: students lose focus on studies due to administrative friction

### The Solution

OhMyUniversity! provides a **service-based backend** that aggregates data from institutional
platforms (primarily Cineca/Esse3) and exposes it through a unified, student-centric interface,
enabling:

- **Multi-Platform Access** (Web, Mobile) with a single authentication
- **Intelligent Automation** (data sync, automatic calculations, caching)
- **Independent Service Scaling** (new domains can be added as standalone services)
- **Strategic Tools** (Master's requirements tracking, career planning)

### Architectural Approach

OhMyUniversity! acts as an **advanced middleware layer** that orchestrates data from external
university systems (primarily Cineca/Esse3) into a unified, student-centric interface. The system
is **NOT a replacement** for institutional systems—it aggregates and normalizes data, allowing
students to interact with a single point of entry while maintaining integrity with official
university records.

The backend is composed of independent services rather than a single monolith. At the current
stage of development, three services are implemented and connected end-to-end:

- **api-core**: the central hub. It owns authentication, the Cineca/Esse3 integration, calendar
  and email features, and is the single point of publication of domain events.
- **api-gateway**: routes client requests to the appropriate backend service and enforces JWT-based
  request authentication at the edge.
- **api-fetcher**: a batch-oriented service that ingests external/public datasets (e.g. MUR
  statistics, professional orders registries, timetable sources) on a schedule, independent of
  user-driven traffic.

Additional domain services for **classroom booking** and **transportation** have already been
scaffolded as independent modules, following the same architectural pattern as `api-core`. They
are not yet wired into the frontend and are planned for integration in a future sprint; until
then, they are not part of the system's externally observable behavior.

Moodle is referenced by the system only as an **external link** surfaced in the "Portals" section
of the client applications—OhMyUniversity! does not synchronize or aggregate data from Moodle.