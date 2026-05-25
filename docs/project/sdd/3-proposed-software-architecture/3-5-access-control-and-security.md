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

The system handles user identity through a dual-flow authentication architecture, completely separating institutional users from external corporate entities.

- **Institutional Authentication (Students & Staff):** University students, academic staff (Professors), and administrative staff (University Administrators) have two seamless options to authenticate. They can log in using the national Identity Providers (**SPID and CIE**) via standard protocols like OpenID Connect (OIDC) or SAML. Alternatively, they can use their standard **Institutional Credentials (Email and Password)** directly linked to their official university portal (via LDAP or Single Sign-On). The system acts as a secure pass-through and does not store these passwords locally, ensuring full compliance with GDPR Article 32.
- **Corporate Authentication (Partners):** Corporate partners do not possess institutional accounts and they cannot self-register. Their accounts are exclusively provisioned and created by the OhMyUniversity! **System Administrators**. Access to the platform is tied to a subscription model: the account is set to an **Active** state upon payment of the monthly fee, granting full access to the partnership tools. If the monthly fee is unpaid, the account is immediately switched to a **Disabled** state, revoking operational access.

## Authorization and Role-Based Access Control (RBAC)

To govern permissions, the system implements a strict Role-Based Access Control (RBAC) model. The authorization logic is enforced by the _Auth & User Session Service_ before any request is routed to the internal business logic.

Following the principles of Object-Oriented System Design, permissions are formalized using a **Global Access Matrix** mapped directly to the core Domain Classes (or aggregates of similar domain objects). The matrix defines the exact operations (`Read`, `Write`, `Delete`, `Approve`, `None`) that each actor can perform on these entities.

## Global Access Matrix (Domain Object Level)

| Actor / Role                        | `UserProfile` / `PartnerOrg`         | `AcademicCareer` / `ExamSession` | `ClassroomBooking`                   | `NoticeboardItem` / `DidacticMaterial`       | `ChatMessage`                   |
| :---------------------------------- | :----------------------------------- | :------------------------------- | :----------------------------------- | :------------------------------------------- | :------------------------------ |
| **University Student**              | `Read`, `Write` (Own)                | `Read` (Own data only)           | `Read` (View only)                   | `Read`                                       | `Read`, `Write`, `Delete` (Own) |
| **University Professor**            | `Read`, `Write` (Own)                | `Read` (Sessions)                | `Read`, `Write`                      | `Read`, `Write`, `Delete` (Own courses)      | `Read`, `Write`, `Delete` (Own) |
| **University Staff (Ateneo)**       | `Read`                               | `Read` (Support view)            | `Read`, `Approve`, `Write`, `Delete` | `Read`, `Write`, `Delete` (Official notices) | `None` (Privacy restriction)    |
| **OhMyUniversity! System Admin**    | `Read`, `Write`, `Approve`, `Delete` | `None` (Privacy restriction)     | `None`                               | `Read`, `Delete` (Platform Moderation)       | `None` (Privacy restriction)    |
| **Corporate Partner (Active)**      | `Read`, `Write` (Own)                | `None`                           | `Read`, `Write`                      | `Read`, `Write`, `Delete` (Own promos)       | `Read`, `Write`                 |
| **Corporate Partner (Disabled)**    | `Read` (Own Profile)                 | `None`                           | `None`                               | `None` (Subscription expired)                | `None`                          |
| **External Systems (Cineca/Esse3)** | `None`                               | `Write` (Sync source of truth)   | `None`                               | `None`                                       | `None`                          |

_Note on Administration:_

- _The **University Staff** represents the institutional administrative core of the university. They handle campus logistics (`ClassroomBooking`) and the publication of official university communications (`NoticeboardItem`), but they do not manage the platform's subscriptions._
- _The **OhMyUniv! System Admin** represents the technical maintainers of the platform. They hold broad privileges for provisioning Corporate Partner accounts (`Approve`/`Write` on `PartnerOrg`) and moderating content, but strictly do not have access to academic records or private chats._

## Data in Transit Security

All communications between the Multi-platform Client Applications (Flutter/Angular) and the API Gateway, as well as the external API calls orchestrated by the External Integration Gateway (towards Cineca, Moodle, and Map Providers), are heavily encrypted in transit using the HTTPS/TLS 1.3 protocol. This prevents Man-In-The-Middle (MITM) attacks and ensures the confidentiality of academic records during synchronization.
