# Warehouse Move Ops

**Power Automate • SharePoint • Excel • Microsoft Teams • SQL Server**

## Overview

Warehouse Move Ops was a business process modernization initiative designed to improve how warehouse move requests were submitted, validated, tracked, and communicated.

The project began with stakeholder requirements gathering in 2025 and resulted in an initial Power Platform implementation using SharePoint, Power Automate, and a standardized Excel submission process.

A broader second iteration was later explored to support additional workflow orchestration, backend processing, and operational communication. Further development was ultimately shelved, with the groundwork retained for potential future modernization as a dedicated application.

---

## The Problem

The warehouse move process relied heavily on Excel files and manual coordination between users and operational stakeholders.

As requirements were gathered, several opportunities for improvement were identified:

- Standardized submissions
- Required-field validation
- Centralized request visibility
- Workflow status tracking
- User feedback for incomplete submissions
- Historical submission visibility
- Automated communication and processing

The goal was to introduce structure and automation around the existing process without immediately replacing familiar tools such as Excel.

---

## V1: Power Platform Prototype

The initial solution used SharePoint as the centralized user experience and Power Automate to coordinate submitted Excel workbooks.

Users could access the standardized move template, submit completed requests, review previous submissions, and monitor workflow status from a centralized SharePoint interface.

Power Automate handled submission detection, validation, status updates, and user feedback.

### SharePoint Operations Hub

The SharePoint experience centralized template access, move submissions, historical requests, and workflow status.

<img width="1056" height="398" alt="image" src="https://github.com/user-attachments/assets/36129a28-a475-4ba0-b33e-220600923198" />


### Submission Tracking

Submitted requests could be tracked through the workflow, with validation and processing status surfaced back to users.

<img width="1048" height="558" alt="image" src="https://github.com/user-attachments/assets/ae0ff0d0-9fa7-4c91-844c-7d9a3a8960c1" />

<img width="1064" height="367" alt="image" src="https://github.com/user-attachments/assets/c8784df2-2a1d-4a14-8e0e-6196216b987d" />

---

## Architecture

```text
                    Sales Admin
                          │
                          ▼
                   Excel Template
                          │
                          ▼
                     SharePoint
                          │
                          ▼
                   Power Automate
                          │
                 ┌────────┴────────┐
                 │                 │
                 ▼                 ▼
            Validation        Status / Feedback
                 │                 │
                 └────────┬────────┘
                          │
                          ▼
                         User
```

SharePoint provided the user-facing submission and tracking experience, while Power Automate handled workflow orchestration and validation.

This allowed the existing Excel-based process to remain familiar to users while introducing a controlled workflow around it.

---

## V2 Exploration

As the project evolved, I explored a broader architecture that treated the submitted workbook as the entry point into a larger operational workflow.

The proposed process began with a Sales Admin submitting an approved macro-enabled Excel template through the controlled SharePoint environment. Submission metadata, such as the targeted move date, could be captured as part of the intake process before Power Automate validated and processed the workbook.

The template itself identified the type of warehouse move request being submitted. This selection could then drive different workflow branches, allowing validation, processing, and exception handling to vary based on the operational scenario.

The expanded design considered:

* Controlled SharePoint intake using an approved XLSM template
* Required submission metadata and workbook validation
* Request-type-driven workflow branching
* SQL Server validation and backend processing
* Interactive Microsoft Teams Adaptive Cards for Sales Admin review
* Exception handling and operational revalidation
* Branch Manager coordination when additional confirmation was required
* Automated Outlook communication summarizing issues requiring follow-up
* Centralized workflow execution through a dedicated service account
* Workflow status tracking and user feedback

Rather than using Teams only for notifications, the proposed design used Adaptive Cards as a human-in-the-loop interaction point. Validation issues could be presented to the Sales Admin for review, allowing the workflow to continue or move into a revalidation path based on their response.

When additional branch-level confirmation was required, the workflow could support further coordination by generating a summarized communication containing the issues requiring review before processing continued.

```text
                    Sales Admin
                         │
                         ▼
                Approved XLSM Template
                         │
                         ▼
                SharePoint Submission
                  + Required Metadata
                         │
                         ▼
                   Power Automate
                  (Service Account)
                         │
                         ▼
                Workbook Validation
                         │
                         ▼
                  Request Type
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
           Workflow   Workflow   Workflow
            Path A     Path B     Path C
              │          │          │
              └──────────┼──────────┘
                         ▼
               Operational Validation
                         │
                  ┌──────┴──────┐
                  ▼             ▼
               Continue      Exception
                                │
                                ▼
                       Teams Adaptive Card
                           Sales Admin
                                │
                         ┌──────┴──────┐
                         ▼             ▼
                      Continue    Re-validation
                                      │
                                      ▼
                              Outlook Summary
                                      │
                                      ▼
                               Branch Manager
                                      │
                                      ▼
                              Status / Feedback
```

Power Automate would serve as the orchestration layer connecting the controlled submission process, backend validation, and human decision points. A dedicated service account was considered for shared workflow ownership and execution, reducing dependency on individual user accounts while supporting the available licensing environment.

The project was ultimately shelved before this expanded architecture was completed.

---

## Outcome

Although Warehouse Move Ops did not progress into a full production application, the initiative produced real requirements, working automation, validation patterns, workflow concepts, and architectural groundwork.

The project also highlighted the limitations of continuing to expand a SharePoint and workflow-based interface as business requirements became more application-like.

If the process is revisited, a dedicated .NET/Blazor application could provide a stronger foundation for structured data entry, workflow state management, dashboards, validation, and backend integrations while retaining Power Automate where Microsoft 365 automation remains appropriate.

---

## My Role

My involvement included:

* Stakeholder requirements gathering
* Existing-process analysis
* Solution and workflow design
* SharePoint implementation
* Power Automate development
* Excel/XLSM submission-process design
* Validation and conditional workflow-routing design
* Human-in-the-loop workflow design using Teams Adaptive Cards
* Testing and troubleshooting
* Expanded architecture exploration

The project provided hands-on experience taking a business process from stakeholder discovery through working low-code implementation while also designing more advanced workflow orchestration and evaluating when future requirements may justify moving toward a dedicated application architecture.

---

## Technologies

| Technology          | Role                                                                            |
| ------------------- | ------------------------------------------------------------------------------- |
| **Power Automate**  | Workflow orchestration, validation, conditional routing, and status processing  |
| **SharePoint**      | Controlled submission interface, storage, metadata, and request visibility      |
| **Excel / XLSM**    | Standardized warehouse move template and structured workflow input              |
| **Microsoft Teams** | Adaptive Card-based Sales Admin review and workflow interaction explored for V2 |
| **Outlook**         | Branch-level exception and revalidation communication explored for V2           |
| **SQL Server**      | Expanded backend validation and processing explored for V2                      |
| **.NET / Blazor**   | Potential future modernization path                                             |

---

## Project Status

**Prototype / architectural exploration**

The initial Power Platform implementation and requirements work were completed, while the broader modernization effort was shelved for potential future re-exploration.

---

## Confidentiality

This case study excludes proprietary source code, confidential business information, production data, credentials, and internal infrastructure details.

Screenshots have been sanitized for public presentation.

