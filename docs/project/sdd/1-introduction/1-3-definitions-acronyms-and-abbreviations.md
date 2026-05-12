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

### **Architectural Patterns (OhMyUniversity-Specific)**

| Term | Definition |
|------|-----------|
| **Cache-Aside Pattern** | Caching strategy: on read, check Redis cache first; if miss, fetch from source and populate cache. Used for CINECA data to achieve < 2.0s sync target. |
| **Middleware Layer** | Software layer acting as intermediary between institutional databases (Cineca, GOMP, Moodle) and student-facing interfaces. OhMyUniversity orchestrates data from multiple sources into unified experience. |
| **Stateless Service** | Microservice that does not store state in memory; all state lives in PostgreSQL, Redis, or MongoDB. Enables horizontal autoscaling. |

### **Italian University Systems**

| Term | Definition |
|------|-----------|
| **CINECA** | Italian national IT systems provider for universities. Manages Esse3 (student/exam data), ANS (authentication), GOMP (career management), and other academic systems. OhMyUniversity integrates with CINECA APIs. |
| **Esse3** | CINECA's student information system storing exams, grades, study plans, administrative data. OhMyUniversity syncs Career data from Esse3. |
| **GOMP** | CINECA subsystem for managing academic records and student careers. Parallel to Esse3; OhMyUniversity integrates both. |
| **ECTS (European Credit Transfer System)** | Standardized academic credit; 1 ECTS ≈ 25-30 study hours. Bachelor = 180 ECTS; Master = 120 ECTS. OhMyUniversity tracks and displays ECTS progress. |
| **CFU (Credito Formativo Universitario)** | Italian equivalent of ECTS; 1 CFU ≈ 25 hours. OhMyUniversity uses CFU for Italian universities, ECTS for international context. |
| **Grade Average (Media Ponderata)** | Weighted mean of exam grades calculated as: (grade1×ECTS1 + grade2×ECTS2 + ...) / (ECTS1 + ECTS2 + ...). OhMyUniversity computes automatically from Esse3 data. |
| **Graduation Projection (Voto Laurea)** | Predicted final degree score (0-110). Calculated as: (grade_average × 110) / 30 + bonus points for early graduation. OhMyUniversity displays for student planning. |
| **Honors/Lode** | Merit mention for 30/30 exam grade; may add points to weighted average per institutional rules. OhMyUniversity flags eligible exams. |
| **Master's Requirements** | Specific exams/ECTS students must complete to be eligible for a particular Master's degree. OhMyUniversity verifies eligibility automatically. |
| **Study Plan (Piano di Studi)** | Official record of courses a student is enrolled in (compulsory + elective). OhMyUniversity displays and allows modification. |

### **Italian Digital Identity & Compliance**

| Term | Definition |
|------|-----------|
| **SPID (Sistema Pubblico di Identità Digitale)** | Italian national digital identity system for secure authentication. Primary authentication method for OhMyUniversity; replaces username/password. |
| **CIE (Carta d'Identità Elettronica)** | Italian electronic ID card supporting SPID authentication. Fallback option if SPID unavailable. |
| **AgID (Agenzia per l'Italia Digitale)** | Italian government agency defining mandatory digital accessibility and usability guidelines for public sector services, including universities. OhMyUniversity must comply with AgID standards. |
| **GDPR (General Data Protection Regulation)** | EU Regulation 2016/679 on personal data protection. Mandates: lawful basis for processing, data minimization, storage limitation (90 days max for grades), data subject rights (access/export/delete within 30 days), breach notification (72 hours). OhMyUniversity implements all requirements. |

---

## Acronyms (Italian & Regulatory Context)

| Acronym | Expansion | Definition |
|---------|-----------|-----------|
| **AgID** | Agenzia per l'Italia Digitale | Italian digital agency setting accessibility/usability standards |
| **API** | Application Programming Interface | Software interface for component communication |
| **AWS** | Amazon Web Services | Cloud provider hosting infrastructure |
| **CFU** | Credito Formativo Universitario | Italian academic credit unit (≈ ECTS) |
| **CIE** | Carta d'Identità Elettronica | Italian electronic ID card |
| **CINECA** | [Italian acronym] | Italian IT systems provider for universities |
| **DG-X** | Design Goal X | References Design Goals (e.g., DG-1 = Data Integrity) |
| **ECTS** | European Credit Transfer System | Standardized academic credit system |
| **EDP** | European Data Portal | Central EU infrastructure for open data |
| **Esse3** | [CINECA subsystem] | Student information system (exams, grades, records) |
| **GDPR** | General Data Protection Regulation | EU personal data protection regulation |
| **GOMP** | [CINECA subsystem] | Academic records and career management system |
| **HPA** | Horizontal Pod Autoscaler | Kubernetes auto-scaling mechanism |
| **LDAP** | Lightweight Directory Access Protocol | Protocol for university user directory queries |
| **MVP** | Minimum Viable Product | Initial release with core features |
| **RAD** | Requirements Analysis Document | Phase 1 documentation (requirements) |
| **RBAC** | Role-Based Access Control | Authorization model based on user roles |
| **RTO** | Recovery Time Objective | Target system restore time after failure |
| **SDD** | System Design Document | Phase 2 documentation (design)—this document |
| **SLA** | Service Level Agreement | Commitment to uptime/performance targets |
| **SPID** | Sistema Pubblico di Identità Digitale | Italian national digital identity |
| **SSO** | Single Sign-On | Unified authentication across systems |
| **TOTP** | Time-based One-Time Password | Standard for one-time code generation (2FA) |
| **WCAG** | Web Content Accessibility Guidelines | W3C accessibility standards (Level AA required) |

---
