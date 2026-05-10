---
title: RAD - 3.4.2 Use Case Model | OhMyUniversity!
description: Use Case Model of OhMyUniversity - defining actors, system goals, use cases, traceability, and the UML functional view of the system.
head:
  - - meta
    - property: og:title
      content: RAD - 3.4.2 Use Case Model | OhMyUniversity!
  - - meta
    - property: og:description
      content: Use Case Model of OhMyUniversity - defining actors, system goals, use cases, traceability, and the UML functional view of the system.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/rad/3-proposed-system/3-4-2-use-case-model
  - - meta
    - name: keywords
      content: ohmyuniversity, rad, use case model, use cases, uml, actors, functional model, traceability matrix, system boundaries, requirements analysis, student portal, middleware
  - - meta
    - name: twitter:title
      content: RAD - 3.4.2 Use Case Model | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Use Case Model of OhMyUniversity - defining actors, system goals, use cases, traceability, and the UML functional view of the system.
---

# OhMyUniversity! - RAD: 3.4.2 Use Case Model

## 3.4.2.1 Overview

This model elaborates on the functional view of the system by identifying the actors and defining their specific goals. 
Use Case modeling allows for the abstraction of the behaviors described in the scenarios into reusable and testable functionalities, 
delimiting the operational boundaries between **OhMyUniversity!** and the external environment.

## 3.4.2.2 Actors

The following table identifies the actors involved in the **OhMyUniversity!** Use Case Model. Actors are divided into primary actors, who directly interact with the system to achieve their goals, and secondary actors, which represent external systems or entities supporting specific functionalities.

| Type | Actor | Description |
|---|---|---|
| Primary | University Student | The main user who interacts with the system to manage their career, view schedules, consult educational material, and book campus services. |
| Secondary | Cineca/Esse3 | External system queried for the synchronization of the university record book, grades, and tax status. |
| Secondary | Moodle | E-learning platform that provides access to educational content and courses. |
| Secondary | Identity Provider (SPID/CIE) | System delegated to secure validation of the student's identity during the authentication phase. |
| Secondary | Map Provider | Provides geolocation services and cartographic information for locations and transport, such as Google Maps or OpenStreetMap. |
| Secondary | Canteen Management System | External entity that exposes daily menus and validates benefits via Tax Code for meal consumption. |
| Secondary | Partner Companies | External entities that provide data related to conventions, discounts, and job announcements dedicated to students. |

## 3.4.2.3 General Use Case Diagram
![use-case-diagram](/diagrams/uml-user-case.webp)

## 3.4.2.4 Use Case List and Traceability Matrix

The following traceability matrix links each identified Use Case to the corresponding functional or nonfunctional requirements. This mapping ensures that every relevant system functionality is connected to at least one requirement and can be verified during later validation and testing activities.

| ID Use Case | Use Case | Functional Requirements |
|---|---|---|
| UC-01 | Authenticate User | NFR Authentication, Security |
| UC-02 | View Academic Career | FR-1.1.1, FR-1.1.2, FR-1.1.3, FR-1.1.4, FR-1.1.5 |
| UC-03 | View Study Plan | FR-1.1.6 |
| UC-04 | Manage Exam Sessions | FR-1.1.7, FR-1.2.4 |
| UC-05 | View Administrative Information | FR-1.1.8 |
| UC-06 | Use Digital Badge | FR-1.1.9 |
| UC-07 | Access External University Services | FR-1.2.1, FR-1.2.2, FR-1.2.3 |
| UC-08 | View and Download Didactic Material | FR-1.3.1 |
| UC-09 | Manage Classroom Booking | FR-1.3.2, FR-1.3.3 |
| UC-10 | Manage Canteen Services | FR-1.3.7, FR-1.3.8, FR-1.3.9 |
| UC-11 | View Maps and Transportation | FR-1.3.6 |
| UC-12 | Evaluate Master’s Degree Requirements | FR-1.4.1, FR-1.4.2 |
| UC-13 | Access Orientation Services | FR-1.4.3 |
| UC-14 | View News, Jobs and Conventions | FR-1.5.1, FR-1.5.2 |
| UC-15 | Configure Quick Access | FR-1.5.3 |
| UC-16 | Use Chat System | FR-1.5.4 |
| UC-17 | Monitor Attendance | FR-1.3.5 |
| UC-18 | Manage Integrated Calendar | FR-1.3.4 |

## 3.4.2.5 Detailed Use Case Specifications

<br>

### UC-01: Authenticate User

| Sezione | Contenuto |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** Identity Provider (SPID/CIE) |
| **Assunzioni** | The student has valid SPID or CIE credentials. |
| **Condizioni di entrata** | The OhMyUniversity app is open and connected to the internet.<br>The Identity Provider service is reachable. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| 1. | The student selects **"Log in with SPID/CIE"**. |  |
| 2. |  | The system redirects the student to the Identity Provider's gateway. |
| 3. | The student inputs their credentials into the external portal. |  |
| 4. |  | The Identity Provider validates the identity and returns an authorization token. |
| 5. |  | The system validates the token and initializes a secure user session. |
| 6. |  | The system redirects the student to the main dashboard. |

| Sezione | Contenuto |
|---|---|
| **Eccezioni** | **No connection:** The system warns the user and prevents the login from starting.<br><br>**Incorrect credentials or cancellation:** The Identity Provider returns an error; the system returns the user to the initial login screen with an error message.<br><br>**Identity Provider unreachable:** The system advises the user to try again later. |
| **Condizione di uscita** | The student is securely authenticated with an active session. |

<br>

### UC-02: View Academic Career

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assunzioni** | The student’s academic records and grades are correctly registered and accessible on the external University Academic System. |
| **Condizioni di entrata** | The student is authenticated.<br>The student has an active academic career. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Academic Career"** module. |  |
| **2.** |  | The system requests updated academic records from the University Academic System (**<<include>> UC-17 Synchronize Academic Data**). |
| **3.** |  | The University Academic System returns the student transcript, including exams, grades and CFU. |
| **4.** |  | The system calculates the arithmetic average, weighted average, projected degree base grade and CFU completion percentage (**<<include>> UC-18 Calculate Academic Metrics**). |
| **5.** |  | The system displays the comprehensive career dashboard, including academic data, calculated metrics and performance charts. |

|  |  |
|---|---|
| **Eccezioni** | **University Academic System unreachable / timeout:** The system loads the latest cached version of the academic data within 500ms, according to the fault tolerance requirement, and displays the banner **"Offline Mode - Data not updated"**. |
| **Condizione di uscita** | The student views an accurate and fully calculated representation of their academic career. |

<br>

### UC-03: View Study Plan

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assunzioni** | The study plan has been submitted, officially approved, and properly recorded in the external academic system. |
| **Condizioni di entrata** | The student is authenticated.<br>The student has a study plan officially approved by the student registry. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Study Plan"** module. |  |
| **2.** |  | The system queries the University Academic System APIs to retrieve the structure of the educational plan. |
| **3.** |  | The system visually divides the exams into categories: **Mandatory**, **Student’s choice**, and **Internships/Suitability**. |
| **4.** |  | The system highlights the status of each exam: **Passed**, **To be taken**, or **Attended**. |
| **5.** | The student navigates through the different academic years. |  |

|  |  |
|---|---|
| **Eccezioni** | **Study plan pending approval:** If Cineca/Esse3 returns a **"Draft"** or **"To be approved"** status, the system shows a warning notifying the student that the displayed plan may be subject to changes. |
| **Condizione di uscita** | The student has a clear and structured view of the exams that make up their academic path. |

<br>

### UC-04: Manage Exam Sessions

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assunzioni** | The exam sessions for the student’s courses have been published and opened for booking by the university staff on the external system. |
| **Condizioni di entrata** | The student is authenticated.<br>The student is enrolled in the relevant courses. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student navigates to the **"Exam Sessions"** module. |  |
| **2.** |  | The system retrieves active sessions from the external academic system (**<<include>> UC-17 Synchronize Academic Data**). |
| **3.** |  | The system presents available dates, times, and classrooms. |
| **4.** |  | The system highlights the optimal booking sequence. |
| **5.** | The student selects an exam session and confirms the booking. |  |
| **6.** |  | The system forwards the booking request to the external academic system. |
| **7.** |  | The external academic system registers the booking and returns a confirmation. |
| **8.** |  | The system updates the student's local booked exams list. |

|  |  |
|---|---|
| **Eccezioni** | **Booking failed:** If the prerequisites are not met or the external academic system rejects the request, the system displays a message explaining the rejection.<br><br>**No exams available:** The system displays a blank state with the message **"No exams currently open for your study plan"**. |
| **Condizione di uscita** | The student is officially registered for the selected exam session in the university's central database. |

<br>

### UC-09: Manage Classroom Booking

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student |
| **Assunzioni** | The university facilities schedules, available classrooms, and capacities are correctly maintained and accessible by the system. |
| **Condizioni di entrata** | The student is authenticated.<br>The requested time slot is within the allowed booking timeframe. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Classrooms"** module. |  |
| **2.** | The student filters spaces by building, date, or capacity. |  |
| **3.** |  | The system evaluates current occupancy (**<<include>> UC-20 Check Classroom Availability**). |
| **4.** |  | The system lists available classrooms and their maximum capacity. |
| **5.** | The student selects a specific space or seat and confirms the reservation. |  |
| **6.** |  | The system registers the booking and updates the remaining capacity. |
| **7.** |  | The system provides a booking confirmation. |

|  |  |
|---|---|
| **Eccezioni** | **Seat or space no longer available:** If another student reserves the same space or seat between availability visualization and confirmation, the system notifies the student and updates the availability. |
| **Condizione di uscita** | The selected space or seat is formally reserved and removed from general availability. |

<br>

### UC-10: Manage Canteen Services

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** University Canteen Service |
| **Assunzioni** | The external Canteen Management System is operational, and the daily menus and maximum seating capacity are correctly updated. |
| **Condizioni di entrata** | The student is authenticated.<br>The Canteen Service has published the daily menu and available time slots. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Canteen"** module. |  |
| **2.** |  | The system displays the daily menu. |
| **3.** | The student selects a preferred dining time slot. |  |
| **4.** |  | The system verifies seat capacity (**<<include>> UC-21 Check Canteen Availability**). |
| **5.** |  | The system requests the canteen service to apply student discounts or benefits using the student’s Tax Code (**<<include>> UC-22 Validate Canteen Benefit**). |
| **6.** |  | The system finalizes the reservation and issues a QR code or digital receipt. |

|  |  |
|---|---|
| **Eccezioni** | **Tax Code not recognized for the discount:** The system notifies the student and offers a full-price reservation. |
| **Condizione di uscita** | A canteen seat is reserved, and financial benefits are applied if the student is eligible. |

<br>

### UC-11: View Maps and Transportation

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** Map Provider |
| **Assunzioni** | The student’s device has a functional GPS module, and the external Map Provider services are active and reachable. |
| **Condizioni di entrata** | The student is authenticated.<br>The student has granted location permissions to the application. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Maps and Logistics"** module. |  |
| **2.** |  | The system obtains the student’s current coordinates through the device’s GPS. |
| **3.** |  | The system queries the Map Provider to obtain walking routes, nearby public transport stops, and the campus map. |
| **4.** |  | The system overlays university points of interest, such as classrooms and libraries, on the map. |
| **5.** | The student selects a destination university campus. |  |
| **6.** |  | The system displays the travel time and suggested public transport routes. |

|  |  |
|---|---|
| **Eccezioni** | **GPS permissions denied:** The system displays the general campus map without calculating the distance or a dynamic starting point, asking the student to manually input the starting location.<br><br>**Lack of connection:** The system cannot load real-time data and only shows a pre-downloaded static map of the university. |
| **Condizione di uscita** | The student has clear spatial and logistical directions to reach the selected academic destination. |

<br>

### UC-12: Evaluate Master’s Degree Requirements

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student |
| **Assunzioni** | The admission requirements and specific regulations for the Master’s degree programs are accurately digitized and available in the system’s database. |
| **Condizioni di entrata** | The student is authenticated.<br>The student’s career data is synchronized.<br>Master's degree requirements are available in the system. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student navigates to the **"Master's Degree Planning"** module. |  |
| **2.** |  | The system lists available Master's degree programs. |
| **3.** | The student selects a target program. |  |
| **4.** |  | The system cross-references the program's mandatory requirements, such as specific CFU in specific scientific sectors, with the student's passed exams. |
| **5.** | The student manually checks off any external or pending requirements using interactive checkboxes. |  |
| **6.** |  | The system calculates the overall eligibility percentage (**<<include>> UC-23 Calculate Requirement Completion**). |
| **7.** |  | The system visually highlights missing requirements and displays the final eligibility status. |

|  |  |
|---|---|
| **Eccezioni** | **Master’s Degree regulations unavailable:** The system notifies the student that the data for the selected program is being updated. |
| **Condizione di uscita** | The student receives a clear, percentage-based assessment of their eligibility for the chosen Master's degree program. |

<br>

### UC-12: Evaluate Master’s Degree Requirements

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student |
| **Assunzioni** | The admission requirements and specific regulations for the Master’s degree programs are accurately digitized and available in the system’s database. |
| **Condizioni di entrata** | The student is authenticated.<br>The student’s career data is synchronized.<br>Master's degree requirements are available in the system. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student navigates to the **"Master's Degree Planning"** module. |  |
| **2.** |  | The system lists available Master's degree programs. |
| **3.** | The student selects a target program. |  |
| **4.** |  | The system cross-references the program's mandatory requirements, such as specific CFU in specific scientific sectors, with the student's passed exams. |
| **5.** | The student manually checks off any external or pending requirements using interactive checkboxes. |  |
| **6.** |  | The system calculates the overall eligibility percentage (**<<include>> UC-23 Calculate Requirement Completion**). |
| **7.** |  | The system visually highlights missing requirements and displays the final eligibility status. |

|  |  |
|---|---|
| **Eccezioni** | **Master’s Degree regulations unavailable:** The system notifies the student that the data for the selected program is being updated. |
| **Condizione di uscita** | The student receives a clear, percentage-based assessment of their eligibility for the chosen Master's degree program. |

<br>

### UC-14: View News, Jobs and Conventions

|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student<br>**Secondary Actor:** Corporate Partner |
| **Assunzioni** | The Corporate Partners and University Information System regularly provide active and correctly formatted data regarding news, offers, jobs, discounts, and conventions. |
| **Condizioni di entrata** | The student is authenticated. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Extra Noticeboard"** module. |  |
| **2.** |  | The system retrieves RSS feeds or API data from the university information systems for news and from Partner Companies for deals and job offers. |
| **3.** |  | The system aggregates the information and presents it in filterable cards, such as **News**, **Jobs**, and **Discounts**. |
| **4.** | The student clicks on a specific offer or deal. |  |
| **5.** |  | The system shows the details and displays a button to generate any discount code (**<<extend>> UC-25**) or to redirect the student to an external service (**<<extend>> UC-26**). |

|  |  |
|---|---|
| **Eccezioni** | **Partner Companies feed unavailable:** The system temporarily hides the **"Deals/Jobs"** tab and only shows University News, notifying the student of a temporary service degradation. |
| **Condizione di uscita** | The student has consulted extracurricular information and may have taken advantage of a partnership service. |

<br>

### UC-16: Use Chat System


|  |  |
|---|---|
| **Attori** | **Primary Actor:** Student |
| **Assunzioni** | The recipient of the message is a registered user of the OhMyUniversity platform. |
| **Condizioni di entrata** | The student is authenticated.<br>The student has an active network connection. |

| **Flusso degli eventi** | **Student** | **System** |
|---|---|---|
| **1.** | The student accesses the **"Chat/Messages"** module. |  |
| **2.** |  | The system retrieves the list of active conversations, including individual and group chats, and the recent message history. |
| **3.** | The student selects an existing chat or starts a new one by searching for a colleague/contact. |  |
| **4.** | The student types a text message and taps **send**. |  |
| **5.** |  | The system delivers the message to the recipient or recipients and updates the chat UI. |

|  |  |
|---|---|
| **Eccezioni** | **Network connection lost:** The system notifies the student that they are offline. The message is queued locally and will be automatically sent once the connection is restored.<br><br>**Recipient unavailable or blocked:** The system shows an error icon next to the message, warning the student that the delivery failed. |
| **Condizione di uscita** | The message is successfully sent, delivered to the server, and displayed in the conversation history. |


