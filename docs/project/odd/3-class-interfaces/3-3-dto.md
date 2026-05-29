---
title: ODD - 3.3 Package DTO | OhMyUniversity!
description: Data Transfer Objects for OhMyUniversity! api-core microservice - complete specification of all DTOs used for client-server communication.
head:
  - - meta
    - property: og:title
      content: ODD - 3.3 Package DTO | OhMyUniversity!
  - - meta
    - property: og:description
      content: Data Transfer Objects for OhMyUniversity! api-core microservice - complete specification of all DTOs used for client-server communication.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/odd/3-class-interfaces/3-3-dto
  - - meta
    - name: keywords
      content: ohmyuniversity, odd, dto, data transfer objects, loginrequest, loginresponse, libretto, media, piano studio
  - - meta
    - name: twitter:title
      content: ODD - 3.3 Package DTO | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Data Transfer Objects for OhMyUniversity! api-core microservice - complete specification of all DTOs used for client-server communication.
---

# 3.3 Package: `dto`

This package contains the Data Transfer Objects (DTO). They are solution objects used exclusively to transport data between the backend and clients, hiding the complexity and depth of the domain object tree or the original Cineca JSON payloads. All DTOs in this package are Plain Old Java Objects (POJOs) with no external dependencies.

**Class: LoginRequest**

- **Description:** Data transfer object encapsulating the credentials and university context needed to perform authentication against ESSE3.
- **Dependencies:** None (POJO).
- **Attributes:**
  - `username: String` - Student ESSE3 username or identifier
  - `password: String` - Student ESSE3 password (plain text, transmitted over HTTPS)
  - `universityId: String` - University identifier to contextualize the login (must be registered in the system)
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: LoginResponse**

- **Description:** Data transfer object containing authentication tokens and available career profiles returned after successful login.
- **Dependencies:** `LoginResponse.ProfiloCarriera` (nested class).
- **Attributes:**
  - `accessToken: String` - JWT access token signed with the configured secret (expires after 15 minutes)
  - `refreshToken: String` - Refresh token identifier for obtaining new access tokens (persisted in Redis)
  - `profiliCarriera: List<ProfiloCarriera>` - List of available career profiles for the authenticated student
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: LoginResponse.ProfiloCarriera**

- **Description:** Encapsulates metadata for a single student career profile (academic program enrollment).
- **Attributes:**
  - `stuId: Long` - Cineca career identifier (ESSE3 stuId)
  - `matId: Long` - Cineca career segment identifier (ESSE3 matId)
  - `matricola: String` - Student registration number at the specific university
  - `corsoLaurea: String` - Name of the degree program
  - `annoIscrizione: Integer` - Year of enrollment in this program
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: LibrettoResponse**

- **Description:** Flat data transfer object representing the student's complete academic transcript (libretto) from ESSE3, listing all exam records with grades.
- **Dependencies:** `LibrettoResponse.RigaLibretto` (nested class).
- **Attributes:**
  - `esami: List<RigaLibretto>` - List of exam records (rows from the transcript)
  - `totaleEsami: Integer` - Total number of exams registered in the career
  - `dataUltimoAggiornamento: LocalDateTime` - Timestamp of last update from ESSE3
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: LibrettoResponse.RigaLibretto**

- **Description:** Represents a single exam record (riga) in the student's transcript.
- **Attributes:**
  - `adId: Long` - Cineca activity identifier (course ID)
  - `nomeInsegnamento: String` - Course/exam name
  - `docente: String` - Instructor name
  - `annoAccademico: String` - Academic year when the exam was taken
  - `dataEsame: LocalDate` - Date the exam was held
  - `voto: Double` - Grade received (typically 18-30 or null if pending)
  - `cfu: Double` - Academic credits for this course
  - `flagSuperato: Boolean` - Whether the exam was successfully passed
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: MediaResponse**

- **Description:** Data transfer object containing calculated academic statistics for the student, including weighted and arithmetic averages and graduation projection.
- **Dependencies:** None (POJO).
- **Attributes:**
  - `mediaAritmetica: Double` - Arithmetic mean of all exam grades
  - `mediaPonderata: Double` - Weighted average: $$\text{weighted avg} = \frac{\sum (\text{grade} \times \text{CFU})}{\sum \text{CFU}}$$
  - `baseLaurea: Double` - Baseline grade used for graduation calculation (typically derived from weighted average)
  - `percentualeCfu: Double` - Percentage of CFU completed relative to the program total
  - `cfuTotaliCompletati: Double` - Total credits acquired so far
  - `cfuTotaliPrevisti: Double` - Total credits required for degree completion
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: PianoStudioResponse**

- **Description:** Data transfer object representing the student's study plan (piano di studi), listing planned courses and associated credits.
- **Dependencies:** `PianoStudioResponse.RigaPiano` (nested class).
- **Attributes:**
  - `righeInserimento: List<RigaPiano>` - List of planned courses in the study plan
  - `statoPiano: String` - Status of the plan (e.g., "APPROVED", "PENDING", "REJECTED")
  - `dataApprovazione: LocalDate` - Date the study plan was approved (null if pending)
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: PianoStudioResponse.RigaPiano**

- **Description:** Represents a single course entry in the student's study plan (riga di inserimento).
- **Attributes:**
  - `adId: Long` - Cineca activity identifier (course ID)
  - `nomeInsegnamento: String` - Course name
  - `cfuPrevisti: Double` - Expected credits for this course
  - `annoAccademico: String` - Academic year when the course is scheduled
  - `tipoCfu: String` - Type of credits (e.g., "OBB" for obligatory, "OPZ" for optional)
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: AppelloResponse**

- **Description:** Data transfer object containing available exam sessions (appelli) for a specific course activity.
- **Dependencies:** `AppelloResponse.Appello` (nested class).
- **Attributes:**
  - `appelli: List<Appello>` - List of available exam sessions for the activity
  - `totaleAppelli: Integer` - Total number of available sessions
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: AppelloResponse.Appello**

- **Description:** Represents a single exam session (appello) with scheduling and availability information.
- **Attributes:**
  - `appelliId: Long` - Cineca exam session identifier
  - `descrizione: String` - Session description (e.g., "Winter Session 2026")
  - `dataInizio: LocalDate` - Session start date
  - `dataFine: LocalDate` - Session end date
  - `dataRitiro: LocalDate` - Last date to withdraw registration without penalty
  - `luogo: String` - Location/room where the exam will be held
  - `flagAperto: Boolean` - Whether the session is open for student bookings
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: BadgeResponse**

- **Description:** Data transfer object representing the student's digital university badge with academic recognition information.
- **Dependencies:** None (POJO).
- **Attributes:**
  - `nomeStato: String` - Current academic status name (e.g., "Studente Regolare", "Fuori Corso")
  - `descrizioneStato: String` - Human-readable description of the status
  - `dataGenerazione: LocalDateTime` - When the badge was generated
  - `valoreHash: String` - Verification hash for badge authenticity
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: PrenotazioneRequest**

- **Description:** Data transfer object containing the parameters needed to create an exam booking (prenotazione) for a student.
- **Dependencies:** None (POJO).
- **Attributes:**
  - `appelliId: Long` - Target exam session identifier
  - `adId: Long` - Course/activity identifier
  - `adGruppoId: Long` - Optional: specific exam group identifier for grouped exams
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

---

**Class: PrenotazioneResponse**

- **Description:** Data transfer object containing the result of exam booking operations and the student's complete booking history.
- **Dependencies:** `PrenotazioneResponse.Prenotazione`, `PrenotazioneResponse.EsitoPrenotazione` (nested classes).
- **Attributes:**
  - `prenotazioni: List<Prenotazione>` - List of active/past bookings for the student
  - `esito: EsitoPrenotazione` - Result of the most recent booking attempt
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: PrenotazioneResponse.Prenotazione**

- **Description:** Represents a single exam booking (prenotazione) record in the student's booking history.
- **Attributes:**
  - `prenotazioneId: Long` - Unique booking identifier
  - `appelliId: Long` - Associated exam session identifier
  - `adId: Long` - Associated course/activity identifier
  - `dataPrenotazione: LocalDateTime` - When the booking was created
  - `dataRitiro: LocalDate` - Deadline to cancel/withdraw the booking
  - `stato: String` - Booking status (e.g., "CONFERMATA", "ANNULLATA", "GIUSTIFICATA")
  - `numeroPos: Integer` - Position in the exam queue (null if not queued)
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.

**Nested Class: PrenotazioneResponse.EsitoPrenotazione**

- **Description:** Encapsulates the result and diagnostic information from a booking attempt.
- **Attributes:**
  - `esito: String` - Success/failure indicator (e.g., "OK", "ERROR", "CONFLICT")
  - `messaggio: String` - Human-readable message explaining the result
  - `codiceErrore: String` - Error code for machine-readable error handling (null if successful)
  - `prenotazioneId: Long` - ID of the created booking (null if booking failed)
- **Operations:**
  - Automatically generated getter and setter methods for attribute access.
