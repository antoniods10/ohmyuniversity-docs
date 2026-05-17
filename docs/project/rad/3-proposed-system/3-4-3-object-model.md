---
title: RAD - 3.4.3 Object Model | OhMyUniversity!
description: Object Model of OhMyUniversity - defining the main domain classes, their responsibilities, and the UML class diagram representing the static structure of the system.
head:
  - - meta
    - property: og:title
      content: RAD - 3.4.3 Object Model | OhMyUniversity!
  - - meta
    - property: og:description
      content: Object Model of OhMyUniversity - defining the main domain classes, their responsibilities, and the UML class diagram representing the static structure of the system.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/rad/3-proposed-system/3-4-3-object-model
  - - meta
    - name: keywords
      content: ohmyuniversity, rad, object model, uml class diagram, domain classes, class responsibilities, static structure, requirements analysis, system models, university app, middleware
  - - meta
    - name: twitter:title
      content: RAD - 3.4.3 Object Model | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Object Model of OhMyUniversity - defining the main domain classes, their responsibilities, and the UML class diagram representing the static structure of the system.
---

# OhMyUniversity! - RAD: 3.4.3 Object Model

## Overview

This model elaborates on the functional view of the system by identifying the actors and defining their specific goals. Use Case modeling allows the behaviors described in the scenarios to be abstracted into reusable and testable functionalities, while also delimiting the operational boundaries between **OhMyUniversity!** and the external environment.

In this perspective, the system is described from the user's point of view, focusing on the objectives that each actor wants to achieve through the platform. The Use Case Model therefore provides a structured representation of the main interactions between users, external services, and the system itself.

This section introduces the actors involved in the system, the general Use Case Diagram, the list of identified use cases, and the traceability between use cases and functional requirements.

## Domain Classes and Responsibilities

This subsection identifies the main domain classes of the **OhMyUniversity!** Object Model and describes their responsibilities. The classes were selected because they represent stable concepts of the university domain and support the main functionalities previously identified in the functional requirements and use cases.

The following table summarizes the main domain classes grouped by functional area.

| Area | Class | Responsibility |
| ---- | ----- | -------------- |
| User and Authentication | UniversityUser | Represents a generic authenticated user of the system. It contains common identification and account information shared by specialized university users. |
| User and Authentication | Student | Represents the main user of OhMyUniversity. The student accesses academic career data, study plans, exam sessions, services, resources and personalized tools. |
| User and Authentication | UserSession | Represents an authenticated user session created after a successful login. It manages the session status, validity and expiration. |
| Academic Career | AcademicCareer | Represents the student’s academic career. It manages academic progress, acquired CFU, grade averages, graduation base score and synchronization date. |
| Academic Career | Exam | Represents an exam recorded in the student’s academic transcript, including grade, CFU, honors, date and status. |
| Academic Career | StudyPlan | Represents the official study plan associated with a student, including the academic year, degree course and approval status. |
| Academic Career | Course | Represents a teaching unit included in the study plan. It connects study plans, exam sessions, didactic materials and attendance records. |
| Exam Sessions | ExamSession | Represents an available exam session for a specific course, including date, time, location, deadline, available seats and booking status. |
| Exam Sessions | ExamBooking | Represents the student’s booking for an exam session. It stores booking date, status and confirmation information. |
| Administrative Area | AdministrativeDocument | Represents administrative documents such as calls, scholarship documents, notices and downloadable administrative files. |
| Administrative Area | TuitionFee | Represents tuition fee information associated with a student, including amount, academic year, due date and payment status. |
| Administrative Area | DigitalBadge | Represents the student’s digital university badge, including validity status and digital identification information. |
| Didactic Material | DidacticMaterial | Represents slides, PDFs and learning resources associated with a course. Moodle is not modeled as an internal class because it is an external platform. |
| Classrooms and Bookings | Classroom | Represents a university classroom, including building, floor, capacity and available equipment. |
| Classrooms and Bookings | ClassroomBooking | Represents a classroom or seat reservation made by a student for a specific time interval. |
| Canteen | CanteenMenu | Represents the daily menu published by the university canteen service. |
| Canteen | MealOrder | Represents the meal selection submitted by a student based on the daily canteen menu. |
| Master’s Degree Planning | MasterDegreeProgram | Represents a Master’s degree programme that the student may consult for future enrollment. |
| Master’s Degree Planning | AdmissionRequirement | Represents an admission requirement associated with a Master’s degree programme and displayed for informational consultation. |
| Master’s Degree Planning | OrientationGuide | Represents orientation information that supports the student in understanding enrollment procedures, available programmes and academic opportunities. |
| Noticeboard, Jobs and Conventions | NoticeboardItem | Represents an item displayed in the student noticeboard, such as university news, notices, events, job offers, discounts or conventions. |
| Noticeboard, Jobs and Conventions | PartnerOrganization | Represents an external company, association, collective entity or partner organization that can submit a registration request and, once approved, provide conventions, discounts, events or job opportunities for students. |
| Resources and Chat | CalendarEvent | Represents an academic event, deadline, exam date or personal reminder displayed in the student calendar. |
| Resources and Chat | AttendanceRecord | Represents a student’s attendance record for a course or didactic activity. |
| Resources and Chat | ChatMessage | Represents a text message exchanged between students through the optional chat system. |

## Object Model Diagram

![uml-object-model](/diagrams/final-object-model.webp)
