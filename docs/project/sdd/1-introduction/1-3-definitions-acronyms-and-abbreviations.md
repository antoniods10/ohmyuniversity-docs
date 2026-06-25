---
title: SDD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
description: Glossary of domain-specific terms, Italian university systems, and project-specific acronyms used in OhMyUniversity.
head:
  - - meta
    - property: og:title
      content: SDD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
  - - meta
    - property: og:description
      content: Definitions and acronyms for OhMyUniversity SDD to ensure clear communication across the technical team.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-3-definitions-acronyms
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, definitions, acronyms, glossary, italian universities, CINECA, GDPR, SPID
  - - meta
    - name: twitter:title
      content: SDD - 1.3 Definitions, Acronyms and Abbreviations | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Glossary of domain-specific and project-specific terms used in OhMyUniversity documentation.
---

# OhMyUniversity! - SDD: 1.3 Definitions, Acronyms and Abbreviations

This section clarifies terminology specific to OhMyUniversity!, Italian university systems, and regulatory contexts.
Standard technical terms (API, REST, Docker, etc.) are assumed to be known by the development team.

---

## Definitions (Domain & Project-Specific)

### **System Components (OhMyUniversity-Specific)**

| Term | Definition |
|------|-----------|
| **api-core** | The central backend service. Owns authentication, the Esse3 integration, calendar and email features, and is the single publisher of domain events to Kafka. |
| **api-gateway** | The edge service that routes client requests to the appropriate backend service and enforces JWT-based request authentication. |
| **api-fetcher** | A batch-oriented service that ingests external/public datasets (e.g. MUR statistics, professional orders registries, timetable sources) on a schedule. |
| **OmuUser** | The local OhMyUniversity user profile. Created automatically on a student's first successful login; linked to one or more Esse3 academic careers via fiscal code. |
| **Cache-Aside Pattern** | Caching strategy: on read, check the Redis cache first; if missing, fetch from source (Esse3) and populate the cache. Used to avoid repeated calls to external APIs and help meet the < 2.0s sync target. |
| **Middleware Layer** | Software layer acting as intermediary between institutional systems (primarily Esse3) and student-facing interfaces. OhMyUniversity orchestrates this data into a unified experience. |

### **Italian University Systems**

| Term | Definition |
|------|-----------|
| **CINECA** | Italian consortium and IT systems provider for universities. Manages Esse3 (student/exam data) among other academic systems. |
| **Esse3** | CINECA's student information system storing exams, grades, study plans, administrative data. OhMyUniversity currently integrates with Esse3 for authentication and Career data. |
| **GOMP** | A proprietary academic records and career management system, used in parallel to Esse3 by some institutions. Not yet integrated by OhMyUniversity; planned for a future iteration. |
| **ECTS (European Credit Transfer System)** | Standardized academic credit; 1 ECTS ≈ 25-30 study hours. Bachelor = 180 ECTS; Master = 120 ECTS. OhMyUniversity tracks and displays ECTS progress. |
| **CFU (Credito Formativo Universitario)** | Italian equivalent of ECTS; 1 CFU ≈ 25 hours. OhMyUniversity uses CFU for Italian universities, ECTS for international context. |
| **Grade Average (Media Ponderata)** | Weighted mean of exam grades calculated as: (grade1×ECTS1 + grade2×ECTS2 + ...) / (ECTS1 + ECTS2 + ...). OhMyUniversity computes this automatically from Esse3 data. |
| **Graduation Projection (Voto Laurea)** | Predicted final degree score (0-110). OhMyUniversity displays it for student planning. |
| **Honors/Lode** | Merit mention for 30/30 exam grade; may add points to the weighted average per institutional rules. OhMyUniversity flags eligible exams. |
| **Master's Requirements** | Specific exams/ECTS students must complete to be eligible for a particular Master's degree. OhMyUniversity verifies eligibility automatically. |
| **Study Plan (Piano di Studi)** | Official record of courses a student is enrolled in (compulsory + elective). OhMyUniversity displays and allows modification. |

### **Authentication**

| Term | Definition |
|------|-----------|
| **JWT (JSON Web Token)** | Token issued by `api-core` after a student's credentials are verified against Esse3. Used to authenticate requests to the backend; valid for 90 minutes. |
| **Refresh Token** | Token issued alongside the JWT, used by the client to obtain a new JWT before the current one expires, without requiring the student to re-enter institutional credentials. |

### **Italian Digital Identity & Compliance**

| Term | Definition |
|------|-----------|
| **SPID (Sistema Pubblico di Identità Digitale)** | Italian national digital identity system for secure authentication. Not yet implemented by OhMyUniversity; planned as a future authentication option alongside institutional credentials. |
| **CIE (Carta d'Identità Elettronica)** | Italian electronic ID card supporting SPID authentication. Relevant to the same future authentication roadmap as SPID. |
| **AgID (Agenzia per l'Italia Digitale)** | Italian government agency defining mandatory digital accessibility and usability guidelines for public sector services, including universities. OhMyUniversity must comply with AgID standards. |
| **GDPR (General Data Protection Regulation)** | EU Regulation 2016/679 on personal data protection. Mandates lawful basis for processing, data minimization, storage limitation, data subject rights (access/export/delete), and breach notification within 72 hours. See SDD 3.5 / RAD 3.3.8 for how OhMyUniversity implements these requirements. |

---

## Acronyms (Italian & Regulatory Context)

| Acronym | Expansion | Definition |
|---------|-----------|-----------|
| **AgID** | Agenzia per l'Italia Digitale | Italian digital agency setting accessibility/usability standards |
| **API** | Application Programming Interface | Software interface for component communication |
| **AWS** | Amazon Web Services | Cloud provider being considered for future deployment (not yet implemented) |
| **CFU** | Credito Formativo Universitario | Italian academic credit unit (≈ ECTS) |
| **CIE** | Carta d'Identità Elettronica | Italian electronic ID card |
| **CINECA** | Consorzio Interuniversitario | Italian consortium and IT systems provider for universities |
| **DG-X** | Design Goal X | References Design Goals (e.g., DG-1 = Data Integrity) |
| **ECTS** | European Credit Transfer System | Standardized academic credit system |
| **EDP** | European Data Portal | Central EU infrastructure for open data |
| **Esse3** | [CINECA subsystem] | Student information system (exams, grades, records); currently the only integrated career data source |
| **GDPR** | General Data Protection Regulation | EU personal data protection regulation |
| **GOMP** | [Proprietary subsystem] | Academic records and career management system, parallel to Esse3; not yet integrated |
| **HPA** | Horizontal Pod Autoscaler | Kubernetes auto-scaling mechanism; relevant only if/when Kubernetes deployment is adopted |
| **JWT** | JSON Web Token | Access token format used for authenticating requests to the backend |
| **MVP** | Minimum Viable Product | Initial release with core features |
| **RAD** | Requirements Analysis Document | Phase 1 documentation (requirements) |
| **RBAC** | Role-Based Access Control | Authorization model based on user roles |
| **RTO** | Recovery Time Objective | Target system restore time after failure |
| **SDD** | System Design Document | Phase 2 documentation (design)—this document |
| **SLA** | Service Level Agreement | Commitment to uptime/performance targets |
| **SPID** | Sistema Pubblico di Identità Digitale | Italian national digital identity; not yet implemented, planned for a future iteration |
| **SSO** | Single Sign-On | Unified authentication across systems |
| **WCAG** | Web Content Accessibility Guidelines | W3C accessibility standards (Level AA required) |

---