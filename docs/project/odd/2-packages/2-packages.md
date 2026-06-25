---
title: ODD - Object Design Document | OhMyUniversity!
description: Object Design Document of OhMyUniversity - describing object design decisions, package organization, class interfaces, and architectural structure.
head:
  - - meta
    - property: og:title
      content: ODD - Object Design Document | OhMyUniversity!
  - - meta
    - property: og:description
      content: Object Design Document of OhMyUniversity - describing object design decisions, package organization, class interfaces, and architectural structure.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/2-packages
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, object design document, object design, packages, package organization, class interfaces, software architecture, angular, flutter, spring boot, backend, frontend, mobile, design documentation
  - - meta
    - name: twitter:title
      content: ODD - Object Design Document | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Object Design Document of OhMyUniversity - describing object design decisions, package organization, class interfaces, and architectural structure.
---

# OhMyUniversity! - ODD: 2 Packages

## Organization Overview
The package organization of the first deliverable describes how the current codebase of OhMyUniversity! is organized at architectural level.  
It focuses on the main software components currently involved in the implementation of the system: the Angular Web frontend, the Flutter mobile frontend, and the Java Spring Boot backend.

The purpose of this section is to provide a high-level view of the packages, explaining their role, their responsibilities, and how they contribute to the overall architecture of the system.  
Detailed descriptions of individual classes and methods are not included here, because they are documented in the following section dedicated to class interfaces.

The first deliverable is organized into three main codebases:

```text
OhMyUniversity First Deliverable

├── Angular Web Frontend
├── Flutter Mobile Frontend
└── Java Spring Boot Backend
```

The Angular Web Frontend represents the browser-based client application.  
It is organized under the `src/app` directory and separates application-wide elements, reusable components, and feature-specific sections.

Current high-level structure:

```text
src/app
├── core
├── shared
└── features
```

The Flutter Mobile Frontend represents the cross-platform mobile client for Android and iOS.  
It is organized under the `lib` directory and follows a modular structure inspired by Clean Architecture, separating configuration, reusable core elements, shared widgets, and feature-specific modules.

Current high-level structure:

```text
lib
├── config
├── core
├── features
├── shared
├── app.dart
└── main.dart
```

The Java Spring Boot Backend represents the server-side part of the system.  
It exposes REST APIs to the frontend clients, coordinates the application logic, manages authentication, handles persistence, and isolates communication with external university systems such as Cineca/Esse3.

Current high-level structure:

```text
org.ohmyopensource.ohmyuniversity.core
├── cineca
├── config
├── controller
├── domain
├── dto
└── service
```

The overall package organization follows a modular and layered approach.  
Each codebase separates responsibilities in order to improve maintainability, reduce coupling, and support future extensions of the system.

At the current stage, the backend already contains concrete packages for authentication, JWT management, Cineca/Esse3 integration, academic-career APIs, DTOs, services, configuration, domain entities, repositories, and database migrations.

The Angular Web frontend already provides a modular structure based on `core`, `shared`, and `features` packages.  
The Flutter mobile frontend already provides a structure based on `config`, `core`, `shared`, and `features`, with some features already organized into `data`, `domain`, and `presentation` layers.

Therefore, the package organization distinguishes between:

- packages already present in the current repositories;
- packages partially implemented for the first academic and authentication features

This organization allows the system to evolve incrementally while keeping a clear separation between user interfaces, application logic, domain concepts, persistence, and external integrations.


## Web Frontend Packages
The Web frontend is implemented with Angular and is organized under the `src/app` directory.

The current structure separates application-wide elements, reusable shared elements, and feature-specific sections. This organization keeps the frontend modular and makes each functional area easier to maintain and extend.

Current high-level structure:

```text
src/app
├── core
├── shared
└── features
```


### Core Package

The `core` package contains application-wide elements used by the whole Angular application.

Current structure:

```text
core
├── config
├── guards
├── interceptors
└── layout
    ├── footer
    ├── navbar
    └── public-layout
```

The `core` package is responsible for global configuration, route protection, HTTP interception, and common layout elements.

It should contain only elements that are shared by the entire application and are not specific to a single feature.


### Shared Package

The `shared` package contains reusable elements that can be used by multiple frontend features.

Current structure:

```text
shared
+-- components
+-- constants
+-- data
+-- directives
+-- icons
+-- types
+-- utils
```

The `shared` package is responsible for common UI elements, constants, directives, types, data, icons, and utility functions.

The `data` package contains mock and static datasets used by dashboard and public features, while the `icons` package contains reusable custom icon components.

This package must remain generic. It should not contain business logic specific to a single feature.


### Features Package

The `features` package contains the main user-facing areas of the Angular application.

Current structure:

```text
features
├── about
├── auth
├── business
├── contatti
├── dashboard
├── faq
├── home
├── legal
├── orientation
├── partner
└── pricing
```

Each feature package groups the code related to a specific section of the Web application.

At the current stage, some features are implemented mainly at presentation level, while others already show a layered internal organization.


### Auth Feature Package

The `auth` feature contains the frontend structure for authentication-related pages and logic.

Current structure:

```text
auth
├── application
│   ├── facades
│   └── usecases
├── domain
│   ├── models
│   └── repositories
├── infrastructure
│   └── api
└── presentation
    ├── components
    └── pages
```

This package is partially organized according to a layered structure.

The `presentation` layer contains the UI structure.
The `application` layer is used to coordinate authentication use cases.
The `domain` layer contains authentication-related domain abstractions.
The `infrastructure` layer is prepared for technical communication with backend APIs.


### Dashboard Feature Package

The `dashboard` feature contains the protected student dashboard area of the Web application.

Current structure:

```text
dashboard
+-- application
|   +-- facades
|   +-- mappers
|   +-- usecases
+-- domain
|   +-- models
|   +-- repositories
+-- infrastructure
|   +-- api
+-- presentation
|   +-- layout
|   +-- pages
|   +-- widgets
+-- services
```

This package is partially organized into application, domain, infrastructure, and presentation layers.

The `dashboard` feature contains the protected student area and includes the main delivered Web flows: career, exam sessions, calendar, timetable, portals, secretariat, profile, and university contacts.

The `presentation` layer contains the dashboard pages, layout components, and widgets.
The `application` layer is used to coordinate dashboard-related operations and includes mappers for transforming data into presentation-ready structures.
The `domain` layer contains the main dashboard abstractions.
The `infrastructure` layer is prepared for communication with backend APIs.
The `services` package contains feature-level services used by the dashboard area.


### Orientation Feature Package

The `orientation` feature contains the frontend structure for orientation-related content.

Current structure:

```text
orientation
+-- application
|   +-- state
+-- domain
|   +-- models
+-- presentation
    +-- components
    +-- pages
```

This package is partially implemented.

The `application/state` package manages the local state of the orientation flow.
The `presentation` layer contains the pages and components used to display orientation content.
The `domain` layer contains the basic models used by this feature.


### Presentation-Level Feature Packages

Several features are currently organized mainly at presentation level.

Current packages:

```text
features
├── about
├── business
├── contatti
├── faq
├── home
├── legal
├── partner
└── pricing
```

These packages mainly contain pages and components related to public sections of the Web application.

Their role is to keep each public section separated from the others, avoiding a single large and unstructured frontend module.


### Web Frontend Package Dependencies

The Web frontend follows a general dependency direction based on separation of responsibilities.

The main dependency direction is:

```text
features
↓
shared
↓
core
```

Feature packages can use reusable elements from `shared` and application-wide elements from `core`.

The `shared` package must remain independent from specific business features.

The `core` package provides global infrastructure and should not depend on feature-specific packages.

Route protection is handled by guards in the `core/guards` package, such as `authGuard` and `carrieraGuard`. HTTP authentication concerns are centralized in `core/interceptors`, which avoids duplicating token-handling logic inside feature components.

Inside partially layered features, the dependency direction is:

```text
presentation
↓
application
↓
domain

infrastructure
↓
domain
```

The `presentation` layer uses application-level logic and domain abstractions.
The `application` layer coordinates feature operations.
The `domain` layer contains the central abstractions of the feature.
The `infrastructure` layer provides technical access to backend APIs and depends on domain contracts.

This organization keeps the Angular frontend modular, reduces coupling between features, and separates UI code from application and technical logic.


## Flutter Mobile Frontend Packages

The Flutter mobile frontend is organized under the `lib` directory.

The current structure separates application configuration, reusable core elements, shared widgets, and feature-specific modules. Some features already follow a layered organization based on `data`, `domain`, and `presentation`, while other features are currently implemented mainly at presentation level.

Current high-level structure:

```text
lib
├── config
├── core
├── features
└── shared
```


### Config Package

The `config` package contains application-level configuration used by the mobile app.

Current structure:

```text
config
├── routes
└── theme
```

The `routes` package contains the navigation structure of the mobile application.

The `theme` package contains the visual configuration of the app, such as colors, typography, and general theme settings.

This package centralizes configuration elements that must be available across the whole Flutter application.


### Core Package

The `core` package contains reusable elements that are not specific to a single feature.

Current structure:

```text
core
+-- config
+-- constants
+-- error
+-- network
+-- usecases
```

The `config` package contains reusable core-level configuration.

The `constants` package contains application-wide constants.

The `error` package contains common error and failure abstractions used by the application.

The `network` package contains shared networking utilities used by data sources and repositories.

The `usecases` package contains generic use case structures shared by different features.

The `core` package provides common architectural elements and should not contain UI code or feature-specific business logic.


### Shared Package

The `shared` package contains reusable UI elements shared by multiple mobile features.

Current structure:

```text
shared
+-- mocks
+-- widgets
```

The `mocks` package contains reusable mock data used by presentation and development flows.

The `widgets` package contains common interface components used in different areas of the mobile application.

This package avoids duplication of UI elements and keeps common presentation components separated from feature-specific code.


### Features Package

The `features` package contains the main functional areas of the Flutter mobile application.

Current structure:

```text
features
+-- academics
+-- academic_career
+-- auth
+-- calendar
+-- chat
+-- companies
+-- email
+-- explore
+-- favorites
+-- home
+-- notifications
+-- onboarding
+-- orientation
+-- profile
+-- services
+-- splash
+-- timetable
```

Each feature package groups the code related to a specific area of the mobile application.

At the current stage, some features are partially implemented with a complete layered structure, while others are mainly organized around presentation-level pages and widgets.


### Academics Feature Package

The `academics` feature contains the main mobile implementation for academic career, exams, study plan, appeals, statistics, and student-administration data.

Current structure:

```text
academics
+-- data
|   +-- datasources
|   +-- mocks
|   +-- models
|   +-- repositories
+-- domain
|   +-- entities
|   +-- exceptions
|   +-- repositories
|   +-- services
|   +-- usecases
+-- presentation
    +-- pages
    +-- providers
    +-- utils
    +-- views
    +-- widgets
```

This feature follows a layered organization based on `data`, `domain`, and `presentation`.

The `data` layer contains technical access to backend APIs, mock data, models, and repository implementations.

The `domain` layer contains entities, repository contracts, exceptions, domain services, and use cases.

The `presentation` layer contains pages, providers, utilities, views, and widgets used to display academic information and exam-session flows.

### Auth Feature Package

The `auth` feature contains the mobile structure for authentication.

Current structure:

```text
auth
├── data
│   ├── datasources
│   ├── models
│   └── repositories
├── domain
│   ├── entities
│   ├── repositories
│   └── usecases
└── presentation
    ├── pages
    └── providers
```

This package is partially implemented with a layered structure.

The `domain` layer contains authentication-related abstractions and repository contracts.

The `data` layer contains the technical structures used to retrieve and manage authentication data.

The `presentation` layer contains the pages and providers used to manage the authentication flow in the mobile interface.

### Calendar Feature Package

The `calendar` feature manages the integrated calendar flow.

Current structure:

```text
calendar
+-- data
|   +-- datasources
|   +-- models
|   +-- repositories
+-- domain
|   +-- entities
|   +-- exceptions
|   +-- repositories
|   +-- usecases
+-- presentation
    +-- pages
    +-- providers
    +-- widgets
```

This feature separates remote or local data access, domain entities, use cases, providers, pages, and widgets.


### Services Feature Package

The `services` feature manages the mobile portals flow and external university services.

Current structure:

```text
services
+-- data
|   +-- datasources
|   +-- models
|   +-- repositories
+-- domain
|   +-- entities
|   +-- exceptions
|   +-- repositories
|   +-- usecases
+-- presentation
    +-- data
    +-- models
    +-- pages
    +-- providers
```

This feature contains data sources, models, repositories, domain entities, use cases, providers, pages, and presentation-specific data for university portals and external service access.


### Home Feature Package

The `home` feature contains the main mobile home area.

Current structure:

```text
home
└── presentation
    ├── pages
    └── widgets
```

This package is currently implemented mainly at presentation level.

It contains the mobile home page structure and the widgets used to organize the main entry point of the application.


### Onboarding and Splash Feature Packages

The `onboarding` and `splash` features manage the initial access flow of the mobile application.

Current structure:

```text
onboarding
├── data
├── domain
└── presentation

splash
└── presentation
```

The `onboarding` feature is partially organized into data, domain, and presentation areas.

The `splash` feature is currently implemented mainly at presentation level.

Together, these packages support the first user interaction with the mobile app before accessing the main application areas.


### Orientation Feature Package

The `orientation` feature contains the mobile orientation experience.

Current structure:

```text
orientation
+-- data
+-- domain
|   +-- entities
|   +-- services
+-- presentation
    +-- pages
    +-- providers
    +-- utils
    +-- widgets
```

This feature contains domain entities, scoring services, providers, topic pages, utility logic, and widgets used by the mobile orientation flow.

The `email`, `profile`, and `timetable` features are also organized with `data`, `domain`, and `presentation` layers. They support institutional email access, student profile visualization, and class timetable consultation.


### Presentation-Level Feature Packages

Several mobile features are currently organized mainly at presentation level.

Current packages:

```text
features
+-- companies
+-- chat
+-- explore
+-- favorites
+-- home
+-- notifications
+-- onboarding
+-- splash
```

These packages contain presentation-oriented or initial structures for additional mobile areas of the application.

Their role is to keep each user-facing section separated from the others, avoiding a single large and unstructured mobile module.


### Flutter Mobile Package Dependencies

The Flutter mobile frontend follows a general dependency direction based on separation of responsibilities.

At application level, the main dependency direction is:

```text
features
↓
shared
↓
core
```

Feature packages can use reusable widgets from `shared` and common architectural elements from `core`.

The `shared` package must remain independent from specific business features.

The `core` package provides common structures and should not depend on feature-specific packages.

Inside partially layered features, the dependency direction is:

```text
presentation
↓
domain

data
↓
domain
```

The `presentation` layer depends on domain abstractions and uses state-management structures to connect the UI with the feature logic.

The `domain` layer contains entities, repository contracts, and use cases. It must remain independent from Flutter UI details and technical data access.

The `data` layer implements technical access to data sources and depends on the contracts defined by the domain layer.

This organization keeps the Flutter mobile frontend modular, separates UI code from domain logic, and allows individual features to evolve independently.


## Java Spring Boot Backend Packages

The Java Spring Boot backend is organized into multiple coordinated repositories rather than a single backend package.

Current high-level backend structure:

```text
Backend
+-- ohmyuniversity-api-gateway
+-- ohmyuniversity-api-core
+-- ohmyuniversity-api-fetcher
+-- ohmyuniversity-infra
```

The `ohmyuniversity-api-gateway` repository is the public entry point for Web and Mobile clients. It contains gateway configuration, security filters, CORS configuration, and route definitions. Its responsibility is to expose public API routes and forward requests to the correct backend service.

The `ohmyuniversity-api-core` repository contains the main application logic of OhMyUniversity!. It includes authentication, JWT management, Cineca/Esse3 integration, academic-career APIs, calendar management, institutional email access, external university services, DTOs, services, configuration, domain entities, repositories, and database migrations.

The `ohmyuniversity-api-fetcher` repository contains jobs and APIs for external data retrieval. It manages timetable data, university statistics, professional-register information, related DTOs, services, domain entities, repositories, and scheduled or batch-oriented data collection logic.

The `ohmyuniversity-infra` repository contains operational infrastructure for the backend, including Docker configuration, monitoring configuration, deployment scripts, rollback scripts, and Terraform files.

At the current stage, the backend components relevant to the selected deliverable are therefore distributed across gateway routing, core application logic, fetcher data retrieval, and infrastructure configuration.

### Backend Package Dependencies

The backend follows a service-oriented dependency direction.

At system level, Web and Mobile clients communicate with the backend through the API Gateway:

```text
Angular Web Frontend
Flutter Mobile Frontend
        |
api-gateway
        |
api-core / api-fetcher
        |
database / external systems
```

The `api-gateway` does not contain business logic. It exposes public routes and forwards requests to the proper internal service.

The `api-core` service contains the main application logic and may depend on repositories, DTOs, configuration classes, Cineca/Esse3 adapters, email providers, and event-publishing components.

The `api-fetcher` service contains scheduled jobs and services for external data retrieval, such as timetables, statistics, and professional-register data.

For academic data, the dependency direction is:

```text
api-core service
        |
cineca adapter
        |
external Cineca/Esse3 system
```

For fetched external data, the dependency direction is:

```text
api-fetcher service
        |
fetcher jobs
        |
external data sources
```

This organization keeps frontend clients isolated from backend internals and external systems while preserving a clear separation between routing, application logic, data fetching, persistence, and infrastructure.


## Overall Package Dependencies

The overall package dependencies define how the current Web frontend, mobile frontend, and backend packages are allowed to communicate with each other.

The goal is to keep the system modular, reduce coupling, and avoid direct dependencies between frontend clients, backend internals, persistence logic, and external university systems.

At system level, the main dependency direction is:

```text
Angular Web Frontend
Flutter Mobile Frontend
        ↓
Java Spring Boot Backend REST APIs
        ↓
Backend Services
        ↓
Domain / Repositories
        ↓
Database
```

Both frontend clients communicate with the backend only through public REST APIs.

The Angular Web frontend and the Flutter mobile frontend must not access the database directly and must not communicate directly with external university systems such as Cineca/Esse3.

External university integration is isolated inside the backend:

```text
Backend Services
        ↓
Cineca Integration Package
        ↓
External Cineca/Esse3 System
```

This keeps external API details hidden from frontend clients and from the backend domain packages.

Inside the Angular Web frontend, the general dependency direction is:

```text
features
↓
shared
↓
core
```

Feature packages can use reusable elements from `shared` and global infrastructure from `core`.

The `shared` package must remain independent from specific features.

The `core` package provides application-wide infrastructure and must not depend on feature-specific packages.

Inside the Flutter mobile frontend, the general dependency direction is:

```text
features
↓
shared
↓
core
```

Feature packages can use shared widgets and common core elements.

The `shared` package contains reusable UI elements.

The `core` package contains common architectural elements and must remain independent from feature-specific modules.

Inside partially layered frontend features, the dependency direction is:

```text
presentation
↓
application / domain

data / infrastructure
↓
domain
```

The presentation layer contains UI-related code.

The application layer coordinates feature operations where it is present.

The domain layer contains the central abstractions of the feature.

The data or infrastructure layer contains technical access to APIs, storage, or external data sources.

Inside the backend, the general dependency direction is:

```text
controller
↓
service
↓
domain / repository
↓
database
```

Controllers expose REST APIs and delegate operations to services.

Services coordinate backend logic and may use repositories, DTOs, configuration elements, and integration packages.

Repositories manage access to persistent backend data.

DTOs are used to exchange data between backend and frontend clients without exposing internal domain structures.

This dependency organization keeps the current implementation understandable, maintainable, and ready for incremental extension.