---
title: SDD - 1.1 Purpose of the System | OhMyUniversity!
description: OhMyUniversity is a distributed, multi-platform student portal that centralizes access to fragmented university data through microservices architecture, ensuring data integrity, performance, and scalability.
head:
  - - meta
    - property: og:title
      content: SDD - 1.1 Purpose of the System | OhMyUniversity!
  - - meta
    - property: og:description
      content: OhMyUniversity is a distributed, multi-platform student portal that centralizes access to fragmented university data through microservices architecture, ensuring data integrity, performance, and scalability.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-1-purpose-of-the-system
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, purpose, system design, microservices, architecture, centralization, scalability, spring boot, flutter, angular
  - - meta
    - name: twitter:title
      content: SDD - 1.1 Purpose of the System | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: System design overview of OhMyUniversity! distributed architecture, technology stack decisions, and strategic vision for the platform.
---

# OhMyUniversity! - SDD: 1.1 Purpose of the System

**OhMyUniversity!** is designed for **Italian university students** navigating the complex
landscape of institutional information systems. While initially targeting **Università degli studi del Molise (UNIMOL)**,
the system is architected to be portable across Italian universities that use CINECA/Esse3
as their academic data management platform. This document is intended for the **technical team
responsible for designing and implementing** the system, including architects, backend developers,
frontend developers, and DevOps engineers.

### The Problem

Today, students navigate 5-7 separate portals (Esse3, Moodle, Email, Library, Canteen, etc.)
to access information spread across different systems. This fragmentation causes:

- **Manual re-entry** of data (grade averaging, progress calculation)
- **Inconsistent information** across systems
- **Cognitive overload**: students lose focus on studies due to administrative friction

### The Solution

OhMyUniversity! provides a **unified backend** composed of multiple independent microservices
that aggregate data from institutional SaaS platforms (Cineca, Moodle, Email), enabling:

- **Multi-Platform Access** (Web, Mobile, Desktop) with a single authentication
- **Intelligent Automation** (real-time data sync, automatic calculations, caching)
- **Scalability** (independent microservice scaling based on load)
- **Strategic Tools** (Master's requirements tracking, career planning)

### Architectural Approach

OhMyUniversity! acts as an **advanced middleware layer** that orchestrates data from external
university systems (Cineca/Esse3, GOMP, Moodle) into a unified, student-centric interface.
The system is NOT a replacement for institutional systems—it aggregates and normalizes data,
allowing students to interact with a single point of entry while maintaining integrity with
official university records.
