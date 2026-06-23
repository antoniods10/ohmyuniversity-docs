---
title: Unit Tests | OhMyUniversity!
description: Unit testing documentation for OhMyUniversity, defining the scope, objectives, tools, coverage expectations, and validation criteria for isolated components.
head:
  - - meta
    - property: og:title
      content: Unit Tests | OhMyUniversity!
  - - meta
    - property: og:description
      content: Unit testing documentation for OhMyUniversity, covering isolated component validation, tools, coverage, and quality criteria.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/unit-tests
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, unit tests, unit testing, component testing, test coverage, automated tests
  - - meta
    - name: twitter:title
      content: Unit Tests | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Unit testing approach for OhMyUniversity components and isolated logic validation.
---

# Unit Tests

## Purpose

Unit tests verify the behavior of individual units of the system in isolation.

In OhMyUniversity, unit tests are used to check that the core logic of the application works correctly when it receives controlled input data. The goal is to verify single functions, services, mappers, providers and calculation utilities without depending on complete real user flows or live external systems.

From a theoretical point of view, unit testing represents the first level of development testing. It focuses on the smallest testable parts of the software and checks their behavior against a predefined expected result, also called the test oracle. For each test, the input data and the expected output are known before execution.

## Scope

The unit testing activity covers the parts of the system where business logic, data transformation or local state management is present.

The current unit test scope includes:

- academic statistics calculation;
- arithmetic average and weighted average calculation;
- graduation base projection;
- CFU progress calculation;
- grade history and weighted average trend generation;
- simulation of future grades;
- mapping of backend career data into mobile models;
- mapping of tuition fee data;
- mapping of digital badge data;
- mapping of external university service URLs;
- mapping and serialization of calendar events;
- calendar repository operations with controlled test data;
- timetable document mapping and repository behavior;
- institutional email inbox rendering with controlled provider data;
- authentication session model behavior;
- orientation state management;
- orientation answer handling;
- orientation completion and result generation;
- recommended exam ordering based on pending exams and CFU.

The scope does not include full production user journeys, real external services or real Cineca/Esse3 availability. These aspects are covered by integration tests, end-to-end tests or manual validation.

## Testing Approach

Each unit test follows the same logical structure:

1. a controlled input is prepared;
2. the unit under test is executed;
3. the actual result is compared with the expected result;
4. the test passes only if the observed behavior matches the expected oracle.

This approach allows the project to detect regressions in critical logic before testing the system as a whole.

For example, academic calculations are tested independently from the UI. This means that the arithmetic average, weighted average, CFU progress and graduation base can be verified even without opening the career page.

Similarly, data mappers are tested independently from the backend. A sample response is provided to the mapper and the resulting domain model is checked.

## Current Mobile Unit Tests

The mobile application includes unit and widget-level tests implemented with the Flutter testing framework.

The current mobile tests mainly cover:

- authentication session models and login-related validation;
- university login data and supported university handling;
- academic career data mapping;
- arithmetic average and weighted average calculation;
- CFU progress and graduation base projection;
- grade history and weighted average trend generation;
- simulated grade handling;
- tuition fee data mapping;
- digital badge data mapping;
- external university service URL mapping;
- institutional email inbox rendering and not-connected state;
- calendar event mapping, serialization and repository CRUD behavior;
- timetable document mapping and repository behavior;
- orientation state management, answer handling and result generation;
- orientation guide flow from topic to saved answer;
- recommended exam ordering based on pending exams and CFU.

## Current Web Unit Tests

The web application also includes unit and component-level tests. They are implemented with the Angular testing ecosystem and focus on services, components, forms and state-related logic.

The current web tests mainly cover:

- authentication pages and forms;
- public pages and navigation-related components;
- reusable UI components;
- dashboard and academic information components;
- orientation pages;
- orientation topic rendering;
- orientation scoring and question flow;
- external service access components;
- general component behavior.

## Current Backend Unit Tests

The backend includes unit and integration-level tests implemented with the Spring Boot testing ecosystem.

The current backend tests mainly cover:

- authentication controllers and services;
- JWT generation, validation and request filtering;
- security configuration;
- API gateway routing and filtering;
- Cineca/Esse3 client behavior and response handling;
- supported university registry;
- career, study plan, tuition, badge and booking-related services;
- calendar controllers and services;
- institutional email controllers and services;
- external university service endpoints;
- timetable fetching and timetable service logic;
- statistics endpoints;
- database and Kafka connectivity in controlled test conditions.

## Requirement Coverage

| Requirement | Unit / Widget-Level Test Coverage | Status |
|---|---|---|
| FR-1.1.1 Career Synchronization | Career API model mapping tests | Partial |
| FR-1.1.2 Grade Calculation | Academic statistics calculator tests | Covered |
| FR-1.1.3 Graduation Projection | Graduation base calculation tests | Covered |
| FR-1.1.4 Progress Tracking | CFU and progress calculation tests | Covered |
| FR-1.1.5 Trend Analysis | Grade history, weighted average trend and simulation tests | Covered |
| FR-1.1.6 Study Plan | Career model, course data and career overview tests | Partial |
| FR-1.1.7 Exam Session Management | Appeals provider, booking state, questionnaire lock and recommendation tests | Partial |
| FR-1.1.8 Administrative Documentation | Tuition fee model and tuition page tests | Partial |
| FR-1.1.9 Digital Badge | Student badge model and profile page tests | Covered |
| FR-1.2.1 Moodle Integration | External service URL mapping and services page tests | Covered |
| FR-1.2.2 Institutional Email | Email inbox page and not-connected state tests | Partial |
| FR-1.2.3 Library Services | External service URL mapping and services page tests | Covered |
| FR-1.2.4 Optimized Esse3 Exam Booking | Recommended appeals provider tests | Covered |
| FR-1.3.3 Class Timetable | Timetable document mapping and repository behavior tests | Partial |
| FR-1.3.4 Integrated Calendar | Calendar event model, serialization and repository CRUD tests | Covered |
| FR-1.4.3 Enrollment Guide | Orientation provider, answer handling, result generation and initial guide flow tests | Partial |



## Limitations

Some requirements cannot be fully validated through unit tests alone.

Unit tests are designed to verify isolated units such as models, mappers, providers, services and calculation logic. They are useful to check that a specific piece of code behaves correctly with controlled input data, but they do not validate the complete behavior of the system.

Some requirements involve multiple layers of the application, such as frontend state, backend APIs, persistence, external services and complete user flows. For this reason, unit tests must be complemented by integration tests, end-to-end tests and manual validation.

In OhMyUniversity, unit tests provide the first level of confidence in the correctness of the implemented logic. Complete validation of user-facing requirements is addressed through higher-level tests, where the interaction between components and the final user experience can be verified.