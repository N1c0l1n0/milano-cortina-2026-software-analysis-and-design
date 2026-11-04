# APS Project — Milano-Cortina 2026

A university project covering the analysis and design of a software system for managing the Milano-Cortina 2026 Winter Olympics. The deliverables describe the system requirements, domain model, and design of object interactions and responsibilities using UML notation.

The project was completed at the **University of Milano-Bicocca**, Department of Informatics, for the **2025/2026 academic year**, early examination session.

The report and supporting documents are in Italian.

**This repository contains design documentation rather than an executable application:** it includes no source code, installation procedures, database configurations, or automated tests. The features described represent the system's intended behavior.

## Contents

- [Purpose and Scope](#purpose-and-scope)
- [Detailed Use Cases](#detailed-use-cases)
- [Deliverable Structure](#deliverable-structure)
- [How to Explore the Project](#how-to-explore-the-project)
- [Design Decisions](#design-decisions)
- [Open Questions](#open-questions)
- [Authors](#authors)

## Purpose and Scope

The modeled system supports the organization of sporting competitions and the activities of its different users:

- **Visitors:** viewing the calendar, searching for activities and events, purchasing and managing tickets, requesting seats, and receiving notifications.
- **Athletes:** signing up for competitions and registering for training sessions.
- **Coaches:** organizing, updating, and canceling training sessions.
- **Team managers:** viewing rankings and assigning the captain role.
- **System administrator:** updating discount policies.
- **Committee members and treasurer:** managing activities and events and allocating budgets.

An external notification system handles SMS and email delivery. The [use case diagram](<01-use-case-diagram/use-case-diagram.jpg>) shows the overall scope; four use cases are developed in detail in the report.

## Detailed Use Cases

### Update Discounts

The administrator reviews existing policies and updates discount percentages and ticket combination criteria. The system must validate the values, prevent invalid percentages from being saved, and apply changes without requiring a restart.

The use case, the operation contract for `inserisciNuovoSconto(combinazioneBiglietti, percentuale)`, and the rationale for the patterns are included in the [complete report](<report/milano-cortina-2026-software-analysis-and-design.pdf>). The [system sequence diagram](<04-system-sequence-diagrams/update-discounts.jpg>) and [design sequence diagram](<07-design-sequence-diagrams/update-discounts.png>) are also available.

### Request Seats

An authenticated visitor requests one or more seats for an event whose ticket sales have not yet opened or whose tickets are sold out. The request is associated with the visitor and their chosen notification channel, SMS or email; a notification is expected when sales open or tickets become available again.

Whether seats should be temporarily held after the notification remains to be defined: the reservation should not be interpreted as a completed purchase.

References: [detailed use case](<03-use-cases/request-seats.docx>), [operation contract for `confermaPrenotazione`](<05-operation-contracts/request-seats.docx>), and [sequence diagram](<07-design-sequence-diagrams/request-seats.jpg>).

### Manage Training Sessions

The coach creates and publishes sessions, specifying the date, time, location, maximum number of athletes, and whether the session covers a single topic or multiple topics. The requirements specify:

- updates are allowed only when **more than 2 hours** remain before the session starts;
- cancellation is allowed only when **more than 1 hour** remains before the session starts;
- notification must be requested for athletes already registered if the session is canceled.

References: [detailed use case](<03-use-cases/manage-training-sessions.docx>), [operation contract for `inserisciDettagliSessione`](<05-operation-contracts/manage-training-sessions.docx>), and [activity diagram](<06-activity-diagrams/manage-training-sessions.jpg>).

### Assign the Captain Role

The manager selects an activity and appoints a captain from the athletes on their team who have registered for that activity, based on the best results achieved during the current year. Ties and the absence of results are also addressed. The appointment must ensure that each team has a single captain per activity, replacing any previous assignment.

References: [detailed use case](<03-use-cases/assign-team-captain.docx>), [operation contract for `assegnaRuoloCapitano`](<05-operation-contracts/assign-team-captain.docx>), and [sequence diagram](<07-design-sequence-diagrams/assign-team-captain.jpg>).

## Deliverable Structure

File and folder numbering follows the sections of the report:

1. **[Use case diagram](<01-use-case-diagram/use-case-diagram.jpg>):** actors, features, and system boundary.
2. **[Domain model](<02-domain-model/domain-model.jpg>):** domain concepts, attributes, and associations, including sporting competitions, teams, athletes, training sessions, reservations, and discount policies.
3. **[Detailed use cases](<03-use-cases/>):** main scenarios, alternatives, preconditions, and special requirements for seat requests, training sessions, and captain assignment. The discount use case is included in the complete report.
4. **[SSD — system sequence diagrams](<04-system-sequence-diagrams/>):** interactions between actors and the system treated as a single entity, for all four detailed use cases.
5. **[Operation contracts](<05-operation-contracts/>):** preconditions and postconditions for the selected operations. The discount operation contract is included in the complete report.
6. **[Activity diagrams](<06-activity-diagrams/>):** workflows for training session management and ticket reservations. An additional [PNG export](<06-activity-diagrams/ticket-reservation.png>) is also available.
7. **[SD — design sequence diagrams](<07-design-sequence-diagrams/>):** object collaboration and responsibility assignment. The folder also includes a document on GRASP patterns for seat requests.
8. **[Preliminary class diagram](<08-class-diagram/preliminary-class-diagram.jpg>):** design classes, operations, and relationships. The [captain assignment GRASP patterns document](<08-class-diagram/assign-team-captain-grasp-patterns.docx>) provides further details on the patterns used for captain assignment.
9. **[State machines](<09-state-machines/>):** lifecycles of a training session and a ticket reservation, including images and editable projects.

The **[complete report in PDF](<report/milano-cortina-2026-software-analysis-and-design.pdf>)** brings together all nine sections, including descriptions and design rationale not available as separate documents. The [editable Word version](<report/milano-cortina-2026-software-analysis-and-design.docx>) is also available.

Editable Visual Paradigm projects are stored alongside the related diagrams, including the [captain assignment system sequence diagram](<04-system-sequence-diagrams/assign-team-captain.vpp>) and the [sequence and class diagrams project](<08-class-diagram/sequence-and-class-diagrams.vpp>).

## How to Explore the Project

For an initial reading:

1. Open the complete report to understand the context and follow its contents.
2. Review the use case diagram and domain model to identify the main actors and concepts.
3. Follow a use case through its detailed description, SSD, operation contract, and design sequence diagram.
4. Complete the reading with the activity diagrams, preliminary class diagram, and state machines.

The available file formats are:

- **`.pdf`:** the complete report, ready to read in a browser or PDF viewer.
- **`.docx`:** documents that can be opened with Microsoft Word-compatible software, such as Word or LibreOffice Writer.
- **`.jpg` and `.png`:** exported diagrams that can be opened with a standard image viewer; zoom in to read attributes, messages, and multiplicities.
- **`.vpp`:** projects editable with Visual Paradigm. The exported images allow the diagrams to be viewed without opening the original projects.

No compilation or running services are required to explore the deliverables.

## Design Decisions

The documents explain responsibility assignment using **GRASP** principles:

- **Controller:** dedicated controllers coordinate system operations.
- **Information Expert:** responsibilities are delegated to the objects that hold the necessary information.
- **Creator:** object creation is assigned to classes that aggregate those objects or manage their lifecycle.
- **Low Coupling and High Cohesion:** dependencies are limited and responsibilities are kept focused.
- **Pure Fabrication:** supporting components, such as controllers and catalogs, are introduced to separate responsibilities.

The report also discusses the **Singleton** pattern for the discount catalog and the **State** pattern for managing participant roles. The preliminary class diagram also represents discount strategies, observable subjects for notifications, and state objects for tickets and training sessions. These elements describe the decisions made across the design deliverables; they do not correspond to implemented software components in this repository.

## Open Questions

The use case descriptions identify several decisions to clarify before a future implementation:

- which discount policy to apply to purchases already in progress when a percentage changes;
- whether to temporarily hold seats for notified visitors or allow purchases on a first-come, first-served basis;
- how to handle a reduction in training session capacity below the number of athletes already registered;
- how to handle the captain's appointment if the appointed athlete cancels their registration or leaves the team.

## Authors

- Nicolò Crespi
- Fabio Bellezza
- Dennis Ismajli
- Nicholas Afelli

Individual contributions are credited in the complete report.
