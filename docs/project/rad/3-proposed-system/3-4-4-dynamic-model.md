---
title: RAD - 3.4.4 Dynamic Model | OhMyUniversity!
description: Dynamic model of OhMyUniversity - describing system behavior over time through sequence diagrams and statechart diagrams.
head:
  - - meta
    - property: og:title
      content: RAD - 3.4.4 Dynamic Model | OhMyUniversity!
  - - meta
    - property: og:description
      content: Dynamic model of OhMyUniversity - describing system behavior over time through sequence diagrams and statechart diagrams.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/rad/3-proposed-system/3-4-4-dynamic-model
  - - meta
    - name: keywords
      content: ohmyuniversity, rad, dynamic model, uml, sequence diagram, statechart diagram, system behavior, object interactions, state transitions, requirements analysis
  - - meta
    - name: twitter:title
      content: RAD - 3.4.4 Dynamic Model | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Dynamic model of OhMyUniversity - describing system behavior over time through sequence diagrams and statechart diagrams.
---

# OhMyUniversity! - RAD: 3.4.4 Dynamic Model

The Dynamic Model describes the behavior of OhMyUniversity over time by showing how the main system objects interact and evolve during the execution of selected use cases. While the Object Model defines the static structure of the domain, the Dynamic Model focuses on the temporal dimension of the system: the order of interactions, the responsibilities of the participating objects, and the lifecycle of the most relevant domain entities.

The model is represented through UML Sequence Diagrams and Statechart Diagrams. The Sequence Diagrams describe how actors, boundary objects, control objects, domain entities and external systems collaborate to realize the most significant use cases of the platform. They are not provided for every use case, but only for the most representative and complex interactions, especially those involving external systems, relevant domain objects or non-trivial coordination logic.

The selected Sequence Diagrams cover the following use cases:

- UC-01 Authenticate User
- UC-02 View Academic Career
- UC-04 Manage Exam Sessions
- UC-09 Manage Classroom Booking
- UC-10 Manage Canteen Services
- UC-12 View Master’s Degree Requirements

In addition, Statechart Diagrams are used to model the lifecycle of selected objects whose behavior depends on state changes over time. These diagrams are applied only to objects with a meaningful lifecycle, such as sessions, bookings, meal orders and partner organizations. This avoids unnecessary diagrams for purely informational or static entities.

The selected Statechart Diagrams describe the lifecycle of:

- UserSession
- ExamBooking
- ClassroomBooking
- MealOrder
- PartnerOrganization

Together, these diagrams provide a dynamic view of the system and help validate the consistency between the Use Case Model and the Object Model.


## User Sequences

### UC-01 Authenticate User
![user-sequence-uc1](/diagrams/user-sequences/user-sequence-uc1.webp)

### UC-02 View Academic Career
![user-sequence-uc2](/diagrams/user-sequences/user-sequence-uc2.webp)

### UC-04 Manage Exam Sessions
![user-sequence-uc4](/diagrams/user-sequences/user-sequence-uc4.webp)

### UC-09 Manage Classroom Booking
![user-sequence-uc9](/diagrams/user-sequences/user-sequence-uc9.webp)

### UC-10 Manage Canteen Services
![user-sequence-uc10](/diagrams/user-sequences/user-sequence-uc1O.webp)

### UC-12 View Master’s Degree Requirements
![user-sequence-uc12](/diagrams/user-sequences/user-sequence-uc12.webp)

## Statechart Diagrams

### Esame Booking
![Esame-Booking](/diagrams/statecharts/EsameBooking.webp)

### Meal Order
![Esame-Booking](/diagrams/statecharts/MealOrder.webp)

### Partner Organization
![Esame-Booking](/diagrams/statecharts/PartnerOrganization.webp)

### User Authentication
![Esame-Booking](/diagrams/statecharts/MealOrder.webp)


