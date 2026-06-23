---
title: Integration Tests | OhMyUniversity!
description: Integration testing documentation for OhMyUniversity, describing how modules, services, APIs, and data flows are validated together.
head:
  - - meta
    - property: og:title
      content: Integration Tests | OhMyUniversity!
  - - meta
    - property: og:description
      content: Integration testing for OhMyUniversity validates interactions between modules, services, APIs, and data flows.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/integration-tests
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, integration tests, integration testing, api testing, service testing, data flow validation
  - - meta
    - name: twitter:title
      content: Integration Tests | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Integration testing approach for OhMyUniversity modules, services, APIs, and data flows.
---

# Integration Tests

## Purpose

Integration tests verify that multiple units, components or layers of the system work correctly together.

While unit tests validate isolated pieces of logic, integration tests focus on the interaction between connected parts of the application. Their goal is to ensure that data is exchanged correctly across components, providers, services, repositories, controllers and external interfaces simulated through controlled test data.

From a theoretical point of view, integration testing, also known as component testing, verifies the interfaces between composed components. Even if each unit works correctly in isolation, errors can appear when those units communicate with each other.

## Testing Approach

The integration testing strategy follows a layered development testing approach.

The project uses controlled test scenarios where dependencies are replaced with mocks, stubs or test providers. This makes it possible to verify component interaction without requiring the complete production environment.

Two theoretical approaches can be identified:

- top-down testing, where higher-level components are tested while lower-level dependencies are replaced with stubs;
- bottom-up testing, where lower-level components are tested first and then connected to higher-level flows through drivers or test harnesses.

In OhMyUniversity, the practical approach is mixed:

- backend controller tests use mocked services to verify HTTP contracts;
- backend service tests verify business logic with controlled dependencies;
- mobile widget tests use provider overrides to simulate repositories, use cases or backend responses;
- web component tests use Angular TestBed to verify component composition, inputs, templates and local state.

## Scope

Integration tests cover the interaction between:

- frontend pages and state providers;
- widgets/components and mocked application data;
- forms and validation logic;
- controllers and services;
- repositories and data sources;
- API gateway and backend routes;
- backend services and controlled infrastructure;
- UI components and domain/application state.

They do not validate real production integrations with Cineca/Esse3, Microsoft email, Moodle or university portals. Those require end-to-end tests, staging tests or manual validation.

## Current Mobile Integration Tests

The mobile application includes widget and provider-level integration tests implemented with the Flutter testing framework.

The current mobile integration tests mainly cover:

- login page behavior, university selection and form validation;
- university login flow with mocked authentication use cases;
- career overview rendering using mocked career snapshots;
- study plan visualization, filters and course detail modal;
- simulated grade interaction and temporary statistics update;
- didactics tab navigation between overview, appeals and questionnaires;
- exam appeals filtering, booking confirmation modal and questionnaire lock;
- recommended appeals based on pending exams and CFU ordering;
- tuition fee page rendering from mocked tuition data;
- questionnaires view and questionnaire state changes;
- profile page rendering from badge data;
- services page rendering from external services provider data;
- institutional email inbox rendering from provider data;
- timetable repository behavior with controlled timetable data;
- calendar repository CRUD behavior with mock datasource;
- home dashboard widgets using mocked academic data;
- orientation guide flow from topic page to question answer saving.

## Current Web Integration Tests

The web application includes component-level integration tests implemented with the Angular testing ecosystem.

The current web integration tests mainly cover:

- Angular page rendering through `TestBed`;
- authentication page composition;
- university and partner login form behavior;
- university search/select interaction;
- reusable UI components inside Angular templates;
- public page composition with child components;
- orientation page composition;
- orientation topic rendering;
- orientation navigation, summary and result components;
- orientation charts and question flow components;
- general component interaction through inputs, template rendering and DOM assertions.

These tests verify that Angular components, templates, inputs and local state work together correctly. They do not represent full browser end-to-end tests against a running production backend.

## Current Backend Integration Tests

The backend has the strongest integration-level coverage.

The current backend integration tests mainly cover:

- REST controller behavior through `MockMvc`;
- authentication endpoints and login request handling;
- security configuration and JWT request filtering;
- API gateway routing and gateway security filters;
- career endpoints for student data, study plan, tuition, badge and booking-related data;
- calendar endpoints for event creation, retrieval, update and deletion;
- institutional email endpoints;
- external university service endpoints;
- Cineca/Esse3 client behavior with controlled responses;
- Cineca session handling;
- timetable fetching endpoints in the fetcher service;
- timetable service behavior;
- job trigger endpoints;
- statistics endpoints;
- database connectivity in controlled test conditions;
- Kafka connectivity in controlled test conditions.

Some backend tests use Spring MVC slices, while others use `SpringBootTest` and Testcontainers-based infrastructure checks.

## Requirement Coverage

| Requirement | Integration Test Coverage | Status |
|---|---|---|
| FR-1.1.1 Career Synchronization | Backend Cineca/Esse3 integration logic, career endpoints and mobile career data flow | Covered |
| FR-1.1.2 Grade Calculation | Mobile career overview and statistics flow | Covered |
| FR-1.1.3 Graduation Projection | Mobile career overview and graduation projection rendering | Covered |
| FR-1.1.4 Progress Tracking | Mobile career overview, CFU progress and home widgets | Covered |
| FR-1.1.5 Trend Analysis | Mobile career simulation, grade history and trend-related statistics | Covered |
| FR-1.1.6 Study Plan | Backend study plan endpoint and mobile career study plan UI | Covered |
| FR-1.1.7 Exam Session Management | Backend booking history/appelli endpoints and mobile appeals view | Partial |
| FR-1.1.8 Administrative Documentation | Backend tuition endpoint and mobile tuition page | Partial |
| FR-1.1.9 Digital Badge | Backend badge endpoint, mobile profile page and badge model | Covered |
| FR-1.2.1 Moodle Integration | Backend external services endpoint and mobile services page | Covered |
| FR-1.2.2 Institutional Email | Backend email endpoints and mobile email inbox page | Partial |
| FR-1.2.3 Library Services | Backend external services endpoint and mobile services page | Covered |
| FR-1.2.4 Optimized Esse3 Exam Booking | Mobile recommended appeals flow based on pending exams and CFU | Covered |
| FR-1.3.3 Class Timetable | Backend fetcher timetable endpoints and mobile timetable repository flow | Partial |
| FR-1.3.4 Integrated Calendar | Backend calendar endpoints and mobile calendar repository CRUD flow | Covered |
| FR-1.4.3 Enrollment Guide | Web orientation flow and mobile orientation guide/provider flow | Partial |

## Limitations

The current integration test suite validates the main interactions between application layers using controlled test data, mocked dependencies, provider overrides and backend test contexts.

Some scenarios still require additional validation because they depend on runtime conditions that are difficult to reproduce deterministically in automated tests, such as:

- availability of third-party systems;
- real user accounts and permissions;
- university-specific data returned by external platforms;
- network conditions;
- complete user journeys across frontend, backend and external services.

For this reason, integration tests are complemented by end-to-end tests and manual validation on selected flows.

The current integration coverage is sufficient to verify the main implemented modules, while the remaining partial areas mainly concern features that require stronger runtime validation or broader scenario coverage.

## Execution

Mobile integration/widget tests are executed with:

```bash
flutter test