---
title: ODD - 3.1 Package Controller | OhMyUniversity!
description: REST Controllers specification for OhMyUniversity! api-core microservice - AuthController and CarrieraController with complete Design by Contract specifications.
head:
  - - meta
    - property: og:title
      content: ODD - 3.1 Package Controller | OhMyUniversity!
  - - meta
    - property: og:description
      content: REST Controllers specification for OhMyUniversity! api-core microservice - AuthController and CarrieraController with complete Design by Contract specifications.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/3-class-interfaces/3-1-controller
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, controller, rest api, authcontroller, carrieracontroller, design by contract
  - - meta
    - name: twitter:title
      content: ODD - 3.1 Package Controller | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: REST Controllers specification for OhMyUniversity! api-core microservice - AuthController and CarrieraController with complete Design by Contract specifications.
---

# 3.1 Package: `controller`

This package contains the _Solution Objects_ responsible for exposing REST APIs to frontend clients (Web and Mobile). They intercept HTTP requests and delegate application logic to the Services. All controllers in this layer are thin orchestration layers: they extract identity from JWT tokens, delegate business logic to services, and map service exceptions to appropriate HTTP responses.

**Class: AuthController**

- **Mapping:** `@RestController @RequestMapping("/api/v1/auth")`
- **Description:** Authentication controller for OhMyUniversity! exposing public endpoints for login, token refresh, and logout. All endpoints are publicly accessible — authentication is delegated to Cineca ESSE3 or refresh-token validation, not Spring Security.
- **Dependencies:** `AuthService` (injected via constructor).
- **Operations:**
  - `login(request: @Valid LoginRequest): ResponseEntity<LoginResponse>`
    - **Mapping:** `@PostMapping("/login")`
    - **Description:** Authenticates a student against Cineca ESSE3. Flow: (1) Validate request payload, (2) Delegate authentication to AuthService, (3) Retrieve Cineca session + JWT, (4) Build OhMyUniversity access + refresh tokens.
    - **Pre-conditions:** `request` must not be null; `request.username`, `request.password`, and `request.universityId` must not be empty.
    - **Post-conditions:** Returns `LoginResponse` with access token, refresh token, and available career profiles.
    - **HTTP Responses:**
      - `200 OK` - Successful login with LoginResponse body
      - `401 Unauthorized` - Cineca credentials are invalid
      - `404 Not Found` - University ID is not registered
      - `503 Service Unavailable` - Cineca ESSE3 service is unreachable
  - `refresh(refreshToken: String, universityId: String): ResponseEntity<String>`
    - **Mapping:** `@PostMapping("/refresh")` with `@RequestParam`
    - **Description:** Refreshes an access token using a valid refresh token. This endpoint does NOT contact Cineca directly; it only validates and rotates OhMyUniversity tokens.
    - **Pre-conditions:** `refreshToken` must be valid and not expired; `universityId` must be registered in the system.
    - **Post-conditions:** Returns a new access token string.
    - **HTTP Responses:**
      - `200 OK` - New access token provided in response body
      - `401 Unauthorized` - Refresh token is invalid or expired
  - `logout(refreshToken: String, universityId: String): ResponseEntity<Void>`
    - **Mapping:** `@PostMapping("/logout")` with `@RequestParam`
    - **Description:** Logs out the user by invalidating all active sessions. Actions: (1) Deletes refresh token from Redis, (2) Clears Cineca session tokens.
    - **Pre-conditions:** `refreshToken` and `universityId` must not be null or empty.
    - **Post-conditions:** Refresh token invalidated; Cineca session data cleared.
    - **HTTP Responses:**
      - `204 No Content` - Logout successful (no response body)

---

**Class: CarrieraController**

- **Mapping:** `@RestController @RequestMapping("/api/v1/carriera")`
- **Description:** Controller responsible for exposing all student career-related APIs. This layer is a thin orchestration layer: extracts identity from JWT (OmuPrincipal), delegates business logic to CarrieraService, and maps Cineca failures into HTTP responses. **Important architectural note:** All data is fetched in real-time from Cineca ESSE3; no academic data is persisted in this service.
- **Dependencies:** `CarrieraService` (injected via constructor), `OmuPrincipal` (from security context).
- **Operations:**
  - `getLibretto(principal: @AuthenticationPrincipal OmuPrincipal): ResponseEntity<LibrettoResponse>`
    - **Mapping:** `@GetMapping("/libretto")`
    - **Description:** Retrieves the student's complete transcript (libretto) from Cineca ESSE3. Returns all exam records associated with the authenticated student (derived from JWT principal).
    - **Pre-conditions:** `principal` must not be null and must contain valid `stuId`, `matId`, and `universityId`.
    - **Post-conditions:** Returns `LibrettoResponse` containing all exam rows (RigaLibretto).
    - **HTTP Responses:**
      - `200 OK` - Transcript retrieved successfully
      - `401 Unauthorized` - Cineca session expired or JWT invalid
      - `503 Service Unavailable` - Cineca ESSE3 is unreachable
  - `getMedia(principal: @AuthenticationPrincipal OmuPrincipal): ResponseEntity<MediaResponse>`
    - **Mapping:** `@GetMapping("/medie")`
    - **Description:** Retrieves computed academic statistics and averages for the authenticated student. Includes arithmetic mean, weighted mean (ECTS-based), projected graduation score, and CFU progress metrics.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns `MediaResponse` with calculated statistics.
    - **HTTP Responses:**
      - `200 OK` - Averages computed and returned
      - `401 Unauthorized` - Cineca session expired
      - `503 Service Unavailable` - Cineca service unreachable
  - `getPiano(principal: @AuthenticationPrincipal OmuPrincipal): ResponseEntity<PianoStudioResponse>`
    - **Mapping:** `@GetMapping("/piano")`
    - **Description:** Retrieves the student's study plan (piano di studi) from Cineca. Returns all planned courses and their status as provided by Cineca piani-service.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns `PianoStudioResponse` with course rows and plan status.
    - **HTTP Responses:**
      - `200 OK` - Study plan retrieved
      - `401 Unauthorized` - Cineca session expired or JWT invalid
      - `503 Service Unavailable` - Cineca service unreachable
  - `getAppelli(principal: @AuthenticationPrincipal OmuPrincipal, cdsId: Long, adId: Long): ResponseEntity<AppelloResponse>`
    - **Mapping:** `@GetMapping("/appelli")` with `@RequestParam cdsId, adId`
    - **Description:** Retrieves available exam sessions (appelli) for a specific course. Requires `cdsId` (degree course ID) and `adId` (teaching activity ID) to query Cineca calesa-service.
    - **Pre-conditions:** `principal` must not be null; `cdsId` and `adId` must be positive values (valid Cineca identifiers).
    - **Post-conditions:** Returns `AppelloResponse` listing all exam sessions with dates and availability.
    - **HTTP Responses:**
      - `200 OK` - Exam sessions retrieved
      - `401 Unauthorized` - Cineca session expired
      - `503 Service Unavailable` - Cineca service unreachable
  - `getPrenotazioni(principal: @AuthenticationPrincipal OmuPrincipal, request: @RequestBody PrenotazioneRequest): ResponseEntity<PrenotazioneResponse>`
    - **Mapping:** `@PostMapping("/prenotazioni")`
    - **Description:** Retrieves full exam booking history for the student. **Security Note:** This endpoint requires the Cineca password in the request body because the underlying calesa-service does not support JWT-only authentication and requires session-level authentication (JSESSIONID + Basic Auth). The password is transmitted in the request body via HTTPS, never persisted, cached, or logged.
    - **Pre-conditions:** `principal` must not be null; `request.cinecaPassword` must not be empty.
    - **Post-conditions:** Returns `PrenotazioneResponse` with booking history and result of any booking operation.
    - **HTTP Responses:**
      - `200 OK` - Booking history retrieved
      - `400 Bad Request` - Password is missing or empty
      - `401 Unauthorized` - Cineca authentication fails or JWT invalid
      - `503 Service Unavailable` - Cineca service unreachable
  - `getBadge(principal: @AuthenticationPrincipal OmuPrincipal): ResponseEntity<BadgeResponse>`
    - **Mapping:** `@GetMapping("/badge")`
    - **Description:** Retrieves the student's digital university badge if available. The badge contains university-issued identification data such as RFID, enrollment status, and course metadata.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns `BadgeResponse` with badge data and verification information.
    - **HTTP Responses:**
      - `200 OK` - Badge retrieved successfully
      - `404 Not Found` - No badge associated with student
      - `401 Unauthorized` - Cineca session expired or JWT invalid
      - `503 Service Unavailable` - Cineca service unreachable

---

## Additional Backend Controllers

The current backend implementation also includes additional controllers that must be considered part of the backend interface design.

### API Core Controllers

- **CalendarController** - exposes `/api/v1/calendar` endpoints for personal calendar events and university event import.
- **EmailController** - exposes `/api/v1/email` endpoints for institutional email authentication, inbox consultation, message details, and message sending.
- **ExternalServicesController** - exposes `/api/v1/university/external-services` for university service links and external portal metadata.

### API Fetcher Controllers

- **TimetableController** - exposes `/api/v1/fetcher/timetables` for timetable-related data.
- **StatisticheController** - exposes `/api/statistiche` endpoints for university statistics.
- **OrdineProfessionaleController** - exposes `/api/ordini-professionali` endpoints for professional-register information.
- **JobTriggerController** - exposes `/api/jobs/{jobName}/run` for manual execution of configured fetcher jobs.

### Gateway Route Boundaries

The API Gateway exposes public routes to Web and Mobile clients and forwards them to the internal backend services. Public routes use the `/v1/...` prefix, while internal Spring Boot controllers use `/api/v1/...`.

