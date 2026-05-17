---
title: SDD - 1.2 Design Goals | OhMyUniversity!
description: Design goals for OhMyUniversity establish measurable, prioritized objectives across data integrity, performance, reliability, usability, and scalability to guide architectural decisions.
head:
  - - meta
    - property: og:title
      content: SDD - 1.2 Design Goals | OhMyUniversity!
  - - meta
    - property: og:description
      content: Design goals define the measurable objectives that OhMyUniversity must achieve, prioritized by importance to the university and students.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/1-introduction/1-2-design-goals
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, design goals, performance, reliability, scalability, data integrity, usability, gdpr, accessibility, legal compliance
  - - meta
    - name: twitter:title
      content: SDD - 1.2 Design Goals | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Measurable design goals guiding OhMyUniversity architecture.
---

# OhMyUniversity! - SDD: 1.2 Design Goals

Design Goals translate system requirements into concrete, measurable objectives that drive all architectural decisions. Each goal has a priority, specific metric, and rationale. Trade-offs between conflicting goals are explicitly resolved.

---

## Design Goals (Prioritized)

| Priority | Goal | Metric | Rationale |
|----------|------|--------|-----------|
| **P1** | **Data Integrity** | 100% correspondence with CINECA records; zero calculation errors | Students trust system with academic records; errors impact future opportunities (Master's eligibility, graduation date) |
| **P1** | **Performance** | Sync from CINECA in < 2.0 seconds; 3-tap access for expert users | Expert users expect instant access; reduces friction vs current fragmented portals |
| **P1** | **Reliability** | 99.9% monthly uptime; graceful fallback when external systems unavailable | Students access platform for critical decisions (exam booking, Master's planning); unexpected downtime causes lost registrations |
| **P1** | **Usability** | New users complete classroom booking in < 60 seconds without documentation; intuitive interface | Platform must reduce cognitive load; complexity drives users back to fragmented portals |
| **P2** | **Scalability** | Horizontal scaling of individual microservices without system-wide re-deployment | Each service scales independently based on demand (Logistics overloaded ≠ Career Service scales) |
| **P2** | **Maintainability** | Clear separation of concerns via microservices; consistent coding standards; test coverage > 80% | Team continuity requires clean architecture; future developers must onboard quickly |
| **P2** | **Security & Authentication** | SPID/CIE as primary auth (Italian government mandate); Spring Security + LDAP fallback; 2FA (TOTP) for sensitive operations | Student data is highly sensitive (grades, SSN, addresses); compliance requirement for Italian universities. GDPR/Data Protection Code requires documented access control. SPID/CIE provides unified Italian digital identity. |
| **P1** | **Legal Compliance & Data Protection** | GDPR-compliant data handling; automated data subject rights (access, export, delete) within 30 days | Student data is sensitive; Italian universities must comply with EU GDPR |

---

## Trade-offs & Resolution

### Trade-off 1: Performance (P1) vs Security (P2)
**Conflict**: Caching CINECA data in Redis achieves Performance goal (p99 < 2.0s), 
but stores sensitive student data in memory, creating confidentiality risk under GDPR 
Article 32 if Redis is compromised.

**Solution**: Implement time-bounded cache expiration (TTL = 1 hour). Exposure window 
for cached sensitive data is limited to at most 60 minutes before automatic expiration. 
VPC-only Redis access further restricts attack surface to internal network compromise. 
Residual risk (data accessible if compromised during TTL window) is acceptable because 
exposure is time-limited and requires VPC compromise.

### Trade-off 2: Maintainability (P2) vs Performance (P1)
**Conflict**: Optimizing code for performance (tight loops, manual caching, complex 
data structures) sacrifices readability and maintainability. Conversely, writing clean, 
readable code (clear logic, comments, simple abstractions) may add computational overhead 
that impacts the Performance Design Goal (p99 < 2.0s) on critical paths like CINECA sync 
and grade calculation.

**Solution**: Apply data-driven optimization strategy. Default to readable, maintainable 
code following Google Style Guides. Profile actual bottlenecks (not developer guesses) 
using APM tools. Optimize only measurable critical paths (CINECA sync, grade calculation) 
where overhead impacts Design Goals. Accept 5-10% performance trade-off on non-critical 
paths to preserve code maintainability. Result: team maintains code long-term while 
Performance goal is still met.

### Trade-off 3: Data Integrity (P1) vs Scalability (P2)

Trade-off 3: Data Integrity (P1) vs Scalability (P2)

**Conflict**: Independent microservices enable horizontal scaling per-service 
(Scalability Design Goal), but independent scaling inherently creates eventual 
consistency. When Career Service receives a grade from CINECA, Logistics Service 
reading the cached data simultaneously may see stale data (temporary inconsistency). 
Maintaining 100% instant Data Integrity across independent services requires 
synchronous updates across all services, which prevents independent scaling and 
violates the Scalability Design Goal.

**Solution**: Implement Eventual Consistency model with versioning and bounded 
consistency windows. Career Service receives grade from CINECA → publishes event 
to Kafka (async propagation) → Logistics Service consumes asynchronously (within 
5-second consistency window). During propagation lag, services know data is 
"in-flight" via version tags. Accept bounded temporary inconsistency (5 seconds max) 
on non-critical paths to preserve independent scaling. Critical operations (grade 
finality, Master's eligibility verification) use synchronous validation where needed. 
Result: Scalability goal achieved while Data Integrity protected with explicit 
guardrails and consistency windows.



---

## Design Goals Impact on Key Architectural Decisions

| Design Goal | Architectural Impact | Driven By (NFR from RAD) |
|---|---|---|
| **Data Integrity** | PostgreSQL (ACID transactions), versioning schema, validation layers, periodic checksums | RAD 3.3.2 (Reliability) |
| **Performance** | Redis caching (cache-aside), Spring Boot connection pooling, API projection optimization | RAD 3.3.3 (Performance) |
| **Reliability** | Multi-AZ RDS, ECS health checks, circuit breaker pattern, Kafka for async decoupling | RAD 3.3.2 (Reliability) |
| **Usability** | SPA (Angular), mobile-first (Flutter), logical navigation hierarchy | RAD 3.3.1 (Usability) |
| **Scalability** | Microservices architecture (Independent services), stateless Spring Boot instances, Kubernetes HPA or AWS ECS autoscaling | RAD 3.3.4 (Supportability) |
| **Maintainability** | Google Style Guides (Java/Dart), linting enforcement in CI, > 80% test coverage | RAD 3.3.5 (Implementation) |
| **Security & Authentication** | Spring Security, SPID/CIE provider (Italian government), LDAP fallback, 2FA (TOTP), RBAC (database-driven roles), TLS 1.3, AES-256 encryption, input validation, audit logging | RAD 3.3.6 (Interface) + RAD 3.3.8 (Legal) |

---
