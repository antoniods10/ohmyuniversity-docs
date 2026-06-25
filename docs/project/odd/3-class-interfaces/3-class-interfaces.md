---
title: ODD - 3. Class Interfaces | OhMyUniversity!
description: Specification of the public interfaces, dependencies, and contracts of the solution objects for the OhMyUniversity! system, focusing on Sprint 1 modules and the api-core microservice.
head:
  - - meta
    - property: og:title
      content: ODD - 3. Class Interfaces | OhMyUniversity!
  - - meta
    - property: og:description
      content: Specification of the public interfaces, dependencies, and contracts of the solution objects for the OhMyUniversity! system, focusing on Sprint 1 modules and the api-core microservice.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/3-class-interfaces/3-class-interfaces
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, class interfaces, controllers, services, dto, adapter pattern, cineca, api-core
  - - meta
    - name: twitter:title
      content: ODD - 3. Class Interfaces | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Specification of the public interfaces, dependencies, and contracts of the solution objects for the OhMyUniversity! system, focusing on Sprint 1 modules and the api-core microservice.
---

# OhMyUniversity! - ODD: 3. Class Interfaces

This section details the solution classes and their public interfaces for the backend services of the system. The current backend implementation includes `api-gateway`, `api-core`, and `api-fetcher`.

The `api-gateway` exposes the public API routes used by Web and Mobile clients. The `api-core` service implements the main application logic for authentication, academic career, calendar, email, and external university services. The `api-fetcher` service implements external data retrieval for timetables, statistics, and related institutional data.

For each class, the general responsibility, dependencies, attributes, and public operations are specified, including strict contracts (pre-conditions, post-conditions, and exceptions) in compliance with Object Design guidelines and the Design by Contract principle.

For the Angular Web frontend, class interfaces are represented mainly by standalone components, route guards, HTTP interceptors, facades, repositories, models, and feature services. The most relevant Web interfaces are concentrated in the `auth`, `dashboard`, and `orientation` features, because they support the selected deliverable modules.

For the Flutter Mobile frontend, class interfaces are represented mainly by pages, providers, widgets, entities, repositories, use cases, models, data sources, and feature services. The most relevant Mobile interfaces are concentrated in the `auth`, `academics`, `calendar`, `services`, `orientation`, `email`, `profile`, and `timetable` features.

---

## Packages Overview

This chapter is organized into 4 main packages:

**3.1 Controller** — REST API endpoints (AuthController, CarrieraController)

**3.2 Service** — Business logic layer (AuthService, CarrieraService, OmuJwtService)

**3.3 DTO** — Data Transfer Objects for client communication

**3.4 Cineca** — External integration gateway to ESSE3 (Adapter pattern)

---

## Architecture Summary

The `api-core` microservice follows a **layered architecture** with clear separation of concerns:

- **Controller Layer** : Exposes HTTP REST endpoints, handles request/response mapping, delegates to services
- **Service Layer** : Implements business logic, orchestrates domain operations, manages authentication flows
- **DTO Layer** : Defines data contracts for client communication, hides internal complexity
- **Adapter Layer** : Isolates external system integration (ESSE3) using Adapter pattern, manages session lifecycle

All classes follow the **Design by Contract** principle, with explicit pre-conditions, post-conditions, and exception specifications for each operation.

---

## Angular Web Frontend Interfaces

The Web frontend exposes its object interfaces mainly through Angular components, guards, interceptors, facades, repositories, models, and services.

The `auth` feature contains login-related pages, forms, use cases, facades, repositories, and API adapters.

The `dashboard` feature contains the protected student area, including career, exams, calendar, timetable, portals, secretariat, profile, contacts, and related widgets.

The `orientation` feature contains the public orientation experience, with application state, domain models, pages, charts, topic components, and result components.

The `core/guards` package controls route access, while `core/interceptors` centralizes HTTP authentication behavior.

---

## Flutter Mobile Frontend Interfaces

The Mobile frontend exposes its object interfaces mainly through pages, providers, widgets, entities, repositories, use cases, models, data sources, and feature services.

The `auth`, `academics`, `calendar`, `services`, `orientation`, `email`, `profile`, and `timetable` features follow a layered organization based on `data`, `domain`, and `presentation`.

The `data` layer contains data sources, models, and repository implementations. The `domain` layer contains entities, repository contracts, use cases, exceptions, and domain services where required. The `presentation` layer contains pages, providers, widgets, views, utilities, and mappers used by the Flutter interface.

Public route constants remain centralized in the mobile route configuration and are kept stable even when feature package names are refactored.

