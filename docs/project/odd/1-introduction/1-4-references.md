---
title: ODD - 1.4 References | OhMyUniversity!
description: Internal documents, style guides, and standards that inform the implementation and object design of OhMyUniversity.
head:
  - - meta
    - property: og:title
      content: ODD - 1.4 References | OhMyUniversity!
  - - meta
    - property: og:description
      content: References, style guides, and standards for the OhMyUniversity Object Design Document.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/1-introduction/1-4-references
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, references, google style guides, javadoc, standards, documentation
  - - meta
    - name: twitter:title
      content: ODD - 1.4 References | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Internal documents, style guides, and standards informing OhMyUniversity's codebase.
---

# OhMyUniversity! - ODD: 1.4 References

This section lists the internal project documents, programming style guides, and development standards that mandate the implementation details for OhMyUniversity!.

---

## Internal Documents

| Document                                 | Location            | Purpose                                                                                                                            |
| ---------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Requirements Analysis Document (RAD)** | `docs/project/rad/` | Defines the functional requirements that the code implementation must remain fully aligned with.                                   |
| **System Design Document (SDD)**         | `docs/project/sdd/` | Outlines the high-level subsystems, persistence strategies, and overall architecture on which the object-oriented design is based. |

---

## Development Standards & Style Guides

Code implementations, formatting rules, and interface documentation practices are strictly based on the following standards:

| Standard / Guide                  | Applicability                                       | Reference                                                             |
| --------------------------------- | --------------------------------------------------- | --------------------------------------------------------------------- |
| **Google Java Style Guide**       | Structural guidelines for Java                      | https://google.github.io/styleguide/javaguide.html                    |
| **Google TypeScript Style Guide** | Structural guidelines for TypeScript                | https://google.github.io/styleguide/tsguide.html                      |
| **Google JavaScript Style Guide** | Structural guidelines for JavaScript                | https://google.github.io/styleguide/jsguide.html                      |
| **Google HTML/CSS Style Guide**   | Structural guidelines for HTML/CSS                  | https://google.github.io/styleguide/htmlcssguide.html                 |
| **Effective Dart**                | Structural guidelines for Dart                      | https://dart.dev/effective-dart                                       |
| **GDPR (Article 32)**             | Security compliance for TTL implementation in cache | Time-To-Live implementation in external data flows to limit exposure. |

---
