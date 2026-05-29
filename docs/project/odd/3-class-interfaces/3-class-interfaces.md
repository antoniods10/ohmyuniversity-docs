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

This section details the solution classes and their public interfaces for the `api-core` microservice of the system. **We focus specifically on this microservice because it implements the functionalities of "Module 1: Career, Didactics, and Student Administration" and "Module 4: Orientation and Future Planning", which are the core deliverables selected for our first Agile Sprint.**

For each class, the general responsibility, dependencies, attributes, and public operations are specified, including strict contracts (pre-conditions, post-conditions, and exceptions) in compliance with Object Design guidelines and the Design by Contract principle.

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
