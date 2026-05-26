---
title: SDD - 2 Current Software Architecture | OhMyUniversity!
description: Overview of ESSE3/Cineca architecture at the University of Molise and identified limitations that motivate OhMyUniversity!'s design.
head:
  - - meta
    - property: og:title
      content: SDD - 2 Current Software Architecture | OhMyUniversity!
  - - meta
    - property: og:description
      content: Overview of ESSE3/Cineca architecture at Unimol and limitations identified in requirements analysis.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/2-current-software-architecture/2-current-software-architecture
  - - meta
    - name: keywords
      content: sdd, software architecture, esse3, cineca, unimol
  - - meta
    - name: twitter:title
      content: SDD - 2 Current Software Architecture | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: ESSE3/Cineca architecture at Unimol and identified gaps.
---

# 2 Current Software Architecture

## Overview

The University of Molise (Unimol) relies on **ESSE3**, a centralized academic management platform provided by **CINECA**. ESSE3 is the official institutional system designed for administrative management (fees, enrollments, exams) and serves as the authoritative data source for academic records.

This section describes the current system architecture and identifies the gaps that OhMyUniversity! addresses.

---

## ESSE3 Architecture at Unimol

### System Infrastructure

ESSE3 at Unimol provides:

- **REST API Gateway**: 31+ service endpoints exposing academic and administrative functionality
- **Centralized Database**: Single institutional database storing all academic records
- **Web Portal**: Official web interface for students and administrators
- **Authentication**: Bearer Token-based access control for API consumers

### Service Categories

ESSE3 organizes functionality across academic management domains:

| Category             | Services                                  | Purpose                                              |
| :------------------- | :---------------------------------------- | :--------------------------------------------------- |
| **Academic Records** | Carriere, Libretto, Piani, Offerta        | Enrollment, transcripts, study plans, course catalog |
| **Examinations**     | Calesa, Verbali, Questionari              | Exam calendar, records, student surveys              |
| **Finance**          | Tasse, Servizi                            | Tuition management, service requests                 |
| **Logistics**        | Logistica, Calendario Appuntamenti, Badge | Rooms, appointments, access control                  |
| **Identity**         | Utenti, Anagrafica, Docenti               | User management, personal data, faculty              |
| **Communication**    | Comunicazioni                             | Official notifications                               |
| **Administration**   | Struttura, regulatory services            | Organizational structure, regulations                |

---

## Identified Limitations

Based on the Requirements Analysis Document (RAD), ESSE3/Unimol has the following documented limitations:

### UX/UI Limitations

- **Complex Navigation**: Institutional interface not optimized for user experience
- **Multiple Clicks Required**: Students must navigate through many pages to access basic information
- **Form-Heavy Design**: Administrative focus rather than student-centric workflows

### Functional Gaps

- **No Integrated Logistical Services**: Canteen, classroom, and transportation information absent from platform
- **No Strategic Planning Support**: Master's degree requirement verification requires manual PDF review
- **Limited Academic Guidance**: No automated pathway recommendations or career planning tools
- **Fragmented Information**: Related data scattered across different institutional systems (Esse3, Moodle, etc.)

### Technical Constraints

- **Centralized Portal**: Single web interface; no native mobile applications
- **Service Proliferation**: 31+ distinct API endpoints requiring external orchestration
- **No Integration Layer**: Clients must independently handle data from multiple university systems

---

## OhMyUniversity! Design Approach

OhMyUniversity! is designed as a **multi-platform application** (Mobile, Web, Desktop) that:

- Acts as a **university services aggregator**, providing students with a centralized access point
- Implements a **lean backend layer** for orchestrating external university services (Esse3, Moodle)
- Does **not replace** ESSE3 but rather **enhances and centralizes** user access to it
- Provides an **intuitive frontend** designed to minimize navigation complexity

The system explicitly targets the identified gaps:

| Gap Identified          | OhMyUniversity! Response                                 |
| :---------------------- | :------------------------------------------------------- |
| Complex navigation      | Multi-platform interfaces optimized for user experience  |
| No integrated logistics | Backend aggregates logistical data sources               |
| No strategic planning   | Application layer can implement degree planning features |
| Fragmented information  | Middleware normalizes data from multiple systems         |
| Portal-only access      | Available on web, mobile, and desktop platforms          |
