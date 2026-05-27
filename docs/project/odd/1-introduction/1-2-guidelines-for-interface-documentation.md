---
title: ODD - 1.2 Interface Documentation Guidelines | OhMyUniversity!
description: Architectural and methodological guidelines for designing and documenting public interfaces, modules, and classes in OhMyUniversity!.
head:
  - - meta
    - property: og:title
      content: ODD - 1.2 Interface Documentation Guidelines | OhMyUniversity!
  - - meta
    - property: og:description
      content: Architectural and methodological guidelines for designing and documenting public interfaces, modules, and classes in OhMyUniversity!.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/1-introduction/1-2-interface-documentation-guidelines
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, interface documentation, guidelines, google style guides, naming conventions, design by contract
  - - meta
    - name: twitter:title
      content: ODD - 1.2 Interface Documentation Guidelines | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Architectural and methodological guidelines for designing and documenting public interfaces in OhMyUniversity!.
---

# OhMyUniversity! - ODD: 1.2 Interface Documentation Guidelines

This section defines the architectural and methodological guidelines that the OhMyUniversity! team must follow for designing and documenting public interfaces, modules, and classes. The goal is to ensure that every system component exposes clear, secure, and uniform contracts.

## Core Design Principles

- **Google Style Guides:** The project adopts the Google Style Guides as the authoritative standard for all programming languages used. These guides serve as the ultimate reference for structural and formatting decisions to ensure cross-language maintainability.

  | Programming Language | Guidelines                                            |
  | -------------------- | ----------------------------------------------------- |
  | Java                 | https://google.github.io/styleguide/javaguide.html    |
  | TypeScript           | https://google.github.io/styleguide/tsguide.html      |
  | HTML/CSS             | https://google.github.io/styleguide/htmlcssguide.html |
  | JavaScript           | https://google.github.io/styleguide/jsguide.html      |
  | Dart                 | https://dart.dev/effective-dart                       |

- **Clarity over Cleverness:** Code is read far more often than it is written. A solution that is slightly less elegant but immediately understandable is almost always preferable to a clever one that requires study.
- **Single Responsibility:** Functions and methods must do exactly one thing. A function that handles input validation, business logic, and database persistence is actually three functions and must be broken down to remain testable and focused.

## Naming Conventions

- **Semantic Naming (Names should tell the truth):** Chosen names must be self-explanatory and precisely describe what an entity is or what a function does without needing additional comments. Vague names like `data`, `temp`, or `processItems` communicate nothing and must be avoided.
- **Classes and Methods:** Class and interface names must be nouns (e.g., `Student`, `CourseManager`), while method names must be verbs or verb phrases indicating the action performed (e.g., `calculateAverage()`, `enrollStudent()`).
- **Ban on Magic Numbers:** The use of "magic numbers" or hardcoded strings in the code is strictly forbidden. Such values are a maintenance hazard and must be extracted into named and typed constants.

## Design by Contract

The ODD establishes that every public method exposes a clear contract to its clients. Interface documentation must always explicitly state:

- **Pre-conditions:** Requirements that must be true before a method invocation (e.g., non-null parameters, valid system state).
- **Post-conditions:** Guarantees that the method ensures after its execution, assuming the pre-conditions were respected.
- **Class Invariants:** Logical properties of the class that must remain valid before and after the execution of any public method.

## Error and Exception Handling

- **Exceptions vs. Return Values:** Contract violations (e.g., unrespected pre-conditions, network failures, data not found) must always be signaled by throwing a specific Exception (e.g., `IllegalArgumentException`, `EntityNotFoundException`).
- **Ban on Status Codes:** Managing error states through special return values (e.g., returning `-1`, `false`, or `null` to indicate a logical error) is forbidden.

## Documentation Standards

Every public component of the system (packages, classes, interfaces, and methods) must mandatorily be documented using the Javadoc standard (or equivalents such as TypeDoc/JSDoc).

- **Mandatory Docstrings:** Every public component (packages, classes, interfaces, and methods) must be documented using the standard docstring format for the specific language (e.g., Javadoc for Java, JSDoc for TypeScript, Google Style for Python).
- **Mandatory Tags:** For example in Java, method doc comments must include `@param` tags to describe inputs, `@return` to explain the expected output, and `@throws` to describe thrown exceptions. Files like `package.html` or `overview.html` will be used to provide general context on subsystems. In classes, tags like `@author`, `@version`, and `@since` will track the component's evolution.
