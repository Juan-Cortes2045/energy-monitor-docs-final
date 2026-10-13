# EnergyMonitor Project Charter

Juan Esteban Cortés Parra and Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Technologist in Software Analysis and Development (ADSO), group (ficha) 3145556

October 16, 2026

## Abstract

This document is the project charter of EnergyMonitor, a solution that lets households see the electrical consumption of each appliance, identify the periods of highest demand, receive alerts and get saving recommendations. It defines why the project is being done and the value it provides, its objectives with measurable indicators, the scope included and excluded, the budget, the phases, the milestones and the schedule until the delivery date, October 16, 2026, and the work team with its roles. The solution integrates a measuring device that attaches to each appliance, a web application and an Android mobile application. At the time of writing, the project is in its final phase, devoted to consolidation, testing and delivery. The detail of the functions is developed in the requirements specification.

*Keywords:* project charter, electrical consumption, energy saving, households, alerts

## Introduction and Justification

### Project Context

EnergyMonitor lets households see the electrical consumption of each appliance, identify the periods of highest demand, receive alerts and get recommendations to save energy. Measurement is done by a purpose-built device that attaches to the appliance and sends real readings of its consumption. Users consult the data from a web application and an Android mobile application.

The project is a formative project of the Technologist in Software Analysis and Development (ADSO) program at SENA, developed by two trainees and due on October 16, 2026.

### Problem Addressed

High energy consumption in Colombian households is largely caused by a lack of energy awareness. People do not know which appliances consume the most or how their consumption varies throughout the day, so they cannot correct habits such as leaving a device on unnecessarily or keeping an appliance that consumes more than it should. The result is high bills and a greater environmental impact.

### Justification

The project starts from the premise that most households have no tools that show their consumption in real time or identify which appliances weigh most on the bill. A system that measures the consumption of each appliance, warns when reasonable limits are exceeded and proposes saving actions based on measured data supports informed decisions and encourages responsible and sustainable use of electricity. Measuring with a real sensor, rather than estimating, is what allows alerts and recommendations to be based on the actual behavior of each household.

### Expected Benefits

- For households: knowing which appliances use the energy, reacting in time to excessive consumption and making saving decisions with their own data, which helps control the cost of the bill.
- For the environment: reducing unnecessary electricity consumption and, with it, its environmental impact.
- For the training program: a complete and demonstrable system, from measurement on a real appliance to the notice on the user's phone.

## Project Objectives

### General Objective

Develop an energy resource control system that monitors household electrical consumption in real time and identifies the appliances that generate the highest expense, in order to promote efficient and responsible use of electricity.

### Specific Objectives

Each specific objective has a measurable indicator and an achievable target within the project timeframe. Table 1 presents them together with their coverage as of October 12, 2026.

**Table 1**

*Specific Objectives, Indicators and Coverage*

| Code | Objective | Indicator and target | Coverage as of October 12, 2026 |
|---|---|---|---|
| OBJ-01 | Measure the electrical consumption of each appliance in the home | One reading every 60 seconds of voltage, current, active power and accumulated energy for each connected measuring device | Covered. Measuring device operational |
| OBJ-02 | Show consumption clearly in a web and a mobile application | Consumption summary and history per home on the web, and consultation of homes, alerts and recommendations in the Android application, in Spanish, English, French and Portuguese | Web application in final adjustments. Mobile application under development |
| OBJ-03 | Issue automatic alerts for unusual or excessive consumption | Four alert types (device linked, disconnection, high or critical consumption, daily or monthly limit). A disconnection is detected in at most 150 seconds | Covered |
| OBJ-04 | Encourage responsible habits through recommendations based on measured data | Five saving rules evaluated every hour. The same recommendation is not repeated within 7 days | Covered |
| OBJ-05 | Deliver the system working end to end | Live demonstration, from a physical device measurement to an alert or recommendation on a phone, with no manual intervention, on October 16, 2026 | Pending |

## General Scope

### In Scope

Table 2 lists what the project builds and the system module responsible for each item.

**Table 2**

*Items in Scope*

| Item | Description | Module or component |
|---|---|---|
| Users and profiles | Registration, sign-in, email verification, password recovery, Google sign-in, profile and account deletion | User and Profile Management Module |
| Homes | Homes, members, owner and member roles, favorites and consumption limits | Home Management Module |
| Devices | Registration, linking, editing and unlinking of measuring devices | Device Management Module |
| Measurements | Reception of readings, device status, consumption history and statistics | Measurements and Consumption Module |
| Alerts | Consumption alerts and device connection alerts | Alerts Module |
| Recommendations | Saving suggestions based on measured consumption | Savings Recommendations Module |
| Notifications | Notices by email and in the browser, preferences and delivery log | Notifications Module |
| Web application | Full client for management, history and statistics | Web application |
| Mobile application | Android client for consultation and alerts | Mobile application |
| Measuring device | Physical device that measures the consumption of each appliance and sends the readings | Measuring device |

### Out of Scope

Table 3 explicitly lists what the project does not build and the reason.

**Table 3**

*Items Out of Scope*

| Item | Reason |
|---|---|
| iPhone application (iOS) | The mobile application is developed for Android only. |
| Manual entry of consumption from the utility bill | The measuring device records consumption automatically, so manual capture is not provided. |
| Administration screen for general permissions | Those functions are operated directly by the technical team. |
| Cost estimation in local currency | There is no agreement with an energy provider that supplies tariffs. The system reports energy in kWh. |
| Remote update of the measuring device's software | The device is updated by connecting it with a cable. |
| Service for several organizations at once | Each installation serves a single organization. |

### Assumptions and Constraints

Table 4 gathers the assumptions and constraints that condition the project.

**Table 4**

*Project Assumptions and Constraints*

| Type | Description |
|---|---|
| Time | Delivery on October 16, 2026, per the ADSO program calendar for group 3145556. |
| Budget | Approximately COP 180,000 invested in the components of the measuring device. No budget for paid hosting or services. |
| Technology | The tools defined for the formative project are used, detailed in the technical proposal. |
| Team | Two trainees of the ADSO program. |
| Regulation | Protection of user information, with reference to Law 1581 of 2012 (Congreso de la República de Colombia, 2012). |
| Connectivity | Continuous Wi-Fi is assumed where measuring devices are installed. |

### Budget

Table 5 summarizes the project budget.

**Table 5**

*Project Budget*

| Item | Amount |
|---|---|
| Measuring device components | Approximately COP 180,000 |
| Hosting and paid services | Not contemplated |
| Total project cost | `[TO BE CONFIRMED]`, consolidated in the technical and cost report |

## Schedule and Milestones

### Main Phases

The project started on May 1, 2026 and was planned in four phases with one-week progress steps. Table 6 describes each phase with its period, goal and exit criterion.

**Table 6**

*Project Phases*

| Phase | Period | Goal | Exit criterion |
|---|---|---|---|
| 1. Foundation and core loop | May 1 to June 30 | A user registers, creates or joins a home, registers a device and sees its measurements arrive. | A registered user logs in, creates a home, links a device and sees at least one real measurement recorded. |
| 2. Consumption visibility and alerts | July 1 to August 31 | Consumption is visible and the system warns when something is abnormal. | A home exceeding its limit produces a visible alert on web and mobile and a notification is delivered. |
| 3. Recommendations and internationalization | September 1 to 30 | Close the product loop with proactive guidance and finish the cross-cutting interface work. | A home with one week of history receives at least one recommendation based on its own consumption, and the interface is verified in more than one language. |
| 4. Consolidation and delivery | October 1 to 16 | Leave the system ready for demonstration and defense. | The system is demonstrated live, from a physical device measurement to an alert or recommendation on a phone. |

**Status as of October 12, 2026:** the project is in phase 4. The functions of phases 1 to 3 are built.

### Milestones

Table 7 gathers the project milestones with their estimated date.

**Table 7**

*Project Milestones*

| Code | Milestone | Estimated date | Status as of October 12, 2026 |
|---|---|---|---|
| H-01 | Project start | May 1, 2026 | Done |
| H-02 | End of phase 1: a registered user links a device and sees a real measurement | June 30, 2026 | Done |
| H-03 | End of phase 2: an alert for exceeding a limit, visible on web and mobile with its notification | August 31, 2026 | Done |
| H-04 | End of phase 3: recommendation based on the home's own consumption and interface in more than one language | September 30, 2026 | Done |
| H-05 | Complete bilingual documentation (project charter and requirements specification) | October 12, 2026 | In progress |
| H-06 | End-to-end tests and user, technical and installation manuals | October 14, 2026 | Pending |
| H-07 | Technical and cost report, and rehearsal of the demonstration | October 15, 2026 | Pending |
| H-08 | Final delivery and live demonstration | October 16, 2026 | Pending |

### Final Phase Plan

Table 8 details the activities of the period between October 10 and 16, 2026, which corresponds to phase 4.

**Table 8**

*Phase 4 Activity Plan*

| Dates | Activities | Deliverable |
|---|---|---|
| October 10 to 12 | Confirm and record the pending changes. Complete the project charter and the requirements specification (ES and EN) | Bilingual documents in APA 7 format |
| October 13 and 14 | End-to-end tests. User manuals (web and mobile), technical and installation manual | Test plan and evidence. Manuals |
| October 15 | Technical and cost report. Rehearsal of the live demonstration | Technical and cost report |
| October 16 | Final delivery | Working system and complete documentation |

Two groups of items have no date within this plan and are carried out only if time allows: security improvements to the devices' credentials and reliability improvements in the sending of readings. The pending items of home and device management, which are home ownership transfer, home deletion and device history management by linking period, also have no assigned date.

## Work Team and Roles

Table 9 presents who does what in the project. The team is made up of two trainees of the ADSO program.

**Table 9**

*Work Team and Roles*

| Member | Role | Responsibilities |
|---|---|---|
| Juan Esteban Cortés Parra | Project lead (Project Manager), Scrum Master, system data manager and developer | Project planning and follow-up, facilitation of the methodology, design and administration of the system data, information analysis, design and programming |
| Miguel Ángel García Artunduaga | Analyst, QA and developer | Quality assurance and testing, information analysis, design and programming |

## References

Congreso de la República de Colombia. (2012, October 17). *Ley 1581 de 2012: Por la cual se dictan disposiciones generales para la protección de datos personales* [Law 1581 of 2012: By which general provisions for the protection of personal data are issued]. Diario Oficial No. 48,587.
