# IRManager

**Power Apps • Power Automate • SQL Server • SharePoint • Excel Online • SSRS**

<img width="1092" height="739" alt="image" src="https://github.com/user-attachments/assets/41732639-5818-49e5-a24b-691ebf3cfe90" />

## Overview

IRManager is a production business application I architected and developed to provide a scalable, centralized approach to managing customer and item restrictions.

Built with Power Apps, Power Automate, SQL Server, SharePoint, Excel Online, and SSRS, the solution supports individual and bulk restriction management, validation, mass imports, auditing, reporting, and integration with an existing SalesPad order-enforcement process.

The application was designed around scalability, data integrity, maintainability, and efficient administration of restriction datasets numbering in the thousands.

---

## Business Context

The organization uses Microsoft Dynamics GP as its ERP platform, with SalesPad providing additional operational functionality and user-facing workflows around GP data.

One of these capabilities is item restriction management, which controls whether specific products can be sold to particular customers based on defined business restrictions.

IRManager provides the administration and data-management layer for these restrictions. Restriction enforcement remains integrated with the existing sales-order workflow through a SalesPad customization that checks the centralized restriction data when a sales document is loaded.

---

## The Problem

Item restrictions were originally managed through native SalesPad functionality that evaluated restrictions through a series of OR-based conditions.

While workable at smaller volumes, this approach did not scale effectively as the number of customer and item restrictions grew into the thousands. Evaluating increasingly large sets of restriction conditions introduced significant performance issues and made continued expansion of the existing process impractical.

The challenge was therefore broader than improving the user interface. The organization needed a more scalable way to maintain a growing restriction dataset while preserving enforcement of the underlying business rules during sales-order processing.

IRManager moved restriction administration into a purpose-built application backed by centralized SQL Server data and processing, supporting individual and bulk operations, validation, auditing, and import result handling at scale.

---

## Solution

IRManager provides a centralized application experience for restriction management, including:

- Creation and maintenance of individual restriction records
- Search, sorting, cascading filters, and pagination
- Multi-record selection and bulk editing
- Excel-based mass imports
- Validation and detailed import result handling
- Enable, disable, and delete operations
- Customer and item selection workflows
- Audit and deletion history
- Reporting and export capabilities
- Automated temporary-file cleanup
- Centralized restriction data used by the existing SalesPad enforcement process

---

## Architecture

IRManager uses a hybrid architecture that separates application data retrieval from command and processing workflows while supporting downstream restriction enforcement within the existing sales-order process.

```text
                           IRManager
                     Power Apps Canvas App
                              │
                ┌─────────────┴─────────────┐
                │                           │
                │ Reads                     │ Commands / Processing
                ▼                           ▼
           SQL Server                 Power Automate
           Data Views                       │
                ▲                   ┌───────┴────────┐
                │                   │                │
                │               SharePoint      Excel Online
                │                   │                │
                │                   └───────┬────────┘
                │                           │
                │                           ▼
                └──────────────────── SQL Server
                                     Processing
                                     Validation
                                     Persistence
                                          │
                       ┌──────────────────┴─────────────────┐
                       │                                    │
                       ▼                                    ▼
                Power Automate                    SalesPad Customization
                       │                          Sales Document OnLoad
                       ▼                                    │
                  Power Apps                               ▼
                                                Restriction Enforcement
                                                During Order Workflow
```

Power Apps provides the primary IRManager user experience and reads application datasets directly from purpose-built SQL Server views. Data-changing operations and more complex processing invoke task-specific Power Automate workflows, with SQL Server providing validation, processing, and persistence.

File-based workflows additionally use SharePoint and Excel Online for temporary staging and workbook processing. Results are returned through Power Automate to the Canvas App for user feedback.

The centralized restriction data also supports downstream sales-order enforcement through an existing SalesPad customization that checks applicable customer and item restrictions when a sales document is loaded.

The SalesPad enforcement customization predates IRManager and was authored by another developer. It is included in the diagram to show how IRManager integrates with the broader sales-order architecture.

SSRS supports additional operational reporting.

---

## Key Features

### Restriction Management

Users can navigate large restriction datasets using search, sorting, cascading filters, and pagination.

The application supports individual and multi-record selection, inline updates, and bulk operations to reduce repetitive maintenance work.

<img width="975" height="698" alt="image" src="https://github.com/user-attachments/assets/0fc402e0-790b-4be0-a117-1493d715b4dc" />

<img width="1377" height="808" alt="image" src="https://github.com/user-attachments/assets/beaca5d1-138c-4858-8700-e6eba3dd9b82" />


---

### Mass Import and Validation

IRManager supports Excel-based mass imports for higher-volume restriction maintenance.

Submitted workbooks are temporarily staged in SharePoint and processed through an automated workflow using Excel Online before submitted data is passed to the SQL processing layer.

Processing results distinguish between successful, skipped, and rejected records, providing clear feedback and allowing problematic records to be exported for review and correction.

<img width="975" height="635" alt="image" src="https://github.com/user-attachments/assets/5004a958-1ae4-48c0-9090-eeb9e102a2f6" />

<img width="975" height="467" alt="image" src="https://github.com/user-attachments/assets/272b96c3-7abd-40f0-83bd-3ae0b486073d" />



---

### Bulk Editing

Users can select multiple restriction records and apply supported changes through a dedicated bulk-edit workflow.

This allows repetitive updates to be performed as a controlled operation rather than requiring users to modify each record individually.


<img width="1329" height="756" alt="image" src="https://github.com/user-attachments/assets/8e2a2530-01a2-408e-a0f4-617e90a16da5" />


---

### Guided Data Entry

Individual record creation uses contextual customer and item selection workflows to guide users through the required information.

Input behavior and validation help prevent invalid submissions and provide feedback before data is processed.

<img width="588" height="393" alt="image" src="https://github.com/user-attachments/assets/83673836-8e4e-4af3-be3f-851667276785" />

---

### Auditability

The solution maintains historical information supporting operational auditing and troubleshooting.

A dedicated deletion-history experience provides read-only visibility into removed restriction records while separating historical information from active restriction management.

<img width="1056" height="591" alt="image" src="https://github.com/user-attachments/assets/a27488d6-af74-4ec6-b48e-64b45d219016" />


---

### Reporting and Export

IRManager includes reporting and export capabilities for operational review and downstream analysis.

Automated workflows support the creation and temporary staging of output files, while scheduled cleanup prevents temporary application files from accumulating indefinitely.


---

## Automation

IRManager uses multiple task-specific Power Automate workflows rather than concentrating application processing into a single automation.

Key workflows support:

- Mass restriction import processing
- Import issue export generation
- Record enable, disable, and delete actions
- Individual record insertion
- Scheduled temporary-file cleanup

The Canvas App invokes the appropriate workflow based on the requested operation. Record actions and individual inserts are orchestrated between Power Apps, Power Automate, and SQL Server, while file-based workflows additionally use SharePoint and Excel Online for staging and workbook processing.

For mass imports, submitted Excel workbooks are temporarily staged in SharePoint and accessed through Excel Online before the data is passed to SQL Server for validation and processing. Results are returned through Power Automate to the Canvas App, where users receive a breakdown of successful, skipped, and rejected records. Processing issues can be exported for review and correction.

Scheduled automation removes temporary application files after processing to prevent unnecessary accumulation.

This separation keeps individual workflows focused, allows processes to be maintained independently, and keeps data-intensive validation and processing in SQL Server rather than the application layer.

---

## Data Processing

SQL Server serves as the primary persistence, validation, and processing layer behind IRManager.

The backend design includes:

- Primary restriction data
- Audit data
- Application-specific views for user-facing datasets
- Stored procedures for mass processing
- Stored procedures for individual record operations
- Stored procedures for supported record actions

Power Apps reads application datasets through purpose-built SQL views, while data-changing and data-intensive operations are handled through Power Automate and backend SQL processing.

This separation keeps processing and persistence in SQL Server while Power Apps remains focused primarily on application interaction and user experience.

---

## Delivery Lifecycle

IRManager has been developed and maintained as a production solution using separate development/testing and production environments. Changes are developed and validated outside production before solution packages are promoted to the production environment.

My involvement has spanned requirements translation, solution architecture, application and workflow development, SQL integration, testing, UAT support, environment promotion, production deployment, documentation, troubleshooting, and ongoing enhancement.

Production releases include validation of application connections, automation dependencies, data connections, workflow availability, and application functionality before release to end users.

---
## My Role

I architected and developed IRManager and continue to own its support and enhancement following production deployment. My responsibilities have spanned solution architecture, Power Apps development, Power Automate workflow design, SQL integration and backend processing, testing, deployment, documentation, and production support.

The SalesPad sales-document enforcement script predates IRManager and was developed by another developer. My work integrates IRManager with that existing enforcement mechanism rather than claiming authorship of the SalesPad customization.

---

## Technologies

| Technology | Role |
| --- | --- |
| **Power Apps** | Canvas App user interface, direct data retrieval, and application interactions |
| **Power Automate** | Workflow orchestration, imports, exports, record actions, and scheduled processing |
| **SQL Server** | Persistence, application views, validation, auditing, backend processing, and centralized restriction data |
| **SharePoint** | File staging, templates, and temporary import/export storage |
| **Excel Online** | Workbook-based mass import processing |
| **SSRS** | Operational reporting |
| **Dynamics GP / SalesPad** | ERP and operational environment integrated with the restriction-management process |

---

## Confidentiality

IRManager was developed for a professional production environment.

This case study intentionally excludes proprietary source code, confidential business information, production data, credentials, internal infrastructure details, organization-specific object names, and other sensitive implementation information.

Screenshots included in this portfolio have been sanitized for public presentation.
