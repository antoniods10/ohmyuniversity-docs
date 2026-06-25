---
title: SDD - 3.1 Proposed Software Architecture | OhMyUniversity!
description: Overview and table of contents for the proposed software architecture section, covering subsystem decomposition, hardware-software mapping, data management, security, and boundary conditions.
head:
  - - meta
    - property: og:title
      content: SDD - 3.1 Proposed Software Architecture | OhMyUniversity!
  - - meta
    - property: og:description
      content: Overview and table of contents for the proposed software architecture section.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-1-overview
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, software architecture, design, subsystems, layers
  - - meta
    - name: twitter:title
      content: SDD - 3.1 Proposed Software Architecture | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Overview and table of contents for the proposed software architecture section.
---

# OhMyUniversity! - SDD: 3.1 Overview

## Overview

This section presents the **proposed software architecture** for the OhMyUniversity! system. The
architecture has two distinct levels of decomposition, and the rest of this chapter keeps them
explicitly separate:

- **Service level (deployment):** the system is composed of independent, separately deployed
  services. Three are implemented and connected end-to-end today—`api-core`, `api-gateway`,
  `api-fetcher`—and two additional domain services (Classroom booking, Transportation) have been
  scaffolded as independent modules but are not yet wired into the frontend (see SDD 1.1).
- **Module level (internal decomposition):** within `api-core`, responsibilities are further
  organized into cohesive logical modules (e.g. Auth, Academic/Career, Campus, Portal, Partner).
  These modules are **not** separately deployed services; they are packages within the same
  Spring Boot application, sharing the same process, database connection pool, and Kafka producer.

The architecture follows a multi-tier pattern (Presentation, Application Logic, Data & Caching,
External Integration) to separate concerns, built on the engineering principles of high cohesion
and low coupling described in 3.2.

---

## Table of Contents

### 3.2 Subsystem Decomposition

Describes the decomposition of `api-core` into cohesive, loosely coupled logical modules, their
responsibilities, domain classes, and how they relate to the deployed services (`api-core`,
`api-gateway`, `api-fetcher`, and the scaffolded Classroom/Transportation services). Includes the
Component Diagram showing static module relationships.

### 3.3 Hardware-Software Mapping

Presents the mapping between the logical software architecture and the physical/deployment
environment as it exists today, separating what is currently running from infrastructure that is
planned but not yet implemented (cloud deployment, orchestration).

### 3.4 Persistent Data Management

Covers the data persistence strategy: what is stored in PostgreSQL, what is cached in Redis and
why, and what is planned for MongoDB once the chat feature is implemented.

### 3.5 Access Control and Security

Describes the authentication mechanism actually in place today (institutional credentials verified
against Esse3), the roles currently able to authenticate, and which role-based capabilities are
implemented versus planned.

### 3.6 Global Software Control

Details the global control flow: event-driven client interactions and thread-based backend
concurrency, expressed in terms of the modules defined in 3.2.

### 3.7 Boundary Conditions

Addresses the system's behavior at boundary conditions, including startup, shutdown, restart, and
recovery procedures.