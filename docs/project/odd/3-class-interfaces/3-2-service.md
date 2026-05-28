---
title: ODD - 3.2 Package Service | OhMyUniversity!
description: Business logic service classes for OhMyUniversity! api-core microservice - AuthService, CarrieraService, and OmuJwtService with complete Design by Contract specifications.
head:
  - - meta
    - property: og:title
      content: ODD - 3.2 Package Service | OhMyUniversity!
  - - meta
    - property: og:description
      content: Business logic service classes for OhMyUniversity! api-core microservice - AuthService, CarrieraService, and OmuJwtService with complete Design by Contract specifications.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/3-class-interfaces/3-2-service
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, service layer, authservice, carrieraservice, omujwtservice, business logic
  - - meta
    - name: twitter:title
      content: ODD - 3.2 Package Service | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Business logic service classes for OhMyUniversity! api-core microservice - AuthService, CarrieraService, and OmuJwtService with complete Design by Contract specifications.
---

# 3.2 Package: `service`

This package contains the backend's business logic. It coordinates controllers, domain data, and integration with external systems (ESSE3 via Cineca adapter) and manages authentication flows and token lifecycle.

**Class: AuthService**

- **Description:** Service responsible for managing the complete authentication flow of OhMyUniversity! It handles user login against Cineca ESSE3, initializes OhMyUniversity sessions, manages token refresh, and handles logout operations.
- **Dependencies:** `CinecaClient`, `CinecaSessionStore`, `OmuJwtService`, `OmuUserRepository`, `UniversityConnectionRepository`, `UniversityRegistry`.
- **Operations:**
  - `login(request: LoginRequest): LoginResponse`
    - **Description:** Authenticates a user against Cineca ESSE3 using provided credentials and university context. On successful authentication, an OhMyUniversity session is initialized with JWT tokens.
    - **Pre-conditions:** `request` must not be null; `request.universityId` must be registered in the system; credentials must be valid for the specified university.
    - **Post-conditions:** A `LoginResponse` is returned containing both access and refresh tokens; user data is persisted in the database; temporary ESSE3 session data is stored in Redis.
    - **Exceptions:** `@throws CinecaClient.CinecaAuthException` if credentials are invalid; `@throws CinecaClient.CinecaUnavailableException` if ESSE3 is unreachable; `@throws IllegalArgumentException` if the universityId is not registered.
  - `logout(refreshToken: String, universityId: String): void`
    - **Description:** Logs out the user by invalidating the refresh token and clearing all Cineca session data stored in Redis.
    - **Pre-conditions:** `refreshToken` and `universityId` must not be null or empty.
    - **Post-conditions:** Refresh token is invalidated in the database; ESSE3 session data is removed from Redis for the specified university context.
    - **Exceptions:** No exceptions thrown; invalid tokens are silently ignored (idempotent operation).
  - `refresh(refreshToken: String, universityId: String): String`
    - **Description:** Issues a new access token using a valid refresh token, allowing the user to continue their session without re-authenticating.
    - **Pre-conditions:** `refreshToken` must be valid and not expired; `universityId` must be registered in the system.
    - **Post-conditions:** A new access token is generated and returned; refresh token remains valid for subsequent refresh operations.
    - **Exceptions:** `@throws InvalidTokenException` if the refresh token is invalid or expired.

---

**Class: CarrieraService**

- **Description:** Service orchestrating career-related operations against ESSE3. It coordinates the retrieval of student academic data (transcript, averages, study plans, exam sessions), aggregates results, and manages application of business rules for academic profile data.
- **Dependencies:** `CinecaCarrieraClient`, `CinecaSessionStore`, `UniversityRegistry`, `UniversityConnectionRepository`.
- **Operations:**
  - `getLibretto(principal: OmuPrincipal): LibrettoResponse`
    - **Description:** Retrieves the complete student transcript from Cineca, including all registered exams with grades and academic history.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context (stuId and matId).
    - **Post-conditions:** Returns a `LibrettoResponse` object populated with all exam records from ESSE3.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 token has expired; `@throws UniversityConnectionException` if the university's Cineca connection is not configured.
  - `getMedia(principal: OmuPrincipal): MediaResponse`
    - **Description:** Retrieves academic statistics and average calculations for the student, including arithmetic and weighted averages based on exam grades and credits.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns a `MediaResponse` object with calculated averages and academic statistics.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 token has expired.
  - `getPiano(principal: OmuPrincipal): PianoStudioResponse`
    - **Description:** Retrieves the student's study plan (Piano di Studi) from Cineca, including planned courses and associated credits.
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns a `PianoStudioResponse` object with the study plan structure.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 token has expired.
  - `getAppelli(principal: OmuPrincipal, adId: Long): AppelloResponse`
    - **Description:** Retrieves all available exam sessions for the specified activity from ESSE3.
    - **Pre-conditions:** `principal` must not be null; `adId` (activity identifier) must be a valid positive number.
    - **Post-conditions:** Returns an `AppelloResponse` object listing all exam sessions with dates and availability information.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 token has expired; `@throws InvalidActivityException` if adId is invalid.
  - `getBadge(principal: OmuPrincipal): BadgeResponse`
    - **Description:** Retrieves the student's digital university badge (recognition of academic status).
    - **Pre-conditions:** `principal` must not be null and must contain a valid active career context.
    - **Post-conditions:** Returns a `BadgeResponse` object with badge data and verification information.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 token has expired.
  - `getPrenotazioni(principal: OmuPrincipal, cinecaPassword: String): PrenotazioneResponse`
    - **Description:** Retrieves the student's exam booking history (prenotazioni), including past and upcoming exam bookings.
    - **Pre-conditions:** `principal` must not be null; `cinecaPassword` must be provided for session authentication with ESSE3.
    - **Post-conditions:** Returns a `PrenotazioneResponse` object with the complete booking history.
    - **Exceptions:** `@throws CinecaSessionExpiredException` if the temporary ESSE3 session is invalid; `@throws InvalidCredentialsException` if cinecaPassword is incorrect.

---

**Class: OmuJwtService**

- **Description:** Service responsible for issuing and validating OhMyUniversity! JWT tokens. Access tokens contain user identity and active university profile claims. Tokens are signed using an HMAC secret key configured through application properties. Access token lifetime defaults to 15 minutes.
- **Dependencies:** JWT library (JJWT), configuration properties for secret and expiration time.
- **Attributes:**
  - `secret: String` (configured via `${omu.jwt.secret}`) - HMAC secret for token signing
  - `expirationMs: long` (configured via `${omu.jwt.expiration}`) - access token expiration time in milliseconds
- **Operations:**
  - `issue(omuUserId: String, codiceFiscale: String, universityId: String, stuId: Long, matId: Long, matricola: String): String`
    - **Description:** Issues a signed JWT access token for the provided user context, including user identity and active university profile information.
    - **Pre-conditions:** All parameters must not be null or empty; `stuId` and `matId` must be positive numbers.
    - **Post-conditions:** Returns a signed JWT token containing all user claims; token is valid immediately and expires after the configured lifetime.
    - **Exceptions:** No exceptions; token generation should always succeed if parameters are valid.
  - `validate(token: String): Claims`
    - **Description:** Validates the provided JWT access token by verifying the signature and checking expiration.
    - **Pre-conditions:** `token` must not be null or empty.
    - **Post-conditions:** Returns the validated `Claims` object containing all token information.
    - **Exceptions:** `@throws InvalidTokenException` if the token is malformed, expired, or signed with an invalid key.
  - `generateRefreshToken(): String`
    - **Description:** Generates a cryptographically random refresh token identifier for use in token refresh flows. Uses secure random generation with increased entropy.
    - **Pre-conditions:** None.
    - **Post-conditions:** Returns a unique refresh token string suitable for persistent storage in Redis.
    - **Exceptions:** No exceptions; generation should always succeed.
