---
title: ODD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
description: Glossary of object-oriented design terms, implementation patterns, and acronyms used in the OhMyUniversity ODD.
head:
  - - meta
    - property: og:title
      content: ODD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
  - - meta
    - property: og:description
      content: Definitions and acronyms for OhMyUniversity ODD to ensure clear communication across the development team.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/1-introduction/1-3-definitions-acronyms
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, definitions, acronyms, glossary, design patterns, dto, proxy pattern, javadoc
  - - meta
    - name: twitter:title
      content: ODD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Glossary of object-oriented terms and project-specific acronyms used in OhMyUniversity documentation.
---

# OhMyUniversity! - ODD: 1.3 Definitions, Acronyms and Abbreviations

This section clarifies terminology specific to the implementation, object-oriented design, and architectural patterns of OhMyUniversity!.

---

## Definitions (Object Design & Implementation)

### **Design Patterns & Strategies**

| Term                             | Definition                                                                                                                                                                                                                         |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Design by Contract**           | A software design methodology where every public method exposes a clear contract consisting of preconditions, postconditions, and class invariants.                                                                                |
| **Data Transfer Object (DTO)**   | Lightweight objects with "flat" data structures used in specific access paths to avoid traversing nested JSON structures, thus optimizing rendering performance.                                                                   |
| **Lazy Loading / Eager Loading** | Memory and network management strategies. Lightweight metadata is loaded _eagerly_ (immediately), while heavy payloads (such as lesson contents) are loaded _lazily_ (only when explicitly requested) to avoid cumulative latency. |
| **Object Composition**           | A design principle strongly preferred over inheritance for modeling roles and user sessions, allowing behaviors and permissions to be encapsulated in dynamically interchangeable components.                                      |
| **Proxy Pattern**                | Architectural pattern adopted by the system to implement the Lazy Loading strategy for external resources.                                                                                                                         |
| **Single Responsibility**        | The principle that functions and methods should do exactly one thing, making code more testable and focused.                                                                                                                       |

### **Documentation & Development Terms**

| Term                                | Definition                                                                                                                                                                                                                      |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Docstrings (e.g. Javadoc/JSDoc)** | Standard required format for documenting public system components (packages, classes, interfaces, methods), which includes specific tags (e.g. `@param`, `@return`, `@throws`).                                                 |
| **Magic Numbers**                   | Numeric values or strings hardcoded directly in the code. Their use is strictly prohibited and they must be replaced with named, typed constants.                                                                               |
| **Semantic Naming**                 | Convention requiring entity, class, and method names to be self-explanatory, precisely describing the action or entity (e.g. nouns for classes and verbs for methods), avoiding generic names such as `data` or `processItems`. |

---

## Acronyms (Design & Architecture Context)

| Acronym   | Expansion                                     | Definition                                                                                                          |
| --------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **ACID**  | Atomicity, Consistency, Isolation, Durability | Properties guaranteed by relational databases (PostgreSQL) to maximize the integrity of natively generated data.    |
| **DTO**   | Data Transfer Object                          | Object used to encapsulate and transfer data in flat structures.                                                    |
| **JWT**   | JSON Web Token                                | Standard used for managing user sessions.                                                                           |
| **ODD**   | Object Design Document                        | Document that specifies the detailed object-oriented design of the system and serves as a guideline for developers. |
| **RAD**   | Requirements Analysis Document                | Document that contains the requirements the codebase must remain aligned with.                                      |
| **RDBMS** | Relational Database Management System         | Relational database management system (e.g. PostgreSQL) used to ensure the integrity of core data.                  |
| **SDD**   | System Design Document                        | Document describing high-level subsystems, from which the ODD inherits architecture to define internal structures.  |
| **TTL**   | Time-To-Live                                  | Time limit (set to 1 hour) applied to in-memory cache for sensitive external data to ensure GDPR compliance.        |

---
