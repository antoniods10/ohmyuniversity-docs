---
title: ODD - 1.0 Introduction | OhMyUniversity!
description: Introduction to the Object Design Document (ODD) of the OhMyUniversity! system, specifying the detailed object-oriented design and bridging the gap between architectural analysis and the codebase.
head:
  - - meta
    - property: og:title
      content: ODD - 1.0 Introduction | OhMyUniversity!
  - - meta
    - property: og:description
      content: Introduction to the Object Design Document (ODD) of the OhMyUniversity! system, specifying the detailed object-oriented design and bridging the gap between architectural analysis and the codebase.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/1-introduction/1-0-introduction
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, object design document, introduction, blueprint, architecture, codebase, requirements
  - - meta
    - name: twitter:title
      content: ODD - 1.0 Introduction | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Introduction to the Object Design Document (ODD) of the OhMyUniversity! system, specifying the detailed object-oriented design and bridging the gap between architectural analysis and the codebase.
---

# OhMyUniversity! - ODD: 1.0 Introduction

The Object Design Document (ODD) specifies the detailed object-oriented design of the OhMyUniversity! system, serving as the definitive blueprint and guide for developers prior to and during the implementation phase. While the System Design Document (SDD) outlined the high-level subsystems, persistence strategies, and global architecture, the ODD shifts the focus to the application's inner workings. It explicitly defines object structures, class interfaces, visibility, method signatures, exceptions, and implementation-level patterns.

This document bridges the gap between high-level architectural analysis and the concrete codebase, ensuring that the system remains maintainable, consistent, and fully aligned with the requirements defined in the Requirements Analysis Document (RAD).

For the current backend implementation, the system is organized into multiple coordinated components rather than a single Spring Boot application. The `ohmyuniversity-api-gateway` exposes the public API routes used by Web and Mobile clients, `ohmyuniversity-api-core` contains the main application logic for authentication, career data, calendar, email, and external university services, `ohmyuniversity-api-fetcher` manages timetable, statistics, and external data retrieval, while `ohmyuniversity-infra` contains deployment, Docker, monitoring, and infrastructure configuration.

