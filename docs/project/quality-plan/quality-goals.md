---
title: Quality Goals | OhMyUniversity!
description: Quality goals for OhMyUniversity, defined according to SonarQube quality attributes such as Security, Reliability, Maintainability, Coverage, Duplications, and Security Hotspots Review.
head:
  - - meta
    - property: og:title
      content: Quality Goals | OhMyUniversity!
  - - meta
    - property: og:description
      content: Quality goals for OhMyUniversity based on SonarQube indicators, including Security, Reliability, Maintainability, Coverage, Duplications Control, and Security Hotspots Review.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/quality-plan/quality-goals
  - - meta
    - name: keywords
      content: ohmyuniversity, quality plan, quality goals, sonarqube, security, reliability, maintainability, coverage, duplications, security hotspots, technical debt, code quality
  - - meta
    - name: twitter:title
      content: Quality Goals | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Quality goals for OhMyUniversity based on SonarQube metrics and objective code quality indicators.
---

# Quality Goals

The quality goals of OhMyUniversity! are defined according to the main quality attributes monitored through SonarQube.

The project focuses on:

* Security;
* Reliability;
* Maintainability;
* Coverage;
* Duplications;
* Security Hotspots Review.

These indicators provide an objective way to evaluate the current state of the codebase and guide future improvements.

## Security

The first quality goal is **Security**.

Since the platform handles personal, academic, and administrative student data, the system must reduce security vulnerabilities as much as possible.

Authentication flows, API communication, and access to protected resources must be implemented carefully. Sensitive information must not be exposed, logged, or stored unnecessarily.

Security issues detected by SonarQube must be treated as high-priority items because they can directly affect the protection of student data.

## Reliability

The second quality goal is **Reliability**.

The system must behave correctly and consistently during the main student workflows, such as:

* login;
* academic career consultation;
* grade and ECTS visualization;
* study plan access;
* exam session consultation;
* calendar management;
* access to external services.

Reliability issues detected by SonarQube must be monitored and progressively reduced in order to prevent runtime errors, unstable behaviors, and failures in critical user flows.

## Maintainability

The third quality goal is **Maintainability**.

The codebase must remain understandable, modular, and easy to modify. This is especially important because the project is developed by a team and evolves across multiple sprints.

Components, routes, services, repositories, and UI pages should follow consistent naming conventions and clear responsibilities.

Maintainability issues detected by SonarQube must be addressed to avoid technical debt and make future development safer.

## Coverage

The fourth quality goal is **Coverage**.

Automated tests are necessary to verify that the most important parts of the system continue to work after changes.

The team should progressively increase test coverage, giving priority to critical flows such as:

* authentication;
* academic career data;
* average and graduation score calculation;
* exam sessions;
* calendar events;
* external-service access.

Low coverage is accepted only as a temporary sprint limitation, but it must be improved in future iterations.

## Duplications Control

The fifth quality goal is **Duplications Control**.

Duplicated code should be avoided because it makes maintenance harder and increases the risk of inconsistent behavior between similar features.

When the same logic appears in multiple parts of the system, the team should evaluate whether it can be extracted into:

* shared services;
* utilities;
* reusable components.

This is particularly important for features shared between Web and Mobile, where behavior must remain coherent.

## Security Hotspots Review

The sixth quality goal is **Security Hotspots Review**.

All security hotspots reported by SonarQube must be reviewed by the team to determine whether they represent an actual vulnerability or an acceptable implementation choice.

A hotspot is not automatically a bug, but it requires inspection because it may involve sensitive operations such as:

* authentication;
* data access;
* input validation;
* communication with external services.

## Accepted Trade-off

Overall, the project prioritizes **Security** and **Reliability** over the rapid introduction of new features.

The accepted trade-off is that the team may delay non-essential functionalities if the current implementation contains unresolved security, reliability, or maintainability issues.

The goal is to deliver fewer features, but with a codebase that is safer, more stable, easier to maintain, and measurable through objective quality indicators.
