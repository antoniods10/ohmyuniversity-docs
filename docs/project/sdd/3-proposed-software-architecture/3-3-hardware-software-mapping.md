---
title: SDD - 3.3 Hardware/Software Mapping | OhMyUniversity!
description: Mapping of the OhMyUniversity! software subsystems to the underlying physical and cloud infrastructure.
head:
  - - meta
    - property: og:title
      content: SDD - 3.3 Hardware/Software Mapping | OhMyUniversity!
  - - meta
    - property: og:description
      content: Mapping of the OhMyUniversity! software subsystems to the underlying physical and cloud infrastructure.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/sdd/3-proposed-software-architecture/3-3-hardware-software-mapping
  - - meta
    - name: keywords
      content: ohmyuniversity, sdd, hardware mapping, software mapping, deployment diagram, aws, cloud, kubernetes, docker, nodes, agile
  - - meta
    - name: twitter:title
      content: SDD - 3.3 Hardware/Software Mapping | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Mapping of the OhMyUniversity! software subsystems to the underlying physical and cloud infrastructure.
---

# OhMyUniversity! - SDD: 3.3 Hardware/Software Mapping

## Deployment Strategy and Infrastructure

The Hardware/Software mapping illustrates how the logical subsystems of **OhMyUniversity!** are distributed across physical and virtual processing nodes. To satisfy the Non-Functional Requirements of **Reliability (99.9% uptime)** and **Scalability**, the backend infrastructure is designed as a cloud-native distributed system, hosted primarily on Amazon Web Services (AWS).

The deployment strategy relies heavily on containerization (Docker) and orchestration (Kubernetes). This approach allows the system to utilize **Horizontal Pod Autoscalers (HPA)**, dynamically allocating hardware resources to specific microservices during peak usage (e.g., during exam booking periods) without affecting the rest of the system.

_It is important to note that the hardware mapping proposed in this document represents an **initial baseline solution**. As the project evolves through subsequent Sprints and as real-world performance metrics are gathered, this architectural mapping may be iteratively modified. Nodes, cloud services, and allocation strategies could be optimized or replaced to better satisfy emerging requirements and technological advancements._

## Hardware/Software Mapping Diagram

> **[In progress]**

- **Client Nodes (Execution Environment):**
  - **Mobile Device (iOS/Android):** Runs the compiled Flutter application natively. It relies on the device's hardware for local storage (secure storage for session tokens) and UI rendering.
  - **Desktop/Laptop (Web Browser):** Executes the Angular web portal within a standard browser environment.
- **API Gateway Node (AWS Application Load Balancer / API Gateway):**
  - Acts as the physical entry point to the cloud network, terminating SSL/TLS 1.3 connections and routing incoming HTTPS requests to the appropriate backend containers.
- **Application Server Nodes (Kubernetes Worker Nodes):**
  - Virtual machines hosting the Spring Boot microservices (Auth, Academic, Campus, Portal, Partner Services). These nodes are scaled horizontally based on CPU and memory thresholds.
  - Also hosts the **External Integration Gateway**, which physically handles outbound SOAP/REST requests to external institutional APIs.
- **Storage & Caching Nodes (AWS RDS & ElastiCache):**
  - **PostgreSQL Node:** A managed relational database instance guaranteeing ACID properties for native platform data (User Profiles, Partner Organizations, Chat Messages).
  - **Redis Cluster Node:** A high-speed, in-memory data store acting as the caching layer for academic records and logistics data, ensuring the **500ms fallback requirement** is met during external API downtimes.
- **External Institutional Nodes:**
  - **Cineca / Esse3 Servers:** External physical servers managing the official source of truth for academic careers and grades.
  - **SPID / CIE Identity Providers:** Government-hosted secure nodes handling OIDC/SAML authentication flows.
  - **Moodle LMS Servers:** University-hosted or third-party servers providing didactic materials.
