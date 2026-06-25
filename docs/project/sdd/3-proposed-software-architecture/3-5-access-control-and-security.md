---
title: SDD - 3.5 Access Control and Security | OhMyUniversity!
description: Definition of the authentication mechanisms, RBAC authorization, and global access matrix based on domain objects for the OhMyUniversity! system.
head:
  - - meta
    - property: og:title
      content: SDD - 3.5 Access Control and Security | OhMyUniversity!
  - - meta
    - property: og:description
      content: Definition of the authentication mechanisms, RBAC authorization, and global access matrix based on domain objects for the OhMyUniversity! system.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-5-access-control-and-security
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, access control, security, rbac, authentication, spid, cie, authorization, global access matrix, domain classes
  - - meta
    - name: twitter:title
      content: SDD - 3.5 Access Control and Security | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Definition of the authentication mechanisms, RBAC authorization, and global access matrix based on domain objects for the OhMyUniversity! system.
---

# OhMyUniversity! - SDD: 3.5 Access Control and Security

## Authentication Mechanisms

Today, OhMyUniversity! has a single authentication mechanism for institutional users (students and
professors): the user submits their institutional credentials, which are verified against
**Esse3**. On success, `api-core` issues a short-lived **JWT** (90 minutes) and a **refresh
token**; the client refreshes the JWT before expiry without requiring the user to re-enter
credentials. On first successful login, a local `OmuUser` profile is created and linked to the
corresponding Esse3 career(s) via fiscal code, allowing a single student with multiple careers
(e.g. Bachelor's and Master's) to be recognized as one OMU identity.

There is currently no separate username/password store, no LDAP integration, and no second-factor
authentication mechanism—the system has no local credentials to protect, since Esse3 remains the
sole identity source.

> **Roadmap (not yet implemented):** SPID/CIE as an additional or primary authentication method,
> per AgID guidelines (SDD 1.4). When introduced, it would coexist with—not replace—the existing
> Esse3-backed login.

Corporate partners follow a separate flow, unrelated to Esse3: they cannot self-register and gain
immediate access. A registration request is submitted and held in a pending state until a System
Administrator reviews and provisions the account (RAD: Partner Provisioning). Once active, the
account's status is tied to the partnership subscription.

## Authorization

Both students and professors can authenticate today, since both hold an Esse3 institutional
identity. However, **no application functionality is currently implemented specifically for the
Professor role**—professors can log in, but the features described in the functional requirements
(career view, exam booking, calendar, etc.) are built and exposed for the Student role only. Any
professor-facing capability (e.g. managing exam sessions, publishing didactic material) is a
planned extension, not a current capability, and is marked as such in the matrix below.

## Global Access Matrix (Domain Object Level) — Implemented

| Actor / Role | `OmuUser` / `UniversityConnection` | Esse3-derived data (Career, Exams, Fees) | `CalendarEvent` | `PartnerOrganization` |
|---|---|---|---|---|
| **Student** | Read, Write (own) | Read (own, via Esse3 sync) | Read, Write (own) | Read |
| **Professor** *(login only — no dedicated features implemented)* | Read, Write (own) | Read (own, via Esse3 sync) | — | — |
| **OhMyUniversity! System Admin** | Read | None (no access to academic records) | None | Read, Write, Approve (provisioning) |
| **Corporate Partner (Active)** | Read, Write (own profile) | None | None | Read, Write (own) |
| **Corporate Partner (Pending/Disabled)** | Read (own profile) | None | None | None |
| **External System (Esse3)** | None | Source of truth (read by `api-core`) | None | None |

## Global Access Matrix — Roadmap (planned, not yet implemented)

The original access control design anticipated additional roles and entities tied to the Classroom
booking, Chat, and Partner Convention modules (SDD 3.2), none of which are implemented today. The
matrix below is preserved as the original design intent and should be honored/revisited once those
modules are built, rather than treated as current system behavior:

| Actor / Role | `ClassroomBooking` | `NoticeboardItem` / `DidacticMaterial` | `ChatMessage` |
|---|---|---|---|
| **Student** | Read (view only) | Read | Read, Write, Delete (own) |
| **Professor** | Read, Write | Read, Write, Delete (own courses) | Read, Write, Delete (own) |
| **University Staff (Ateneo)** | Read, Approve, Write, Delete | Read, Write, Delete (official notices) | None (privacy restriction) |
| **OhMyUniversity! System Admin** | None | Read, Delete (platform moderation) | None (privacy restriction) |
| **Corporate Partner (Active)** | Read, Write | Read, Write, Delete (own promos) | Read, Write |
| **Corporate Partner (Disabled)** | None | None (subscription expired) | None |

> **Note on roles:** **University Staff (Ateneo)** represents the institutional administrative
> core of the university, handling campus logistics (`ClassroomBooking`) and official university
> communications (`NoticeboardItem`), without managing platform subscriptions. This role does not
> exist in the system today—there is no corresponding login or permission model implemented for
> it; it is introduced here only as part of the original roadmap design.

## Data in Transit Security

Communication between the client applications (Flutter/Angular) and `api-gateway`, and between
`api-core` and Esse3, uses HTTPS/TLS. No further claims are made here about specific TLS versions
or cipher suites beyond what has been verified in the actual configuration.

> **Roadmap:** the original design anticipated the same HTTPS/TLS protection extending to calls
> toward Moodle and map/transportation providers, orchestrated through a dedicated external
> integration layer. Today there are no such calls to protect—Moodle is only a link (SDD 1.1) and
> the Transportation service is not yet connected (SDD 3.2)—but the same protection should apply
> to those calls once they exist.