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



### Submission Tracking

Submitted requests could be tracked through the workflow, with validation and processing status surfaced back to users.



---

## Architecture

```text
                    Warehouse User
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

As the project evolved, I explored a broader architecture that treated the submitted workbook as one component of a larger operational workflow.

The expanded design considered:

- Additional workflow states and exception handling
- Microsoft Teams notifications
- SQL Server processing and integration
- More structured backend validation
- Improved operational tracking
- Automated user communication

Conceptually, Power Automate would serve as the orchestration layer between the submission experience and supporting enterprise services.

```text
                  SharePoint / Excel
                         │
                         ▼
                   Power Automate
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
         SharePoint     Teams    SQL Server
             │           │           │
             └───────────┼───────────┘
                         ▼
                 Status / Feedback
```

The project was ultimately shelved before this expanded architecture was completed.

---

## Outcome

Although Warehouse Move Ops did not progress into a full production application, the initiative produced real requirements, working automation, validation patterns, workflow concepts, and architectural groundwork.

The project also highlighted the limitations of continuing to expand a SharePoint and workflow-based interface as business requirements became more application-like.

If the process is revisited, a dedicated .NET/Blazor application could provide a stronger foundation for structured data entry, workflow state management, dashboards, validation, and backend integrations while retaining Power Automate where Microsoft 365 automation remains appropriate.

---

## My Role

My involvement included:

- Stakeholder requirements gathering
- Existing-process analysis
- Solution and workflow design
- SharePoint implementation
- Power Automate development
- Excel submission-process design
- Validation and workflow-status design
- Testing and troubleshooting
- Expanded architecture exploration

The project provided hands-on experience taking a business process from stakeholder discovery through working low-code implementation while also evaluating when future requirements may justify moving toward a dedicated application architecture.

---

## Technologies

| Technology | Role |
| --- | --- |
| **Power Automate** | Workflow orchestration, validation, and status processing |
| **SharePoint** | Submission interface, storage, and request visibility |
| **Excel** | Standardized warehouse move submission template |
| **Microsoft Teams** | Explored for workflow notifications |
| **SQL Server** | Explored for expanded backend processing |
| **.NET / Blazor** | Potential future modernization path |

---

## Project Status

**Prototype / architectural exploration**

The initial Power Platform implementation and requirements work were completed, while the broader modernization effort was shelved for potential future re-exploration.

---

## Confidentiality

This case study excludes proprietary source code, confidential business information, production data, credentials, and internal infrastructure details.

Screenshots have been sanitized for public presentation.
