---
title: ODD - 3.4 Package Cineca | OhMyUniversity!
description: External integration gateway for Cineca ESSE3 - adapter pattern implementation with session management and complete API integration.
head:
  - - meta
    - property: og:title
      content: ODD - 3.4 Package Cineca | OhMyUniversity!
  - - meta
    - property: og:description
      content: External integration gateway for Cineca ESSE3 - adapter pattern implementation with session management and complete API integration.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/3-class-interfaces/3-4-cineca
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, cineca, adapter pattern, esse3, session management, external integration
  - - meta
    - name: twitter:title
      content: ODD - 3.4 Package Cineca | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: External integration gateway for Cineca ESSE3 - adapter pattern implementation with session management and complete API integration.
---

# 3.4 Package: `cineca`

This layer acts as an _Adapter_ towards the outside. It isolates the rest of the system from the technical details of the legacy ESSE3 APIs using the Adapter pattern. All communication with Cineca ESSE3 is handled here, with response transformation to internal DTOs.

### Exception Classes

**Class: CinecaClient.CinecaAuthException**

- **Description:** Checked exception thrown when Cineca authentication fails (invalid credentials, unauthorized user).
- **Extends:** `Exception`
- **Usage:** Thrown by `CinecaClient.login()` to indicate credential validation failure at ESSE3.

---

**Class: CinecaClient.CinecaUnavailableException**

- **Description:** Checked exception thrown when Cineca ESSE3 service is unreachable (network error, service down, timeout).
- **Extends:** `Exception`
- **Usage:** Thrown when HTTP calls to ESSE3 endpoints fail due to connectivity or service unavailability.

---

### Main Adapter Class

**Class: CinecaClient**

- **Description:** Adapter executing HTTP calls (via Spring WebClient) to ESSE3 APIs, handling Basic authentication, session management, and response deserialization. This is the primary gateway to the legacy ESSE3 system.
- **Dependencies:** Spring `WebClient`, Cineca configuration endpoints, response DTOs.
- **Responsibility:** Isolates the rest of the system from Cineca's API specifics; transforms Cineca JSON responses to internal data structures.
- **Key Operations:**
  - All methods throw `CinecaAuthException` if authentication fails or `CinecaUnavailableException` if the service is unreachable.

---

### Specialized Adapter for Career Data

**Class: CinecaCarrieraClient**

- **Description:** Specialized adapter extending `CinecaClient`, dedicated to retrieving student career-related data from ESSE3 (transcripts, averages, study plans, exam sessions, bookings). It handles the specific endpoints and transformations for career APIs.
- **Dependencies:** `CinecaClient` (inheritance), Spring `WebClient`.
- **Nested Classes:** `CinecaRigaLibretto`, `CinecaMedia`, `CinecaPianoDettaglio`, `CinecaPrenotazione`, `CinecaEsito`, `CinecaEsitoPrenotazione`, `CinecaChiaveAd`, `CinecaTestataPiano`, `CinecaValueWrapper`.

**Nested Class: CinecaCarrieraClient.CinecaRigaLibretto**

- **Description:** Internal data structure mapping a single exam row from ESSE3 carriera API.
- **Attributes:**
  - `adId: Long` - Course/activity ID from Cineca
  - `nomeInsegnamento: String` - Course name from ESSE3
  - `docente: String` - Instructor name
  - `annoAccademico: String` - Academic year
  - `dataEsame: LocalDate` - Exam date
  - `voto: Double` - Grade (18-30) or null if pending
  - `cfu: Double` - Academic credits
  - `flagSuperato: Boolean` - Passed status

**Nested Class: CinecaCarrieraClient.CinecaMedia**

- **Description:** Internal structure for calculated averages from Cineca media API.
- **Attributes:**
  - `mediaAritmetica: Double` - Arithmetic mean of grades
  - `mediaPonderata: Double` - Weighted average (CFU-based)
  - `baseLaurea: Double` - Graduation baseline calculation
  - `percentualeCfu: Double` - CFU completion percentage
  - `cfuTotaliCompletati: Double` - Credits earned
  - `cfuTotaliPrevisti: Double` - Credits required

**Nested Class: CinecaCarrieraClient.CinecaTestataPiano**

- **Description:** Header/metadata for a study plan (piano di studi) from ESSE3.
- **Attributes:**
  - `stuId: Long` - Career identifier
  - `matId: Long` - Career segment identifier
  - `statoPiano: String` - Plan approval status (APPROVED, PENDING, REJECTED)
  - `dataApprovazione: LocalDate` - Approval date (null if pending)

**Nested Class: CinecaCarrieraClient.CinecaPianoDettaglio**

- **Description:** Single course row in the study plan from ESSE3 piani API.
- **Attributes:**
  - `adId: Long` - Teaching activity ID
  - `nomeInsegnamento: String` - Course name
  - `cfuPrevisti: Double` - Expected credits
  - `annoAccademico: String` - Academic year scheduled
  - `tipoCfu: String` - Type (OBB=obligatory, OPZ=optional)

**Nested Class: CinecaCarrieraClient.CinecaChiaveAd**

- **Description:** Unique identifier structure for a Cineca teaching activity (adId).
- **Attributes:**
  - `adId: Long` - Activity identifier (primary key in ESSE3)

**Nested Class: CinecaCarrieraClient.CinecaValueWrapper**

- **Description:** Generic wrapper for Cineca API responses that return simple scalar values (e.g., single numeric responses).
- **Attributes:**
  - `value: Object` - Generic wrapped value from Cineca response

**Nested Class: CinecaCarrieraClient.CinecaPrenotazione**

- **Description:** Exam booking record from ESSE3 calesa API.
- **Attributes:**
  - `prenotazioneId: Long` - Booking identifier
  - `appelliId: Long` - Exam session ID
  - `adId: Long` - Course activity ID
  - `dataPrenotazione: LocalDateTime` - Booking creation timestamp
  - `dataRitiro: LocalDate` - Withdrawal deadline
  - `stato: String` - Status (CONFERMATA, ANNULLATA, GIUSTIFICATA)
  - `numeroPos: Integer` - Queue position (null if not queued)

**Nested Class: CinecaCarrieraClient.CinecaEsito**

- **Description:** Generic result wrapper for Cineca API operation outcomes.
- **Attributes:**
  - `esito: String` - Result code (OK, ERROR, CONFLICT, etc.)
  - `messaggio: String` - Descriptive message
  - `codiceErrore: String` - Error code for error cases

**Nested Class: CinecaCarrieraClient.CinecaEsitoPrenotazione**

- **Description:** Result structure specific to exam booking operations in ESSE3.
- **Attributes:**
  - `esito: String` - Booking result (SUCCESS, CONFLICT, QUOTA_FULL, etc.)
  - `messaggio: String` - User-facing message
  - `prenotazioneId: Long` - ID of created booking (null if failed)
  - `numeroPos: Integer` - Queue position if applicable

---

### Session Management

**Class: CinecaSessionStore**

- **Description:** Redis-backed session storage service managing temporary Cineca ESSE3 tokens and session data for authenticated users. Handles session lifecycle (creation, validation, expiration, cleanup).
- **Storage:** Redis with configurable TTL (typically 1-2 hours for ESSE3 session tokens).
- **Responsibilities:**
  - Store ESSE3 session tokens (JSESSIONID + authentication cookies)
  - Store temporary OAuth/token data from Cineca login
  - Provide fast retrieval of session data by user/universityId key
  - Handle automatic expiration of stale sessions
  - Support multi-university contexts (per-universityId isolation)
- **Key Operations:**
  - `storeSession(userId: String, universityId: String, sessionData: CinecaSessionData, ttlSeconds: int): void`
  - `retrieveSession(userId: String, universityId: String): CinecaSessionData`
  - `invalidateSession(userId: String, universityId: String): void`
  - `isSessionValid(userId: String, universityId: String): Boolean`

---

### Cineca Login Response Structure

**Class: CinecaLoginResponse**

- **Description:** Response structure encapsulating the complete result of a Cineca ESSE3 login operation, including user data, career profiles (tratti), and session information.
- **Nested Classes:** `CinecaUser`, `TrattoCarriera`, `DettaglioTratto`.
- **Attributes:**
  - `user: CinecaUser` - Authenticated user information
  - `tratti: List<TrattoCarriera>` - List of career segments (academic profiles)
  - `jsessionid: String` - Session cookie from ESSE3 for subsequent API calls

**Nested Class: CinecaLoginResponse.CinecaUser**

- **Description:** User identity information returned by ESSE3 during login.
- **Attributes:**
  - `userid: String` - ESSE3 username
  - `codiceFiscale: String` - Italian tax ID (unique identifier)
  - `nome: String` - First name
  - `cognome: String` - Last name
  - `email: String` - University email

**Nested Class: CinecaLoginResponse.TrattoCarriera**

- **Description:** Represents a single career segment (tratto) for a student, usually corresponding to an academic program enrollment.
- **Attributes:**
  - `stuId: Long` - Career identifier in ESSE3
  - `matId: Long` - Career segment/matric identifier
  - `dettaglio: DettaglioTratto` - Additional metadata

**Nested Class: CinecaLoginResponse.DettaglioTratto**

- **Description:** Detailed metadata for a career segment.
- **Attributes:**
  - `matricola: String` - Student registration number
  - `corsoLaurea: String` - Degree program name
  - `annoIscrizione: Integer` - Year of enrollment
  - `annoCorrente: Integer` - Current academic year
  - `statoCodStudente: String` - Academic status code
