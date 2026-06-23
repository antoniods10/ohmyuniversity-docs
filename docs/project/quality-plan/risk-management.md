---
title: Risk Management | OhMyUniversity!
description: Risk management plan for OhMyUniversity, identifying project risks related to external services, academic data correctness, security, test coverage, Web-Mobile consistency, and scope creep.
head:
  - - meta
    - property: og:title
      content: Risk Management | OhMyUniversity!
  - - meta
    - property: og:description
      content: Risk management plan for OhMyUniversity, covering external service dependency, incorrect academic data processing, security weaknesses, low test coverage, Web-Mobile inconsistency, and scope creep.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/quality-plan/risk-management
  - - meta
    - name: keywords
      content: ohmyuniversity, quality plan, risk management, external services, esse3, cineca, moodle, security risks, data correctness, test coverage, web mobile consistency, scope creep
  - - meta
    - name: twitter:title
      content: Risk Management | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Risk management for OhMyUniversity, describing the main project risks and mitigation strategies.
---

# Risk Management

The main risks of the OhMyUniversity! project are related to external service dependency, security, reliability, low test coverage, duplicated or inconsistent logic, and possible misalignment between the Web and Mobile applications.

## External Service Dependency

OhMyUniversity! depends on external university systems such as Esse3/Cineca, Moodle, institutional email services, and library portals.

If one of these services is unavailable, slow, or returns unexpected data, the application may not be able to show updated academic or didactic information.

The mitigation strategy is to handle errors clearly, avoid application crashes, and provide fallback behavior where possible, such as cached data, informative error messages, or links to the original external service.

## Security Weakness

A relevant risk is the presence of security issues in the codebase.

This risk is realistic because the platform handles personal, academic, and administrative student data, including authentication flows and data retrieved from university services.

The mitigation strategy is to treat SonarQube security issues as high-priority tasks, review authentication and API communication carefully, avoid exposing sensitive data, and ensure that credentials or personal information are not stored or logged unnecessarily.

## Reliability Issues

Another risk is the presence of reliability problems that may cause unstable behavior during important user flows.

This is especially relevant for operations such as login, academic career visualization, grade and ECTS consultation, study plan access, calendar usage, and access to external services.

The mitigation strategy is to monitor reliability issues reported by SonarQube, test the main user flows, handle unexpected API responses, and prevent isolated failures from blocking the entire application.

## Low Test Coverage

Low test coverage is a concrete project risk because it makes regressions harder to detect.

If critical flows are not covered by tests, future changes may break authentication, academic career visualization, grade calculation, calendar behavior, or external-service access without being immediately detected.

The mitigation strategy is to progressively increase automated test coverage, starting from the most critical and stable features of the selected modules.

## Maintainability and Duplicated Logic

A further risk is the growth of technical debt caused by duplicated code, unclear responsibilities, or inconsistent implementation patterns.

This risk can make future development slower and increase the probability of introducing bugs when similar logic must be changed in multiple places.

The mitigation strategy is to monitor duplication through SonarQube, keep components and services modular, follow consistent naming conventions, and extract shared logic when duplication becomes harmful.

## Inconsistency Between Web and Mobile

Since OhMyUniversity! provides both a Web application and a Mobile application, there is a risk that the same feature behaves differently on the two platforms.

This could confuse users and make the documentation unreliable, especially for shared functionalities such as authentication, academic career consultation, exam sessions, calendar, and external-service access.

The mitigation strategy is to align routes, feature names, user flows, and expected behavior between Web and Mobile, and to document only the behavior that is actually available in the current sprint.

## Scope Creep

The project includes many possible university services, but only some modules are part of the selected delivery scope.

If the team tries to include too many secondary features, the quality of the core modules may decrease.

The mitigation strategy is to focus on the selected modules for the sprint and clearly separate implemented features from future or planned functionalities.