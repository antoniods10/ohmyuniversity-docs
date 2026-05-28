---
title: RAD - 4 Glossary | OhMyUniversity!
description: Glossary of critical terms used in OhMyUniversity RAD documentation, including business domain terminology and system-specific concepts.
head:
  - - meta
    - property: og:title
    - content: RAD - 4 Glossary | OhMyUniversity!
  - - meta
    - property: og:description
    - content: Definitions of critidocs(rad), enhance functional requirements for organization and logistics modulecal terms in OhMyUniversity requirements and business domain.
  - - meta
    - property: og:url
    - content: https://docs.university.ohmyopensource.org/project/rad/4-glossary/4-glossary
  - - meta
    - name: keywords
    - content: ohmyuniversity, rad, glossary, terminology, definitions, business terms
  - - meta
    - name: twitter:title
    - content: RAD - 4 Glossary | OhMyUniversity!
  - - meta
    - name: twitter:description
    - content: Glossary of OhMyUniversity RAD terms.
---

# OhMyUniversity! - RAD: 4 Glossary

This glossary defines critical business and system-specific terms used throughout the RAD documentation to ensure consistent understanding across the stakeholder community (university administrators, students, partners, development team).

---

## Core Business Terms

### **Academic Career**

The record of a student's academic progression at the university, including exams passed, grades obtained, ECTS earned, and degree plan. Stored and managed by the external CINECA/Esse3 system; OhMyUniversity displays and synchronizes this data locally.

### **CFU (Credito Formativo Universitario)**

Italian academic credit unit equivalent to ECTS. One CFU ≈ 25 study hours. Bachelor degrees require 180 CFU; Master's degrees require 120 CFU.

### **ECTS (European Credit Transfer System)**

Standardized academic credit system used internationally. OhMyUniversity displays ECTS for international contexts; CFU for Italian universities.

### **Eligibility**

The state of a student meeting specific criteria to participate in or access a service. Examples:

- **Master's eligibility**: Student has completed required prerequisite exams/ECTS for a specific Master's program.
- **Classroom booking eligibility**: Student is enrolled and not already booked for that time slot.

### **Master's Requirements**

Formal admission criteria for a Master's degree program, specified by the university. May include:

- Minimum ECTS in specific scientific-disciplinary sectors
- Specific bachelor specializations
- Minimum grade thresholds
- Language proficiency certifications

In MVP, OhMyUniversity **displays requirements for consultation only** (no automatic eligibility verification). Students manually check their status.

### **Study Plan (Piano di Studi)**

The official list of courses (compulsory and elective) that a student is enrolled in for a degree program. May be modified by the student within university constraints.

---

## Partner & Administration Terms

### **Partner Organization**

A company, institution, association, or collective entity external to the university that participates in the OhMyUniversity partnership area, publishing job offers, agreements, discounts, and events for students.

### **Partner Provisioning (Account Activation)**

The administrative process of **authorizing a partner organization to access the system**. Occurs **after** a partner submits a registration request and is validated by a System Administrator. Partners do **not** self-register and immediately gain access; they must be explicitly provisioned.

### **Registration Request (Partner)**

A request submitted **by a partner organization** to participate in the partnership area. Request is stored in **PENDING_APPROVAL** status until a System Administrator reviews and activates the partner account. See also: **Partner Provisioning**.

### **Partner Validation**

The administrative review process of a pending partner registration request, assessing legitimacy, alignment with institutional policies, and completeness of required information before provisioning.

---

## Technical & Operational Terms

### **Offline Mode**

Capability for the OhMyUniversity mobile app to function with limited connectivity or no internet connection, displaying locally cached data (e.g., exam schedules, classroom maps) from the previous sync. Write operations (e.g., booking exam) are queued and executed upon reconnection.

### **Synchronization (Sync)**

The process of **fetching the latest data from external systems** (primarily Cineca/Esse3) and **updating OhMyUniversity's local database**. For example:

- **Academic Career sync**: Fetches latest transcript, grades, ECTS from Cineca.
- **Exam session sync**: Refreshes available exam dates and times.

Sync is triggered automatically on app launch and on user request. Target: p99 < 2.0 seconds (SDD Design Goal Performance).

### **Availability**

The state indicating whether a resource (exam session, classroom, canteen seating) is unbooked and can be reserved. Availability is dynamic and updates in real-time as bookings are made.

### **Booking**

A confirmed reservation of a resource (exam session, classroom, canteen meal) by a student. Once booked, the resource is reserved for that student and unavailable to others (no overbooking).

### **No Overbooking Constraint**

Guarantee that a single exam session, classroom, or canteen seat cannot be booked by more than one student simultaneously. If two booking attempts occur concurrently, only one succeeds; the second user is notified of unavailability.

---

## Data & Privacy Terms

### **Data Minimization**

Principle of collecting and storing only the personal data necessary for the system to function. OhMyUniversity minimizes collection of sensitidocs(rad): enhance functional requirements for organization and logistics moduleve student information (SSN, addresses) and implements retention limits per GDPR Article 5.

### **Retention Policy**

Rules governing how long data is stored before automatic deletion. Example: Grade data retained for academic purposes + 1 year; chat messages deleted after 6 months (unless flagged as important).

### **Audit Trail**

Immutable log of all administrative actions (partner activation, system parameter changes, user access) recorded for compliance and troubleshooting. Retained per GDPR Article 32 requirements.

---

## External System Terms

### **Cineca**

Italian national IT systems provider for universities. OhMyUniversity integrates with CINECA's Esse3 (student records), ANS (authentication), and GOMP (career management) systems via REST APIs.

### **Esse3**

CINECA's student information system managing exams, grades, study plans, and administrative records. OhMyUniversity syncs academic career data from Esse3.

### **Moodle**

Learning management system providing course materials, online learning spaces, and assignment management. OhMyUniversity provides direct access and integration with Moodle for students.

### **External Integration Gateway**

OhMyUniversity component acting as proxy/adapter for calls to external systems (Cineca, Moodle, Canteen), handling authentication, format adaptation, retry logic, and fallback.

---
