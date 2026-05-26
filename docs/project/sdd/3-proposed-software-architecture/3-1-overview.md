---
title: SDD - 3. Proposed Software Architecture | OhMyUniversity!
description: Overview and table of contents for the proposed software architecture section, covering subsystem decomposition, hardware-software mapping, data management, security, and boundary conditions.
head:
  - - meta
    - property: og:title
      content: SDD - 3. Proposed Software Architecture | OhMyUniversity!
  - - meta
    - property: og:description
      content: Overview and table of contents for the proposed software architecture section.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-1-overview
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, software architecture, design, subsystems, layers
  - - meta
    - name: twitter:title
      content: SDD - 3. Proposed Software Architecture | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Overview and table of contents for the proposed software architecture section.
---

# OhMyUniversity! - SDD: 3.1 Overview

## Overview

This section presents the **proposed software architecture** for the OhMyUniversity! system, describing how the system's complexity is managed through a layered, modular design. The architecture is built on principles of high cohesion and low coupling, utilizing a multi-tier pattern to clearly separate concerns across Presentation, Application Logic, Data & Caching, and External Services layers.

The proposed architecture translates the functional and non-functional requirements into a concrete design that guides implementation across all platforms (Mobile, Web, Desktop) and ensures scalability, maintainability, and reliability.

---

## Table of Contents

### [3.2 Subsystem Decomposition](./3-2-subsystem-decomposition.md)

Describes the decomposition of the OhMyUniversity! system into manageable, highly cohesive, and loosely coupled subsystems. This section details the nine primary subsystems, their responsibilities, domain classes, and how they are distributed across the multi-tier architecture. Includes the Component Diagram showing static module relationships.

### [3.3 Hardware-Software Mapping](./3-3-hardware-software-mapping.md)

Presents the mapping between the logical software architecture and the physical hardware deployment. Details the distribution of components across different nodes, network interactions, and deployment topology.

### [3.4 Persistent Data Management](./3-4-persistent-data-management.md)

Covers the data persistence strategy, database schema design, data models, and caching mechanisms. Explains how data is stored, retrieved, and cached to ensure system reliability and performance.

### [3.5 Access Control and Security](./3-5-access-control-and-security.md)

Describes the security architecture, including authentication mechanisms (SPID/CIE integration), authorization strategies (RBAC), data protection measures, and secure communication protocols.

### [3.6 Global Software Control](./3-6-global-software-control.md)

Details the global control flow, system initialization, shutdown procedures, error handling, and exception management strategies across the entire system.

### [3.7 Boundary Conditions](./3-7-boundary-conditions.md)

Addresses the system's behavior at boundary conditions, including startup, shutdown, restart, and recovery procedures. Covers how the system handles edge cases and failure scenarios.

---
