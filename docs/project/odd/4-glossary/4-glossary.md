---
title: ODD - Glossary | OhMyUniversity!
description: Glossary of Object-Oriented Design concepts, architectural patterns, and coding standards used in the OhMyUniversity! Object Design Document (ODD).
head:
  - - meta
    - property: og:title
    - content: ODD - Glossary | OhMyUniversity!
  - - meta
    - property: og:description
    - content: Definitions of patterns, data structures, and code-level implementation rules for OhMyUniversity!.
  - - meta
    - property: og:url
    - content: https://docs.university.ohmyopensource.org/project/odd/4-glossary/4-glossary
  - - meta
    - name: keywords
    - content: ohmyuniversity, odd, glossary, design by contract, design pattern, dto, clean architecture
  - - meta
    - name: twitter:title
    - content: ODD - Glossary | OhMyUniversity!
  - - meta
    - name: twitter:description
    - content: Glossary of the OhMyUniversity Object Design Document.
---

# OhMyUniversity! - ODD: Glossary

This glossary defines the development patterns, object-oriented design principles, and coding conventions that the development team must apply when implementing OhMyUniversity!.

---

## Design Patterns

### **Design by Contract**

Rigorous software design methodology adopted in the project. It requires every public method to expose an explicit contract consisting of _pre-conditions_ (input requirements), _post-conditions_ (output guarantees), and _class invariants_.

### **Adapter Pattern**

Structural pattern used by the `cineca` layer to isolate the system from the technical details of legacy ESSE3 APIs. It hides the complexity of external communications and transforms responses into standard internal formats.

### **Proxy Pattern (Lazy Loading)**

Strategy implemented to avoid loading heavy dependencies or payloads (such as documents or entire lesson contents) during the first network call. Detailed data is deferred (Lazy Loading) and downloaded from university APIs only when explicitly requested by the user.

### **Object Composition**

Design principle favoring composition over inheritance. Used to model permissions and multiple dynamic roles of a user (e.g., student and simultaneously teaching assistant), encapsulating them in interchangeable components without having to rewrite complex class hierarchies.

---

## Code Architectural Layers (Packages)

### **Controller Layer**

Components exposing HTTP REST endpoints (e.g., `AuthController`, `CarrieraController`). They act as a thin orchestration layer: they extract identity from JWT tokens and delegate business logic to Services, then mapping the responses to appropriate HTTP formats.

### **Service Layer**

Classes containing business and domain logic (e.g., `AuthService`, `CarrieraService`). They coordinate controllers, handle authentication flows, and orchestrate integrations with external systems like ESSE3.

### **DTO (Data Transfer Object)**

Lightweight objects (POJOs) without domain logic, used exclusively to transport data between the backend and clients. They are used to structure data into "flat" formats, avoiding exposing the complexity of the original Cineca JSONs and optimizing interface rendering.

---

## Development Conventions and Standards

### **Semantic Naming**

Development convention dictating that names of entities, classes, and methods must be self-explanatory. Classes must be nouns (e.g., `Student`), while methods must be verbs (e.g., `calculateAverage()`). The use of vague names like `data` or `processItems` is strictly forbidden.

### **Magic Numbers**

Numbers or strings _hardcoded_ directly into the logical code. Their use is strictly forbidden by the ODD; they must always be replaced by named and typed constants.

### **Single Responsibility**

Programming principle stating that methods and functions must perform a single, precise operation. Complex functions must be broken down to ensure testability and clarity.

### **Docstrings (e.g., Javadoc / JSDoc)**

Mandatory standard format prescribed by the project for in-code documentation of packages, interfaces, classes, and methods. It requires the use of specific tags like `@param`, `@return`, and `@throws` to explicitly state contracts.

---

## Code-Level Specific Technologies

### **JWT (JSON Web Token)**

Standard used by the `OmuJwtService` service to sign, issue, and validate user sessions in OhMyUniversity!. The JWT token access contains the user identity and active university profiles.

### **OmuPrincipal**

Spring security context object (`@AuthenticationPrincipal`) injected into Controllers, representing the identity extracted and validated from the JWT token of the currently authenticated student.
