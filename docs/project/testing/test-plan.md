---
title: Test Plan | OhMyUniversity!
description: Test plan for OhMyUniversity, defining testing objectives, scope, resources, schedule, responsibilities, risks, and acceptance criteria.
head:
  - - meta
    - property: og:title
      content: Test Plan | OhMyUniversity!
  - - meta
    - property: og:description
      content: The OhMyUniversity test plan defines testing scope, objectives, responsibilities, resources, risks, and acceptance criteria.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/testing/test-plan
  - - meta
    - name: keywords
      content: ohmyuniversity, testing, test plan, quality assurance, acceptance criteria, test scope, test schedule, risks
  - - meta
    - name: twitter:title
      content: Test Plan | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Testing plan for OhMyUniversity, including scope, responsibilities, risks, and acceptance criteria.
---

# Test Plan

The test plan defines the scope, objectives, responsibilities and acceptance criteria of the testing activities for OhMyUniversity.

## Scope

The testing activities cover the features currently implemented or partially implemented in the first deliverable:

- authentication prototype on web and mobile;
- dashboard navigation and widgets;
- academic career visualization;
- grade averages, graduation projection and CFU progress;
- study plan and exam-related views;
- exam sessions, booking history and questionnaires;
- tuition fees and administrative information;
- digital badge and student profile;
- orientation flows;
- calendar and timetable sections;
- external university services such as email, Moodle and library links;
- frontend communication with backend APIs.

Features not fully implemented yet, or still depending on partial backend support, are tested through mocks or controlled test data when needed.

## Responsibilities

The main testing responsibilities are distributed as follows:

| Area | Responsibility |
|---|---|
| Backend services | Validate API behavior, domain logic and data exposure |
| Web frontend | Validate Angular components, navigation and UI rendering |
| Mobile frontend | Validate Flutter screens, state management and user flows |
| Cross-layer integration | Validate that frontend requests and backend responses match the expected contracts |

## Timing

Testing is performed:
- during feature implementation;
- before each merge into the main development branch;
- after any relevant refactor or backend integration change;
- before release or demo preparation.

## Success Criteria

A test is considered successful when:
- the expected result is known in advance and matches the actual result;
- the flow can be completed without blocking errors;
- the displayed data is coherent with the requirement or the test dataset;
- no critical regression is introduced;
- the relevant automated checks pass in CI;
- the feature remains usable on the target screen sizes.

When a backend endpoint is missing or not yet stable, the frontend may keep a mock fallback to preserve development and validation continuity.