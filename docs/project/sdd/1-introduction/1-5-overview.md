---
title: SDD - 1.5 Overview | OhMyUniversity!
description: Overview and table of contents for the introduction section of the Software Design Document, covering system purpose, design goals, definitions, and references.
head:
  - - meta
    - property: og:title
      content: SDD - 1.5 Overview | OhMyUniversity!
  - - meta
    - property: og:description
      content: Overview and table of contents for the introduction section of the SDD.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-5-overview
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, introduction, purpose, design goals, definitions, references
  - - meta
    - name: twitter:title
      content: SDD - 1.5 Overview | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Overview and table of contents for the introduction section of the SDD.
---

# OhMyUniversity! - SDD: 1.5 Overview

This section provides the foundational context for the entire Software Design Document. It establishes the system's purpose, articulates measurable design goals, clarifies terminology, and provides references for further consultation.

---

## Table of Contents

## 1.1 Purpose of the System

Describes the fundamental purpose and vision of OhMyUniversity!: a multi-platform student portal that centralizes access to fragmented university data. Details the problem OhMyUniversity! solves, the target users (Italian university students), initial deployment scope (UNIMOL), and the service-based architecture approach—currently three services implemented and connected end-to-end (`api-core`, `api-gateway`, `api-fetcher`), with additional domain services already scaffolded for future integration.

## 1.2 Design Goals

Defines the prioritized, measurable design goals that drive all architectural decisions. Covers critical objectives including Data Integrity, Performance, Reliability, Usability, Scalability, Maintainability, Authentication, and Legal Compliance. Includes explicit trade-off analysis and resolutions for conflicting goals, and clearly separates currently implemented decisions from planned future evolutions (e.g. SPID/CIE login, cloud deployment).

## 1.3 Definitions, Acronyms, and Abbreviations

Provides a comprehensive glossary of key terms, acronyms, and abbreviations used throughout the SDD, ensuring consistent terminology and reducing ambiguity across the document.

## 1.4 References

Lists internal project documentation (RAD, ODD, Quality Plan), regulatory and compliance standards (GDPR, AgID, WCAG, SPID), and technical documentation for currently integrated external systems (Esse3).