---
title: ODD - 1.1 Object Design Trade-offs | OhMyUniversity!
description: Documentation of the main compromise decisions (trade-offs) at the object design and detailed architecture level for the OhMyUniversity! project.
head:
  - - meta
    - property: og:title
      content: ODD - 1.1 Object Design Trade-offs | OhMyUniversity!
  - - meta
    - property: og:description
      content: Documentation of the main compromise decisions (trade-offs) at the object design and detailed architecture level for the OhMyUniversity! project.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/1-introduction/1-1-object-design-trade-offs
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, object design, trade-offs, architecture, caching, proxy pattern, encapsulation
  - - meta
    - name: twitter:title
      content: ODD - 1.1 Object Design Trade-offs | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Documentation of the main compromise decisions (trade-offs) at the object design and detailed architecture level for the OhMyUniversity! project.
---

# 1.1 Object Design Trade-offs

This section documents the main compromise decisions (trade-offs) at the object design and detailed architecture level adopted by the development team for the OhMyUniversity! project. These decisions guide and implementation to ensure a proper balance among performance, maintainability, and development time.

### Component Reuse (Buy) vs. Internal Development (Build)

The system adopts a hybrid approach by restricting custom development strictly to core domain value. For standardized cross-cutting functionalities—such as network request serialization, JWT session handling, and OAuth2 communication protocols with university middleware—the design relies entirely on mature, audited open-source libraries (Component Reuse). Conversely, internal development is dedicated to building custom translation layers, API wrappers, and aggregation services. This design choice ensures that the system can map the raw, heterogeneous responses from university backends into a unified, stable internal object model without relying on rigid, third-party integration platforms.

### Memory Space vs. Response Time (Caching and Offline Fallback)

Because the application acts as an intermediary to legacy external APIs that are prone to high network latency and downtimes, reducing user response time and ensuring system availability are heavily prioritized over minimizing local memory footprints. The object design incorporates an advanced in-memory caching layer (Redis) to store temporary JSON representations of external university data (e.g., AcademicCareer, ExamSession). This choice drastically reduces redundant downstream API calls and allows the application to fulfill the fallback requirement by serving the "last known good state" to the user in offline mode, explicitly trading memory space for extreme responsiveness.

### Performance and Reliability vs. Data Integrity and Security

The master integrity of official academic records remains offloaded to the university's systems. However, security is paramount for both the native data managed by the app and the transient session tokens. The architecture explicitly accepts computational overhead to implement strict in-transit encryption and runtime input sanitization. Furthermore, to balance high performance caching with GDPR Article 32 compliance, the object design enforces a strict 1-hour Time-To-Live (TTL) on all sensitive external data cached in memory. This specific compromise ensures that the exposure window of student records is minimized in the event of a memory compromise, prioritizing security over indefinite caching performance.

### Data Integrity vs. Scalability

The system manages a dual-tier persistence strategy requiring a careful balance. For core data generated natively within the platform (e.g., UniversityUser profiles, NoticeboardItem, PartnerOrganization, and ChatMessage), the design guarantees maximum Data Integrity through rigid ACID transactions and relational coupling utilizing a Relational Database Management System (PostgreSQL). Conversely, for read-heavy operations concerning external academic data, the system explicitly favors Scalability. By routing these requests through the highly scalable Redis caching buffer, the system absorbs heavy concurrent traffic spikes without overloading either the native relational database or the external Cineca/Esse3 infrastructure.

### Information Hiding vs. Access Path Efficiency

To maximize long-term maintainability, the design enforces encapsulation by hiding the complex, deeply nested JSON structures returned by external university endpoints behind clean, domain-specific internal interfaces. However, to optimize rendering performance and prevent high memory usage during complex multi-endpoint object mapping (such as assembling the main student dashboard view), the architecture selectively derogates from pure information hiding. In these specific access paths, the system maps raw API data directly into flat, lightweight Data Transfer Objects (DTOs), cutting down object traversal depth and streamlining data delivery to the client.

### Composition vs. Inheritance

To model client-side user sessions and the varying roles dynamically returned by university API endpoints—such as an individual being simultaneously a student and a teaching assistant—the design strongly favors object composition over inheritance. Rather than implementing rigid, deep class hierarchies that would break whenever external API models change, user permissions and specific behaviors are encapsulated into independent, interchangeable components. These components are aggregated dynamically within a base user session object, allowing the middleware to adapt seamlessly to complex real-world roles without structural rewrites.

### Lazy Loading vs. Eager Loading (Proxy Pattern)

Given that every academic data point requires an external network trip to third-party endpoints, loading full resource dependencies eagerly would introduce massive cumulative latency. The system resolves this by applying a lazy loading strategy implemented via the Proxy Pattern. Lightweight metadata, such as course names or assignment titles, is loaded eagerly to ensure instant interface rendering; meanwhile, the heavy, detailed payloads, including full lesson contents or associated document attachments, are deferred and requested from the university APIs only when explicitly triggered by a user action, optimizing local bandwidth and application velocity.
