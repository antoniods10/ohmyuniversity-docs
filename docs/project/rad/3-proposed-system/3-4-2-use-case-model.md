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

## Overview

This model elaborates on the functional view of the system by identifying the actors and defining their specific goals.
Use Case modeling allows for the abstraction of the behaviors described in the scenarios into reusable and testable functionalities,
delimiting the operational boundaries between **OhMyUniversity!** and the external environment.

## Actors

The following table identifies the actors involved in the **OhMyUniversity!** Use Case Model. Actors are divided into primary actors, who directly interact with the system to achieve their goals, and secondary actors, which represent external systems or entities supporting specific functionalities.

| Type      | Actor                                             | Description                                                                                                                                                                      |
| --------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Primary   | University Student                                | The main user who interacts with the system to manage their career, view schedules, consult educational material, and book campus services.                                      |
| Primary   | Partner Organization                              | A company, association, collective entity or external partner that can submit a registration request to participate in the partnership area of OhMyUniversity.                  |
| Secondary | Cineca/Esse3                                      | External system queried for the synchronization of the university record book, grades, and tax status.                                                                           |
| Secondary | Moodle                                            | E-learning platform that provides access to educational content and courses.                                                                                                     |
| Secondary | University Authentication System / Identity Provider | External authentication service used to validate users who already own official university credentials or supported digital identity credentials.                                 |
| Secondary | Map Provider                                      | Provides geolocation services and cartographic information for locations and transport, such as Google Maps or OpenStreetMap.                                                    |
| Secondary | Canteen Management System                         | External entity that exposes daily menus and validates benefits via Tax Code for meal consumption.                                                                               |

## General Use Case Diagram

![use-case-diagram](/diagrams/final-user-case-model.webp)

## Use Case List and Traceability Matrix

The following traceability matrix links each identified Use Case to the corresponding functional or nonfunctional requirements. This mapping ensures that every relevant system functionality is connected to at least one requirement and can be verified during later validation and testing activities.

| ID Use Case | Use Case                            | Functional Requirements                           |
| ----------- | ----------------------------------- | ------------------------------------------------- |
| UC-01       | Authenticate User                   | NFR Authentication, Security                      |
| UC-02       | View Academic Career                | FR-1.1.1, FR-1.1.2, FR-1.1.3, FR-1.1.4, FR-1.1.5 |
| UC-03       | View Study Plan                     | FR-1.1.6                                          |
| UC-04       | Manage Exam Sessions                | FR-1.1.7, FR-1.2.4                                |
| UC-05       | View Administrative Information     | FR-1.1.8                                          |
| UC-06       | Use Digital Badge                   | FR-1.1.9                                          |
| UC-07       | Access External University Services | FR-1.2.1, FR-1.2.2, FR-1.2.3                      |
| UC-08       | View and Download Didactic Material | FR-1.3.1                                          |
| UC-09       | Manage Classroom Booking            | FR-1.3.2, FR-1.3.3                                |
| UC-10       | Manage Canteen Services             | FR-1.3.7, FR-1.3.8                                |
| UC-11       | View Maps and Transportation        | FR-1.3.6                                          |
| UC-12       | View Master’s Degree Requirements   | FR-1.4.1, FR-1.4.2                                |
| UC-13       | Access Orientation Services         | FR-1.4.3                                          |
| UC-14       | View University News                | FR-1.5.1                                          |
| UC-15       | Configure Quick Access              | FR-1.5.2                                          |
| UC-16       | Use Chat System                     | FR-1.5.3                                          |
| UC-17       | Monitor Attendance                  | FR-1.3.5                                          |
| UC-18       | Manage Integrated Calendar          | FR-1.3.4                                          |
| UC-19       | Register Partner Organization       | FR-1.6.2                                          |
| UC-20       | View Agreements, Jobs and Conventions | FR-1.6.1                                        |

## Detailed Use Case Specifications

<br>

### UC-01: Authenticate User

| Section              | Content                                                                                                                                                                                                                                                                              |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Actors**           | **Primary Actor:** Student / Partner User • **Secondary Actors:** OhMyUniversity Authentication Service, University Authentication System, SPID/CIE Identity Provider                                                                                                                |
| **Assumptions**      | The user owns valid university credentials, supported digital identity credentials, or partner credentials. • The selected university is supported by OhMyUniversity. • The authentication services are reachable. • The authenticated account is authorized to access the platform. |
| **Entry Conditions** | The OhMyUniversity app is open and connected to the internet. • The login page is available. • The user has not yet started an authenticated session.                                                                                                                                |

| **Event Flow** | **User**                                                                                         | **System**                                                                                              |
| -------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| 1.             | The user opens the login page inside the OhMyUniversity application.                             |                                                                                                         |
| 2.             | The user selects the access mode: university login, SPID/CIE login, or partner login.            |                                                                                                         |
| 3.             | If the user chooses university login, the user selects their university from the available list. |                                                                                                         |
| 4.             |                                                                                                  | The system enables the credential fields according to the selected university.                          |
| 5.             | The user enters their university credentials.                                                    |                                                                                                         |
| 6.             |                                                                                                  | The system sends the credentials to the authentication service associated with the selected university. |
| 7.             |                                                                                                  | The authentication service validates the credentials and returns an authorization response.             |
| 8.             |                                                                                                  | The system validates the received response and loads the corresponding user profile.                    |
| 9.             |                                                                                                  | The system initializes a secure user session.                                                           |
| 10.            |                                                                                                  | The system redirects the authenticated user to the main dashboard.                                      |

| **Alternative Flows**   | **User**                                                  | **System**                                                                                                                             |
| ----------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **A1 - SPID/CIE login** | The user selects the SPID or CIE access option.           |                                                                                                                                        |
| 1.                      |                                                           | The system starts the authentication flow with the selected digital identity provider.                                                 |
| 2.                      | The user completes the authentication using SPID or CIE.  |                                                                                                                                        |
| 3.                      |                                                           | The identity provider returns an authentication result.                                                                                |
| 4.                      |                                                           | The system validates the result, loads the user profile, initializes the session and redirects the user to the main dashboard.         |
| **A2 - Partner login**  | The partner user selects the partner access option.       |                                                                                                                                        |
| 1.                      | The partner user enters the required partner credentials. |                                                                                                                                        |
| 2.                      |                                                           | The system validates the partner credentials.                                                                                          |
| 3.                      |                                                           | If the credentials are valid, the system initializes a secure partner session and redirects the user to the partner area or dashboard. |

| Section             | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Exceptions**      | **No connection:** The system warns the user and prevents the login from starting. • **University not selected:** The system keeps the credential fields disabled or shows a validation message. • **Unsupported university:** The system informs the user that the selected university is not currently available. • **Incorrect credentials:** The system rejects the login and shows an error message. • **SPID/CIE cancellation or failure:** The system returns the user to the login page and displays an error message. • **Partner credentials not valid:** The system denies access and shows an authentication error. • **Authentication service unreachable:** The system advises the user to try again later. |
| **Exit Conditions** | The user is securely authenticated with an active session. • The system has loaded the profile associated with the authenticated account. • The user is redirected to the correct area of the application according to their access type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |


<br>

### UC-02: View Academic Career

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assumptions** | The student’s academic records and grades are correctly registered and accessible on the external University Academic System. |
| **Entry Conditions** | The student is authenticated. • The student has an active academic career. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Academic Career"** module. | |
| 2. | | The system requests updated academic records from the University Academic System (**`<<include>>` UC-17 Synchronize Academic Data**). |
| 3. | | The University Academic System returns the student transcript, including exams, grades and CFU. |
| 4. | | The system calculates the arithmetic average, weighted average, projected degree base grade and CFU completion percentage (**`<<include>>` UC-18 Calculate Academic Metrics**). |
| 5. | | The system displays the comprehensive career dashboard, including academic data, calculated metrics and performance charts. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **University Academic System unreachable / timeout:** The system loads the latest cached version of the academic data and displays the banner **"Offline Mode - Data not updated"**. |
| **Exit Conditions** | The student views an accurate and fully calculated representation of their academic career. |

<br>

### UC-03: View Study Plan

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assumptions** | The study plan has been submitted, officially approved, and properly recorded in the external academic system. |
| **Entry Conditions** | The student is authenticated. • The student has a study plan officially approved by the student registry. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Study Plan"** module. | |
| 2. | | The system queries the University Academic System APIs to retrieve the structure of the educational plan. |
| 3. | | The system visually divides the exams into categories: **Mandatory**, **Student’s choice**, and **Internships/Suitability**. |
| 4. | | The system highlights the status of each exam: **Passed**, **To be taken**, or **Attended**. |
| 5. | The student navigates through the different academic years. | |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **Study plan pending approval:** If Cineca/Esse3 returns a **"Draft"** or **"To be approved"** status, the system shows a warning notifying the student that the displayed plan may be subject to changes. |
| **Exit Conditions** | The student has a clear and structured view of the exams that make up their academic path. |

<br>

### UC-04: Manage Exam Sessions

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actor:** University Academic Systems (Cineca/Esse3) |
| **Assumptions** | The exam sessions for the student’s courses have been published and opened for booking by the university staff on the external system. |
| **Entry Conditions** | The student is authenticated. • The student is enrolled in the relevant courses. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student navigates to the **"Exam Sessions"** module. | |
| 2. | | The system retrieves active sessions from the external academic system (**`<<include>>` UC-17 Synchronize Academic Data**). |
| 3. | | The system presents available dates, times, and classrooms. |
| 4. | | The system highlights the optimal booking sequence. |
| 5. | The student selects an exam session and confirms the booking. | |
| 6. | | The system forwards the booking request to the external academic system. |
| 7. | | The external academic system registers the booking and returns a confirmation. |
| 8. | | The system updates the student's local booked exams list. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **Booking failed:** If the prerequisites are not met or the external academic system rejects the request, the system displays a message explaining the rejection. • **No exams available:** The system displays a blank state with the message **"No exams currently open for your study plan"**. |
| **Exit Conditions** | The student is officially registered for the selected exam session in the university's central database. |

<br>

### UC-09: Manage Classroom Booking

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student |
| **Assumptions** | The university facilities schedules, available classrooms, and capacities are correctly maintained and accessible by the system. |
| **Entry Conditions** | The student is authenticated. • The requested time slot is within the allowed booking timeframe. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Classrooms"** module. | |
| 2. | The student filters spaces by building, date, or capacity. | |
| 3. | | The system evaluates current occupancy (**`<<include>>` UC-20 Check Classroom Availability**). |
| 4. | | The system lists available classrooms and their maximum capacity. |
| 5. | The student selects a specific space or seat and confirms the reservation. | |
| 6. | | The system registers the booking and updates the remaining capacity. |
| 7. | | The system provides a booking confirmation. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **Seat or space no longer available:** If another student reserves the same space or seat between availability visualization and confirmation, the system notifies the student and updates the availability. |
| **Exit Conditions** | The selected space or seat is formally reserved and removed from general availability. |

<br>

### UC-10: Manage Canteen Services

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actor:** University Canteen Service |
| **Assumptions** | The external Canteen Management System is operational, and the daily menus and maximum seating capacity are correctly updated. |
| **Entry Conditions** | The student is authenticated. • The Canteen Service has published the daily menu and available time slots. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Canteen"** module. | |
| 2. | | The system retrieves and displays the daily menu from the Canteen Management System. |
| 3. | The student selects the preferred dining time slot and meal items they intend to consume. | |
| 4. | | The system verifies seat capacity (**`<<include>>` UC-21 Check Canteen Availability**). |
| 5. | | The system validates that the selected items belong to the daily menu and requests the canteen service to apply student discounts or benefits using the student’s Tax Code (**`<<include>>` UC-22 Validate Canteen Benefit**). |
| 6. | | The system finalizes the reservation, submits the meal selection to the Canteen Management System, and issues a QR code or digital receipt. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **Tax Code not recognized for the discount:** The system notifies the student and offers a full-price reservation. • **Menu unavailable:** The system informs the student that the daily menu is temporarily unavailable. • **Canteen Management System unavailable:** The system informs the student that the seat reservation or meal selection cannot be completed. |
| **Exit Conditions** | A canteen seat is formally reserved, the meal selection is recorded, and financial benefits are applied if the student is eligible. |

<br>

### UC-11: View Maps and Transportation

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actor:** Map Provider |
| **Assumptions** | The student’s device has a functional GPS module, and the external Map Provider services are active and reachable. |
| **Entry Conditions** | The student is authenticated. • The student has granted location permissions to the application. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Maps and Logistics"** module. | |
| 2. | | The system obtains the student’s current coordinates through the device’s GPS. |
| 3. | | The system queries the Map Provider to obtain walking routes, nearby public transport stops, and the campus map. |
| 4. | | The system overlays university points of interest, such as classrooms and libraries, on the map. |
| 5. | The student selects a destination university campus. | |
| 6. | | The system displays the travel time and suggested public transport routes. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **GPS permissions denied:** The system displays the general campus map without calculating the distance or a dynamic starting point, asking the student to manually input the starting location. • **Lack of connection:** The system cannot load real-time data and only shows a pre-downloaded static map of the university. |
| **Exit Conditions** | The student has clear spatial and logistical directions to reach the selected academic destination. |

<br>

### UC-12: Evaluate Master’s Degree Requirements

|                           |                                                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Attori**                | **Primary Actor:** Student                                                                                                                            |
| **Assunzioni**            | The admission requirements and specific regulations for the Master’s degree programs are accurately digitized and available in the system’s database. |
| **Condizioni di entrata** | The student is authenticated. • The student’s career data is synchronized. • Master's degree requirements are available in the system.                |

| **Flusso degli eventi** | **Student**                                                                                        | **System**                                                                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1.**                  | The student navigates to the **"Master's Degree Planning"** module.                                |                                                                                                                                                         |
| **2.**                  |                                                                                                    | The system lists available Master's degree programs.                                                                                                    |
| **3.**                  | The student selects a target program.                                                              |                                                                                                                                                         |
| **4.**                  |                                                                                                    | The system cross-references the program's mandatory requirements, such as specific CFU in specific scientific sectors, with the student's passed exams. |
| **5.**                  | The student manually checks off any external or pending requirements using interactive checkboxes. |                                                                                                                                                         |
| **6.**                  |                                                                                                    | The system calculates the overall eligibility percentage (**`<<include>>` UC-23 Calculate Requirement Completion**).                                    |
| **7.**                  |                                                                                                    | The system visually highlights missing requirements and displays the final eligibility status.                                                          |

|                          |                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Eccezioni**            | **Master’s Degree regulations unavailable:** The system notifies the student that the data for the selected program is being updated. |
| **Condizione di uscita** | The student receives a clear, percentage-based assessment of their eligibility for the chosen Master's degree program.                |

<br>

### UC-12: Evaluate Master’s Degree Requirements

|                           |                                                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Attori**                | **Primary Actor:** Student                                                                                                                            |
| **Assunzioni**            | The admission requirements and specific regulations for the Master’s degree programs are accurately digitized and available in the system’s database. |
| **Condizioni di entrata** | The student is authenticated. • The student’s career data is synchronized. • Master's degree requirements are available in the system.                |

| **Flusso degli eventi** | **Student**                                                                                        | **System**                                                                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1.**                  | The student navigates to the **"Master's Degree Planning"** module.                                |                                                                                                                                                         |
| **2.**                  |                                                                                                    | The system lists available Master's degree programs.                                                                                                    |
| **3.**                  | The student selects a target program.                                                              |                                                                                                                                                         |
| **4.**                  |                                                                                                    | The system cross-references the program's mandatory requirements, such as specific CFU in specific scientific sectors, with the student's passed exams. |
| **5.**                  | The student manually checks off any external or pending requirements using interactive checkboxes. |                                                                                                                                                         |
| **6.**                  |                                                                                                    | The system calculates the overall eligibility percentage (**`<<include>>` UC-23 Calculate Requirement Completion**).                                    |
| **7.**                  |                                                                                                    | The system visually highlights missing requirements and displays the final eligibility status.                                                          |

|                          |                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Eccezioni**            | **Master’s Degree regulations unavailable:** The system notifies the student that the data for the selected program is being updated. |
| **Condizione di uscita** | The student receives a clear, percentage-based assessment of their eligibility for the chosen Master's degree program.                |

<br>

### UC-14: View University News

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student • **Secondary Actors:** University Information System, Corporate Partners |
| **Related Requirements** | FR-1.5.1 |
| **Assumptions** | The University Information System and Corporate Partners regularly provide active, correctly formatted data regarding official notices, news, job offers, discounts, and partnerships. |
| **Entry Conditions** | The student has accessed the OhMyUniversity platform. • The student is authenticated. • The News and Extra Noticeboard modules are available. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"News & Extra Noticeboard"** module. | |
| 2. | | The system retrieves RSS feeds and API data containing official university notices from the University Information System, alongside deals and job offers from Corporate Partners. |
| 3. | | The system aggregates the information and presents it in a clear, filterable dashboard divided into cards or tabs (e.g., **News**, **Jobs**, **Discounts**). |
| 4. | The student selects a specific news item, notice, or corporate deal. | |
| 5. | | The system displays the full details of the selected item. If it is a partner deal, it displays options to generate a discount code (**`<<extend>>` UC-25**) or redirect the student (**`<<extend>>` UC-26**). |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **University Information System unavailable / News unavailable:** The system displays an error message informing the student that official university notices are temporarily unavailable. <br><br> **Partner Companies feed unavailable:** The system temporarily hides the **"Deals/Jobs"** tab, displays a notification about temporary service degradation, and only shows available University News. <br><br> **No active items:** If a category is empty, the system displays a blank state indicating that no active news or offers are currently available. |
| **Exit Conditions** | The student has successfully consulted official university updates or extracurricular opportunities, and optionally interacted with partnership services. |

<br>

### UC-16: Use Chat System

| Section | Content |
| ------- | ------- |
| **Actors** | **Primary Actor:** Student |
| **Assumptions** | The recipient of the message is a registered user of the OhMyUniversity platform. |
| **Entry Conditions** | The student is authenticated. • The student has an active network connection. |

| **Event Flow** | **Student** | **System** |
| -------------- | ----------- | ---------- |
| 1. | The student accesses the **"Chat/Messages"** module. | |
| 2. | | The system retrieves the list of active conversations, including individual and group chats, and the recent message history. |
| 3. | The student selects an existing chat or starts a new one by searching for a colleague/contact. | |
| 4. | The student types a text message and taps **send**. | |
| 5. | | The system delivers the message to the recipient or recipients and updates the chat UI. |

| Section | Content |
| ------- | ------- |
| **Exceptions** | **Network connection lost:** The system notifies the student that they are offline. The message is queued locally and will be automatically sent once the connection is restored. • **Recipient unavailable or blocked:** The system shows an error icon next to the message, warning the student that the delivery failed. |
| **Exit Conditions** | The message is successfully sent, delivered to the server, and displayed in the conversation history. |
