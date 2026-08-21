<img width="975" height="698" alt="image" src="https://github.com/user-attachments/assets/378ed966-d5d1-4b17-a354-409c32af5a9c" /># IRManager

**Power Apps • Power Automate • SQL Server • SharePoint • Excel Online • SSRS**

## Overview

IRManager is a production business application I architected and developed to provide a scalable, centralized approach to managing customer and item restrictions.

The solution provides users with a centralized interface for creating, reviewing, updating, importing, and auditing restriction records. It combines a Power Apps Canvas App with Power Automate workflows, SQL Server processing, SharePoint-based file handling, Excel Online, and reporting capabilities.

IRManager serves as the administration and data-management layer for a broader restriction workflow, with centralized restriction data also consumed by an existing SalesPad customization that enforces applicable restrictions during sales-order processing.

The application was designed around scalability, data integrity, guided validation, bulk-processing efficiency, auditability, and maintainability.

---

## Business Context

The organization uses Microsoft Dynamics GP as its ERP platform, with SalesPad providing additional operational functionality and user-facing workflows around GP data.

One of those capabilities was item restriction management, which controls whether specific products can be sold to particular customers based on defined business restrictions.

The native restriction approach encountered scalability and performance limitations as restriction volume increased. IRManager was developed as a purpose-built administration and data-management layer for this process.

Restriction enforcement remains integrated with the sales-order workflow through an existing SalesPad customization. When a sales document is loaded, the SalesPad script checks the centralized restriction data and applies the appropriate restrictions for customer and item combinations.

IRManager therefore focuses on scalable restriction administration, validation, bulk processing, and auditing while integrating with the existing order-entry enforcement mechanism.

---

## The Problem

Item restrictions were originally managed through native SalesPad functionality that evaluated restrictions through a series of OR-based conditions.

While workable at smaller volumes, this approach did not scale effectively as the number of customer and item restrictions grew into the thousands. Evaluating increasingly large sets of restriction conditions introduced significant performance issues and made continued expansion of the existing process impractical.

The challenge was therefore broader than improving the user interface. The organization needed a more scalable way to maintain a growing restriction dataset while preserving enforcement of the underlying business rules during sales-order processing.

IRManager was developed to move restriction administration into a purpose-built application backed by centralized SQL Server data and processing. The solution supports individual and bulk operations, validation, auditing, and import result handling while providing a more maintainable approach to managing restriction data at scale.

The centralized data can then be consumed by the existing SalesPad enforcement customization during the sales-order workflow.

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

Power Apps provides the primary IRManager user experience and reads application datasets from purpose-built SQL Server views.

For record actions, individual inserts, mass imports, exports, and other processing workflows, the Canvas App invokes task-specific Power Automate flows. Power Automate serves as the orchestration layer between the application and backend services, coordinating SQL Server processing and, for file-based workflows, SharePoint and Excel Online.

For mass imports specifically, submitted workbooks are temporarily staged in SharePoint and accessed through Excel Online before the submitted data is passed to SQL Server for validation and processing. Processing results are then returned through Power Automate to the Canvas App to provide feedback to the user.

The centralized restriction data also supports downstream enforcement within the existing SalesPad order-entry workflow. An existing SalesPad customization, triggered when a sales document is loaded, checks applicable customer and item restrictions against the centralized data and enforces those restrictions during sales-order processing.

The SalesPad enforcement customization was an existing component authored by another developer and is shown here to provide context for the complete solution architecture. My work on IRManager focused on the architecture and development of the application, automation, data-management, integration, deployment, and supporting processes surrounding that enforcement layer.

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

Processing results distinguish between successfully processed, skipped, and rejected records, giving users clear feedback when submitted data cannot be processed.

Problematic records can be exported for review and correction.

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

**Screenshot: Deletion History**

<!-- Add: images/deletion-history.png -->

---

### Reporting and Export

IRManager includes reporting and export capabilities for operational review and downstream analysis.

Automated workflows support the creation and temporary staging of output files, while scheduled cleanup prevents temporary application files from accumulating indefinitely.

**Optional Screenshot: Reporting / Export**

<!-- Add if desired: images/reporting.png -->

---

### Sales-Order Enforcement Integration

IRManager centralizes the restriction data used by the broader sales-order process.

An existing SalesPad customization checks the centralized restriction data when a sales document is loaded and uses the applicable customer and item combinations to enforce restrictions during order processing.

This enforcement component predates IRManager and was developed by another developer. IRManager integrates with this existing mechanism by providing a scalable administration and data-management layer for the restriction information it consumes.

---

## Automation

IRManager is supported by multiple Power Automate workflows with separate responsibilities:

- Mass restriction import processing
- Import issue export generation
- Record enable, disable, and delete actions
- Individual record insertion
- Scheduled temporary-file cleanup

The Canvas App invokes task-specific workflows when appropriate rather than concentrating all processing into a single automation.

Not every application operation requires the same workflow path. Record actions and individual inserts can be orchestrated directly between Power Apps, Power Automate, and SQL Server, while file-based workflows additionally use SharePoint and Excel Online for staging and workbook processing.

This separation keeps individual processes focused and allows workflows to be maintained and enhanced independently.

---

## Data Processing

SQL Server serves as the primary persistence and processing layer behind the application.

The backend design includes:

- Primary restriction data
- Audit data
- Application-specific views for user-facing datasets
- Stored procedures for mass processing
- Stored procedures for individual record operations
- Stored procedures for supported record actions

Power Apps reads application datasets through purpose-built SQL views, while data-changing operations and more complex processing are handled through task-specific Power Automate workflows and backend SQL processing.

The centralized restriction data also provides the shared data source used by the existing SalesPad enforcement process.

This design keeps data-intensive processing and backend operations in SQL Server while Power Apps remains focused primarily on interaction and user experience.

---

## File Processing

SharePoint and Excel Online support the application's workbook-based processing workflows.

For mass imports, submitted files are temporarily staged in SharePoint and processed through Power Automate. Excel Online is used to interact with workbook data before the workflow passes the submitted information into backend processing.

Separate export functionality allows processing issues to be returned in a user-friendly file format for review.

Temporary application files are removed through scheduled automation to prevent unnecessary accumulation.

---

## Delivery Lifecycle

IRManager has been developed and maintained as a production solution using separate development/testing and production environments.

My work has included:

- Solution architecture and technical design
- Requirements translation
- Power Apps development
- Power Automate workflow design and development
- SQL integration and backend processing
- SharePoint-based file handling
- Integration with the existing SalesPad restriction-enforcement process
- Validation and error-handling design
- Testing and troubleshooting
- User acceptance testing support
- Solution packaging and environment promotion
- Production deployment and validation
- Technical documentation
- Ongoing application support and enhancement

Changes are developed and tested outside of production before solution packages are promoted to the production environment.

Deployment includes validation of application connections, automation dependencies, data connections, workflow availability, and application functionality before release to end users.

---

## My Role

I architected and developed IRManager as an end-to-end business application solution and have continued to own its support and enhancement following deployment.

My responsibilities span the application lifecycle, including solution architecture, requirements translation, user experience design, Power Automate workflow design and implementation, SQL integration and backend processing, validation and bulk-processing design, integration with the existing SalesPad enforcement process, testing, environment promotion, production deployment, technical documentation, troubleshooting, and ongoing enhancement.

The SalesPad sales-document enforcement script was developed by another developer and was an existing component of the surrounding architecture. My work integrates IRManager's centralized restriction-management solution with that enforcement process rather than claiming authorship of the SalesPad customization itself.

The project has provided hands-on experience architecting, developing, deploying, and maintaining a production Power Platform solution spanning application development, workflow automation, data processing, systems integration, reporting, deployment, and operational support.

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
