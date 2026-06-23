---
title: Test Strategy | OhMyUniversity!
description: Test strategy for OhMyUniversity, describing the overall testing approach, test levels, techniques, environments, automation, and quality gates.
head:
  - - meta
    - property: og:title
      content: Test Strategy | OhMyUniversity!
  - - meta
    - property: og:description
      content: The OhMyUniversity test strategy describes the testing approach, test levels, techniques, environments, automation, and quality gates.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/test-strategy
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, test strategy, test levels, automation, quality gates, test environment, validation
  - - meta
    - name: twitter:title
      content: Test Strategy | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Testing strategy for OhMyUniversity, covering approach, automation, environments, and quality gates.
---

# Test Strategy

The testing strategy follows a layered approach. Each level verifies the system from a different point of view and with increasing integration depth.

## Overall Approach

The strategy combines:
- unit testing for isolated logic;
- integration testing for connected modules and API boundaries;
- end-to-end testing for complete user flows;
- performance testing for usability and response-time targets;
- regression testing after changes and refactors.

This structure is consistent with the development testing progression: from single units, to integrated components, to the complete system.

## Automated Quality Gates

### Mobile

The Flutter client is validated through:
- `flutter analyze`
- `flutter test`
- `flutter test --coverage`
- `build_runner` generation before tests when needed

### Web

The Angular client is validated through:
- `pnpm lint`
- `pnpm tsc --noEmit`
- `pnpm vitest run`
- `pnpm build`
- `pnpm test:coverage`

### Backend

The backend is already documented and broadly covered by its own testing activities. In the project documentation, the backend is treated as the integration source for the frontend clients and as the reference point for contract validation.

## Testing Levels

| Level | Purpose | Typical Scope |
|---|---|---|
| Unit Testing | Verify isolated logic | validators, mappers, calculations, state helpers, use cases |
| Integration Testing | Verify cooperation between components | repository/datasource/API boundaries, backend contracts, frontend-backend communication |
| End-to-End Testing | Verify full user flows | login, dashboard, career, exams, badge, orientation, calendar |
| Performance Testing | Verify non-functional targets | load time, tap count, synchronization time, responsive behavior |
| Regression Testing | Prevent breakage after changes | previously validated flows and calculations |

## Current Focus

For the current deliverable, the highest-priority test areas are:
- authentication;
- academic career and calculations;
- exam sessions and booking history;
- tuition fees and administrative data;
- badge and profile data;
- external services links;
- orientation flows;
- calendar and timetable sections.

## Expected Result Model

For every test case, the expected result is defined before execution. This acts as the oracle for validation and makes it possible to compare:
- the requirement;
- the planned outcome;
- the observed result.

This approach keeps the verification objective and easy to trace back to the RAD requirements.