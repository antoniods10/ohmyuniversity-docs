---
title: End-to-End Tests | OhMyUniversity!
description: End-to-end testing documentation for OhMyUniversity, validating complete user workflows, system behavior, and critical scenarios from start to finish.
head:
  - - meta
    - property: og:title
      content: End-to-End Tests | OhMyUniversity!
  - - meta
    - property: og:description
      content: End-to-end testing for OhMyUniversity validates complete user workflows, system behavior, and critical scenarios.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/e2e-tests
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, e2e tests, end-to-end testing, user workflows, system testing, acceptance testing
  - - meta
    - name: twitter:title
      content: End-to-End Tests | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: End-to-end testing approach for OhMyUniversity user workflows and critical system scenarios.
---

# End-to-End / System-Level Tests

## Purpose

End-to-end tests verify the system from the user's point of view.

From a theoretical perspective, this level corresponds to system testing and use-case testing. The use cases defined in the RAD are used as the basis for validation because they describe the real goals that users are expected to achieve through the system.

The purpose of this section is to verify whether the implemented system supports the main student flows required by the selected RAD modules.

## Current Coverage

At the current stage, the project does not include a fully automated end-to-end suite that runs complete user journeys across frontend, backend and all external university services.

However, the project provides real system-level coverage through the combination of:

- mobile widget and provider tests;
- web component and page tests;
- backend controller, service, gateway and infrastructure tests;
- manual validation of the main user flows.

This means that the implemented modules are verified across multiple layers, even if the whole flow is not always executed as a single automated end-to-end test.

## Module 1: Career, Didactics and Student Administration

Module 1 has real coverage across backend and mobile, with partial web support.

The current tests and validations cover:

- retrieval and normalization of career data from backend services;
- student record book data, including exams, grades and CFU;
- arithmetic average and weighted average calculation;
- graduation base projection out of 110;
- CFU progress and exams passed;
- grade history and weighted average trend data;
- study plan visualization;
- compulsory and elective course handling;
- exam session visualization;
- booked and recommended exam states;
- questionnaire-related booking constraints;
- tuition fee status visualization;
- digital badge and student profile data.

Some areas remain partially validated at full E2E level, especially real booking operations and broader administrative documentation such as calls for applications or downloadable documents.

## Module 2: Access to External University Services

Module 2 has real coverage across backend and mobile, with partial web support.

The current tests and validations cover:

- external university service configuration;
- Moodle URL handling;
- library service URL handling;
- institutional email section states;
- email inbox visualization when data is available;
- fallback state when the email account is not connected;
- optimized exam recommendation based on pending exams and CFU priority.

The external services are validated through controlled application data and backend endpoint behavior. Full validation of the external platforms themselves depends on the availability of third-party systems.

## Module 3: Organization and Logistics

Module 3 has real backend and mobile coverage.

The current tests and validations cover:

- timetable document retrieval logic;
- timetable document mapping;
- filtering timetable data according to the student course;
- visualization of lesson timetable information;
- calendar event mapping;
- calendar event creation, update and deletion logic;
- calendar event retrieval through backend-supported flows;
- backend calendar endpoint behavior;
- backend timetable fetcher behavior.

This module is therefore implemented and testable, although full validation with real university timetable sources and production-like data should also be completed manually.

## Module 4: Orientation and Future Planning

Module 4 has strong web coverage and growing mobile coverage.

The current tests and validations cover:

- orientation page rendering;
- orientation topic rendering;
- orientation navigation;
- question flow and answer handling;
- orientation scoring logic;
- summary and result behavior;
- mobile orientation state management;
- mobile answer saving;
- initial mobile guide flow from topic content to question answer.

The orientation guide is therefore covered at component and state level. Full mobile validation of the complete flow from the first topic to the final result remains a higher-level scenario to validate manually or through future automated E2E tests.

## Limitations

The current system-level coverage validates the main implemented modules through automated tests at different layers and manual checks on the main user flows.

Some scenarios still require additional validation because they depend on runtime conditions that cannot always be reproduced deterministically in automated tests, such as:

- real user credentials;
- availability of external university systems;
- university-specific data returned by third-party platforms;
- network conditions;
- complete journeys involving frontend, backend and external services together.

For this reason, automated tests are complemented by manual validation on selected flows.

