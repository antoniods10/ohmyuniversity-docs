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
├── components
├── constants
├── directives
├── types
├── ui
└── utils
```

The `shared` package is responsible for common UI elements, constants, directives, types, and utility functions.

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
    ├── layout
    ├── pages
    └── state
```

This package is partially organized into application, domain, infrastructure, and presentation layers.

The `presentation` layer contains the dashboard pages, layout, UI components, and state structure.
The `application` layer is used to coordinate dashboard-related operations.
The `domain` layer contains the main dashboard abstractions.
The `infrastructure` layer is prepared for communication with backend APIs.


### Orientation Feature Package

The `orientation` feature contains the frontend structure for orientation-related content.

Current structure:

```text
orientation
├── domain
│   └── models
└── presentation
    ├── components
    └── pages
```

This package is partially implemented.

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
├── constants
├── error
└── usecases
```

The `constants` package contains application-wide constants.

The `error` package contains common error and failure abstractions used by the application.

The `usecases` package contains generic use case structures shared by different features.

The `core` package provides common architectural elements and should not contain UI code or feature-specific business logic.


### Shared Package

The `shared` package contains reusable UI elements shared by multiple mobile features.

Current structure:

```text
shared
└── widgets
```

The `widgets` package contains common interface components used in different areas of the mobile application.

This package avoids duplication of UI elements and keeps common presentation components separated from feature-specific code.


### Features Package

The `features` package contains the main functional areas of the Flutter mobile application.

Current structure:

```text
features
├── academic_career
├── auth
├── aziende
├── chat
├── didattica
├── explore
├── home
├── notifiche
├── onboarding
├── orientamento
├── preferiti
├── services
└── splash
```

Each feature package groups the code related to a specific area of the mobile application.

At the current stage, some features are partially implemented with a complete layered structure, while others are mainly organized around presentation-level pages and widgets.


### Academic Career Feature Package

The `academic_career` feature contains the mobile structure for the student academic career area.

Current structure:

```text
academic_career
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
    ├── providers
    └── widgets
```

This package is partially implemented according to a Clean Architecture-style organization.

The `domain` layer contains the central academic-career abstractions and repository contracts.

The `data` layer contains the technical structures used to retrieve and map academic-career data.

The `presentation` layer contains the pages, providers, and widgets used to display academic-career information in the mobile interface.


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

The `orientamento` feature contains the mobile structure for orientation-related content.

Current structure:

```text
orientamento
└── presentation
    ├── pages
    └── widgets
```

This package is currently implemented mainly at presentation level.

It groups the pages and widgets used to present orientation information in the mobile application.


### Presentation-Level Feature Packages

Several mobile features are currently organized mainly at presentation level.

Current packages:

```text
features
├── aziende
├── chat
├── didattica
├── explore
├── notifiche
├── preferiti
└── services
```

These packages contain the initial structure for additional mobile areas of the application.

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

The Java Spring Boot backend is organized under the `org.ohmyopensource.ohmyuniversity.core` package.

The current backend structure separates REST API exposure, application logic, domain persistence, data transfer objects, configuration, and external Cineca/Esse3 integration.

Current high-level structure:

```text
org.ohmyopensource.ohmyuniversity.core
├── cineca
├── config
├── controller
├── domain
│   ├── entity
│   └── repository
├── dto
└── service
```

The backend also contains resource-level packages used for application configuration and database migration management.

Current resource structure:

```text
src/main/resources
├── db
│   └── migration
└── application configuration files
```

### Controller Package
The `controller` package contains the REST entry points exposed by the backend.

Current structure:

```text
controller
```

This package is responsible for receiving HTTP requests from frontend clients and returning HTTP responses.

Controllers do not contain persistence logic or direct external system access. Their role is to expose backend APIs and delegate application operations to the service layer.

At the current stage, this package is already present and partially implements authentication and academic-career API access.


### Service Package
The `service` package contains the application logic of the backend.

Current structure:

```text
service
```

This package coordinates the main backend operations, such as authentication, token management, and academic-career data handling.

Services can use domain repositories, DTOs, configuration elements, and integration packages. They act as the coordination layer between REST controllers, persistence, and external systems.

At the current stage, this package is already present and partially implements the authentication flow and the first academic-career operations.


### Domain Package
The `domain` package contains the persistent domain area of the backend.

Current structure:

```text
domain
├── entity
└── repository
```

The `entity` package contains the persistent business objects managed directly by the backend.

The `repository` package contains the persistence access layer used to read and write backend-managed data.

This package is responsible for data that belongs to OhMyUniversity and is stored by the system, such as user-related and university-connection information.

At the current stage, this package is already present and focused on the backend-managed identity and connection domain.


### DTO Package

The `dto` package contains the objects used to exchange data through REST APIs.

Current structure:

```text
dto
```

This package is responsible for request and response structures exchanged between the backend and frontend clients.

DTOs are used to avoid exposing internal domain structures directly and to provide frontend-oriented representations of backend and external university data.

At the current stage, this package is already present and contains DTOs for authentication and academic-career related operations.

### Configuration Package

The `config` package contains backend configuration and security-related elements.

Current structure:

```text
config
```

This package is responsible for application configuration, security configuration, JWT authentication support, and university-related configuration data.

Configuration elements are shared across multiple backend packages and support the correct execution of controllers, services, and integration logic.

At the current stage, this package is already present and supports authentication, security, environment configuration, and university registry configuration.

### Cineca Integration Package

The `cineca` package contains the backend integration layer for Cineca/Esse3.

Current structure:

```text
cineca
```

This package isolates communication with the external university system.

Its role is to keep external API access separated from controllers and domain logic. Backend services use this package when they need to retrieve or manage university-related data coming from Cineca/Esse3.

At the current stage, this package is already present and partially implements login/session handling and academic-career data retrieval from the external university system.

### Database Migration Resources

Database migrations are organized under the `db/migration` resource directory.

Current structure:

```text
src/main/resources
└── db
    └── migration
```

This directory contains the versioned database migration files used to create and evolve the backend database schema.

Its role is to keep database structure changes reproducible and aligned with the backend domain persistence layer.

At the current stage, the migration structure is already present and supports the persistence of backend-managed user and university-connection data.


### Backend Package Dependencies
The backend follows a layered dependency direction.

The main dependency direction is:

```text
controller
↓
service
↓
domain / repository
↓
database
```

Controllers depend on services to execute application operations.

Services coordinate the backend logic and may depend on domain repositories, DTOs, configuration elements, and integration packages.

Domain entities and repositories represent the persistent data managed by the backend.

For external university data, the dependency direction is:

```text
service
↓
cineca
↓
external Cineca/Esse3 system
```

The `cineca` package isolates external system communication and prevents controllers and domain packages from depending directly on Cineca/Esse3 details.

The `dto` package is used across the controller and service layers to exchange API data without exposing internal domain structures.

The `config` package provides shared configuration and security support used by the backend packages.

This organization keeps the backend modular, separates REST APIs from application logic, and isolates persistence and external integrations.



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