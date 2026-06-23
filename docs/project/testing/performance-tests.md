---
title: Performance Tests | OhMyUniversity!
description: Performance testing documentation for OhMyUniversity, defining load, stress, scalability, responsiveness, and reliability validation criteria.
head:
  - - meta
    - property: og:title
      content: Performance Tests | OhMyUniversity!
  - - meta
    - property: og:description
      content: Performance testing for OhMyUniversity validates load, stress, scalability, responsiveness, and reliability requirements.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/performance-tests
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, performance tests, load testing, stress testing, scalability, reliability, response time
  - - meta
    - name: twitter:title
      content: Performance Tests | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Performance testing approach for OhMyUniversity load, stress, scalability, and responsiveness.
---

# Performance Tests

## Purpose

Performance tests verify whether the system satisfies measurable non-functional requirements related to response time, usability, reliability and resource usage.

In OhMyUniversity, the RAD defines performance and usability targets such as synchronization time, booking completion time, maximum number of interactions and accuracy of academic data.

## Current Status

At the current stage, the project does not include a dedicated automated performance testing suite.

The project currently includes automated unit, widget, component and integration tests, but these tests mainly verify correctness rather than execution time, load behavior or accessibility compliance.

Some performance-related design decisions are already present in the architecture, such as:

- use of DTOs to simplify data transfer;
- session-based reuse of authentication and academic data;
- backend separation through microservices;
- API gateway routing;
- lazy access to external resources such as PDFs or external service links;
- local/mock fallback for development and testing.

However, quantitative performance targets still require dedicated validation.

## Performance Criteria

| Requirement | Target | Current Validation |
|---|---|---|
| Data synchronization | Within 2 seconds | Not automatically measured yet |
| Booking flow duration | Within 60 seconds | Not automatically measured yet |
| Important information retrieval | Maximum 3 taps | Partially validated manually |
| Academic data accuracy | 100% correspondence with Cineca/Esse3 data | Partially validated through calculation tests; real comparison requires controlled Cineca/Esse3 data |
| Accessibility | WCAG 2.1 AA | Not automatically validated yet |
| Dashboard loading | No blocking UI during data retrieval | Partially validated through widget/integration tests |
| Calendar and timetable retrieval | Acceptable response time with backend services | Not automatically measured yet |

## Planned Performance Tests

| Test ID | Scenario | Metric | Expected Result |
|---|---|---|---|
| PT-01 | Synchronize academic career data | Response time | Data is loaded within 2 seconds in controlled test conditions |
| PT-02 | Open career dashboard | Loading time | Main academic information appears without blocking the UI |
| PT-03 | Retrieve weighted average | Number of interactions | User reaches the information in 3 taps or fewer |
| PT-04 | Complete exam/session booking flow | Completion time | User completes the flow within 60 seconds |
| PT-05 | Open timetable document | Response time | Timetable data or document link is displayed within an acceptable time |
| PT-06 | Create calendar event | Response time | Event is created and displayed without visible delay |
| PT-07 | Validate academic calculations | Accuracy | Calculated values match the reference Cineca/Esse3 dataset |
| PT-08 | Accessibility check | WCAG criteria | Main screens satisfy WCAG 2.1 AA checks |

## Testing Approach

Performance testing will be executed through a combination of:

- manual timed tests on critical user flows;
- backend response-time measurements;
- frontend loading-time checks;
- controlled datasets for academic calculation accuracy;
- accessibility audits using automated tools and manual inspection;
- repeated runs to avoid relying on isolated measurements.

## Limitations

The current implementation has not yet been validated through a complete performance testing campaign.

The measurable RAD targets are defined, but they require dedicated execution conditions, stable backend services, representative data and repeatable measurement tools.

For this reason, performance testing is currently planned as a validation activity for future iterations, while the current test suite mainly covers functional correctness.

## Summary

Performance requirements are clearly defined in the RAD and supported by architectural decisions described in the ODD.

However, the current automated test suite does not yet provide full quantitative performance validation. Existing tests verify correctness and integration, while future performance tests will measure response time, usability, accessibility and data accuracy against the targets defined in the RAD.