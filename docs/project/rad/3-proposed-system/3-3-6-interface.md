---
title: RAD - 3.3.6 Interface | OhMyUniversity!
description: Interface requirements of OhMyUniversity - defining external API integration, secure communication, and digital identity authentication standards.
head:
  - - meta
    - property: og:title
      content: RAD - 3.3.6 Interface | OhMyUniversity!
  - - meta
    - property: og:description
      content: Interface requirements of OhMyUniversity - defining external API integration, secure communication, and digital identity authentication standards.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/rad/3-proposed-system/3-3-6-interface
  - - meta
    - name: keywords
      content: ohmyuniversity, rad, interface, nonfunctional requirements, external integration, api, cineca, esse3, moodle, tls, security, authentication, spid, cie, oidc, saml, university app
  - - meta
    - name: twitter:title
      content: RAD - 3.3.6 Interface | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Interface requirements of OhMyUniversity - defining external API integration, secure communication, and digital identity authentication standards.
---

# OhMyUniversity! - RAD: 3.3.6 Interface

The interface requirements define how **OhMyUniversity!** must communicate with external systems and authentication providers. Since the platform acts as a middleware between the user and university services, external integrations must be controlled, secure, and compliant with institutional standards.

- **External Integration:** The system must communicate with university webservices through official and authorized interfaces provided by the relevant institutional systems, such as Cineca/Esse3, Moodle, or other approved university platforms. Direct access to external databases, unofficial data extraction, or uncontrolled manipulation of external information must be avoided.

- **Security:** All communications between the client, the backend, and external services must be protected through secure communication mechanisms. The system must preserve the confidentiality and integrity of transmitted data, especially when handling personal, academic, or administrative information.

- **Authentication:** Access to the system must support secure authentication mechanisms compatible with university identity management services and, where applicable, national digital identity systems such as SPID or CIE. The authentication process must ensure that only authorized users can access protected services and personal academic data.

- **Interface Consistency:** The system must expose and consume interfaces that are clearly defined, documented, and consistent across its modules. This reduces integration errors, simplifies maintenance, and supports the future extension of the platform with additional university services.
