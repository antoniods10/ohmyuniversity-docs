---
title: SDD - Glossary | OhMyUniversity!
description: Glossary of critical terms and architectural concepts used in the OhMyUniversity! System Design Document (SDD).
head:
  - - meta
    - property: og:title
    - content: SDD - Glossary | OhMyUniversity!
  - - meta
    - property: og:description
    - content: Definitions of architectural, infrastructural, and security terms of OhMyUniversity!.
  - - meta
    - property: og:url
    - content: https://docs.university.ohmyopensource.org/project/sdd/5-glossary/5-glossary
  - - meta
    - name: keywords
    - content: ohmyuniversity, sdd, glossary, architecture, microservices, definitions, infrastructure
  - - meta
    - name: twitter:title
    - content: SDD - Glossary | OhMyUniversity!
  - - meta
    - name: twitter:description
    - content: Glossary of OhMyUniversity SDD architectural concepts.
---

# OhMyUniversity! - SDD: Glossary

This glossary defines the critical technical, architectural, and infrastructural terms used in the System Design Document (SDD). Its purpose is to ensure a consistent understanding of the design choices and technologies adopted across the technical team.

---

## Architectural Models and Patterns

### **Multi-Layer Architecture (3-Tier)**

Logical architectural pattern chosen for OhMyUniversity!, which divides the system into Presentation Layer (UI App), Application Logic (API Gateway and Services), Caching & Database Layer, and External Services Layer.

### **Independent Microservices (Stateless)**

Design approach where the backend is composed of independent services that do not store state in memory, allowing horizontal scalability based on load (e.g., scaling the Logistics Service independently from the Career Service).

### **API Gateway (Main API Gateway)**

Single entry point for client applications (Web and mobile frontend) that routes incoming REST requests to the appropriate internal services, ensuring low coupling between modules.

### **External Integration Gateway**

OhMyUniversity! component acting as an external proxy/adapter orchestrating API calls to third-party systems (such as CINECA, Moodle, map providers), handling data formatting.

---

## Data Management and Persistence

### **RDBMS (PostgreSQL)**

Relational database chosen to guarantee high data integrity (ACID transactions) for data natively generated within the platform, such as user profiles, partner conventions, and chat messages.

### **In-Memory Caching (Redis)**

High-performance caching layer used to temporarily store JSON representations of external academic data (Cineca). It ensures compliance with response times (< 2.0s) and allows offline viewing in case of external API downtime.

### **TTL (Time-To-Live)**

Maximum lifespan assigned to sensitive academic data stored in the cache (Redis). Strictly set to 1 hour to balance high performance with GDPR Article 32 compliance, minimizing the exposure window in the event of a compromise.

### **Eventual Consistency**

Data consistency model adopted to favor scalability. Updates are propagated via asynchronous events (e.g., Kafka) with bounded consistency windows (e.g., 5 seconds), accepting temporary inconsistency on non-critical paths.

---

## Concurrency and Control

### **Event-Driven Control Flow**

Control mechanism adopted on the Client side (Flutter and Angular Frontend), based on dynamic reaction to external stimuli (e.g., user click or swipe-to-refresh) via asynchronous REST requests.

### **Thread-Based Control Flow**

Control mechanism adopted by the Server (Spring Boot). Each HTTP request receives a separate execution thread, allowing thousands of interactions to proceed in parallel without blocking the global execution.

### **Pessimistic Locking / Optimistic Locking**

Database concurrency management strategies to avoid race conditions.

- **Pessimistic**: Used for critical exclusive resources (e.g., exam or classroom booking) to prevent overbooking.
- **Optimistic**: Used for shared resources with low contention rates (e.g., canteen menu updates).

---

## Security and Authentication

### **RBAC (Role-Based Access Control)**

Hierarchical model for permissions management formalized via a Global Access Matrix, which maps the exact operations (`Read`, `Write`, `Delete`, `Approve`, `None`) granted to actors on domain classes (Students, Professors, Partners, Admins).

### **SPID / CIE**

Italian public digital identity systems and electronic identity card. They represent the primary institutional authentication methods (via OIDC/SAML) provided by the system in compliance with Italian government requirements.
