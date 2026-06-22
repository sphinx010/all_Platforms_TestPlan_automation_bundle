# RegComply Document & Task Management — Functional and Agentic QA Specification

- **Source:** ComplianceOS — Document & Task Management PRD, v1.0 Draft, last updated June 2026
- **Product Type:** SaaS compliance evidence, document, obligation, and task management module
- **Specification Status:** Draft — derived from a pre-build/design-phase PRD; no browser or network verification has been performed
- **Scope Note:** Covers Document Management, Task Management, document-to-framework mapping, review/approval/acknowledgement, native editing, version control, AI document intelligence, notifications, risk escalation, and the Google Workspace, identity-provider, RegWatch, email, Risk Register, and Audit Firm access dependencies described in the PRD. Broader RegComply modules are excluded except where this PRD explicitly invokes them as an integration or upstream data source.
- **Environment Note:** Environment access is partial. RegTech Admin staging and production URLs are supplied. QA admin routes for Standards and Audit Firms and a shared QA client portal are supplied. Dedicated Document Management and Task Management route URLs are not supplied. Credentials are available for RegTech Admin, Organisation Admin, Audit Firm, and Client accounts, but the PRD roles of Compliance Manager, Analyst, Internal Auditor, and Department Lead are not explicitly mapped to those accounts.
- **Automation Test Basis:** Automation candidates are derived from PRD-confirmed functional rules, role permissions, business-state transitions, known regression risks, and integration contracts. Implementation priority is then filtered by repetition, determinism, business relevance, execution safety, and environment suitability.
- **Environment Execution Policy:** QA is the primary environment for automated regression, bug-fix validation, API and integration testing, controlled stress testing, negative testing, and manual exploratory testing. Production is restricted to monitoring, authorized smoke and sanity checks, rollback-support validation, and review of customer-reported issues. The full QA regression or stress suite must not run against Production.

---

## 1. Platform Access and Roles

### QA Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Staging RegTech Admin | RegTech Admin | `https://regtech365-admin-client-staging.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `jossyojih@gmail.com` | `Okpasbaba99$` | `REGCOMPLY_STAGING_ADMIN_URL`, `REGCOMPLY_STAGING_REGTECH_ADMIN_EMAIL`, `REGCOMPLY_STAGING_REGTECH_ADMIN_PASSWORD` |
| QA RegTech Admin — Standards | RegTech Admin | `https://regtech365-admin-client-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/standards` | `jossyojih@gmail.com` | `Okpasbaba99$` | `REGCOMPLY_QA_ADMIN_STANDARDS_URL`, `REGCOMPLY_QA_REGTECH_ADMIN_EMAIL`, `REGCOMPLY_QA_REGTECH_ADMIN_PASSWORD` |
| QA RegTech Admin — Audit Firms | RegTech Admin | `https://regtech365-admin-client-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/audit-firms` | `jossyojih@gmail.com` | `Okpasbaba99$` | `REGCOMPLY_QA_ADMIN_AUDIT_FIRMS_URL`, `REGCOMPLY_QA_REGTECH_ADMIN_EMAIL`, `REGCOMPLY_QA_REGTECH_ADMIN_PASSWORD` |
| QA Organisation Client Portal | Organisation Admin (Client) | `https://regtech365global-client-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `qatester@opexconsult.co.uk` | `Econome@2026` | `REGCOMPLY_QA_CLIENT_URL`, `REGCOMPLY_QA_ORG_ADMIN_EMAIL`, `REGCOMPLY_QA_ORG_ADMIN_PASSWORD` |
| QA Audit Firm Portal | Audit Firm | `https://regtech365global-client-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `auditquest@mailinator.com` | `Audit@2025` | `REGCOMPLY_QA_CLIENT_URL`, `REGCOMPLY_QA_AUDIT_FIRM_EMAIL`, `REGCOMPLY_QA_AUDIT_FIRM_PASSWORD` |
| QA Client Portal | Client | `https://regtech365global-client-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `fundverse@mailinator.com` | `Fund@verse2026` | `REGCOMPLY_QA_CLIENT_URL`, `REGCOMPLY_QA_CLIENT_EMAIL`, `REGCOMPLY_QA_CLIENT_PASSWORD` |
| QA Document Management Route | Document-role account | `<ENTER_DOCUMENT_MANAGEMENT_URL>` | `<ENTER_DOCUMENT_ROLE_EMAIL>` | `<ENTER_DOCUMENT_ROLE_PASSWORD>` | `REGCOMPLY_QA_DOCUMENT_URL`, `REGCOMPLY_QA_DOCUMENT_USER_EMAIL`, `REGCOMPLY_QA_DOCUMENT_USER_PASSWORD` |
| QA Task Management Route | Task-role account | `<ENTER_TASK_MANAGEMENT_URL>` | `<ENTER_TASK_ROLE_EMAIL>` | `<ENTER_TASK_ROLE_PASSWORD>` | `REGCOMPLY_QA_TASK_URL`, `REGCOMPLY_QA_TASK_USER_EMAIL`, `REGCOMPLY_QA_TASK_USER_PASSWORD` |

**QA execution policy:** This is the primary test environment. Run automated smoke, critical regression, RBAC, bug-regression, API, integration, negative, and controlled stress suites here. Use manual testing for exploratory investigation, usability, new or unstable features, subjective AI-output assessment, and high-risk behaviours that are not yet deterministic enough for reliable automation. Every confirmed defect should be documented and, where repeatable, converted into a permanent regression test.

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Production RegTech Admin | RegTech Admin | `https://regtech365.com/admin/login` | `admin@regtech365.com` | `SecurePass123!` | `REGCOMPLY_PROD_ADMIN_URL`, `REGCOMPLY_PROD_REGTECH_ADMIN_EMAIL`, `REGCOMPLY_PROD_REGTECH_ADMIN_PASSWORD` |
| Production Client Portal | Organisation/Client roles | `<ENTER_PRODUCTION_CLIENT_URL>` | `<ENTER_PRODUCTION_CLIENT_EMAIL>` | `<ENTER_PRODUCTION_CLIENT_PASSWORD>` | `REGCOMPLY_PROD_CLIENT_URL`, `REGCOMPLY_PROD_CLIENT_EMAIL`, `REGCOMPLY_PROD_CLIENT_PASSWORD` |
| Production Audit Firm Portal | External Auditor | `<ENTER_PRODUCTION_AUDIT_FIRM_URL>` | `<ENTER_PRODUCTION_AUDIT_FIRM_EMAIL>` | `<ENTER_PRODUCTION_AUDIT_FIRM_PASSWORD>` | `REGCOMPLY_PROD_AUDIT_FIRM_URL`, `REGCOMPLY_PROD_AUDIT_FIRM_EMAIL`, `REGCOMPLY_PROD_AUDIT_FIRM_PASSWORD` |

**Production execution policy:** Production is the live customer environment. Use application logs, error monitoring, customer feedback, and operational alerts as the primary defect signals. Automated execution must be limited to authorized, lightweight, non-destructive smoke, health, and sanity checks. Production may also support controlled rollback verification after a release incident. Do not run the full regression suite, integration stress tests, load tests, bulk operations, destructive workflows, or routine data-creating scenarios against Production. Document creation, file upload, task assignment, approval, acknowledgement, notification delivery, risk generation, deletion, and similar state-changing checks require written authorization, isolated synthetic accounts, and an approved cleanup plan.

---

## 2. Feature List

### A. Identity, Access, and Role Governance

- **Role-Based Permissions:** The PRD defines Compliance Manager, Admin, Analyst, Internal Auditor, and Department Lead access. External Auditors operate through an Audit Firm entity and separate dashboard.
- **Department Lead Designation:** Department Lead is a designation applied to an existing role rather than a standalone role type. It adds department task-receiving and delegation rights.
- **Compliance Manager Override:** Document- or workflow-level change-management settings configured by the Compliance Manager override default role permissions.
- **Document Upload Rights:** Compliance Manager, Admin, Analyst, Internal Auditor, and Department Lead may upload documents.
- **Task Creation Rights:** Only Compliance Manager and Admin may create parent or manual tasks.
- **Task Assignment Rights:** Compliance Manager and Admin may assign tasks globally. Department Leads may assign or delegate within their own department only.
- **Sub-task Rights:** Compliance Manager, Admin, Analyst, and Department Lead may create sub-tasks subject to assignment and parent-task rules. Internal Auditors may not create sub-tasks.
- **Approval Rights:** Only Compliance Manager and Admin may approve documents.
- **Review Rights:** Compliance Manager, Admin, Analyst, and Department Lead may review. Internal Auditors may review only documents explicitly shared with them.
- **Acknowledgement Rights:** All listed internal roles may acknowledge documents assigned to them.
- **Native Editing Rights:** Compliance Manager, Admin, Analyst, and Department Lead may edit documents. Internal Auditors have no native edit rights.
- **Framework Mapping Rights:** Compliance Manager, Admin, and Analyst may map documents to frameworks. Internal Auditor and Department Lead mapping is prohibited by the permissions table.
- **AI Findings Access:** Compliance Manager, Admin, Analyst, and Department Lead may view findings. Internal Auditors may view findings only when the underlying document is shared.
- **Risk Register Access:** Compliance Manager, Admin, and Analyst may view risk records generated by document and task failures.
- **Integration Administration:** Only Compliance Manager and Admin may manage integrations.
- **External Auditor Access:** External Auditors receive read-only access only to documents shared with their Audit Firm during an active audit engagement. Access is automatically revoked when the engagement closes. They have no task-management or document-administration permissions.

### B. Document Ingestion and Repository

- **Framework-Mapped Upload:** A user selects an active framework and clause before or during upload. The document is mapped to the clause and becomes compliance evidence.
- **Accepted File Types:** Direct upload supports PDF, DOCX, XLSX, PPTX, PNG, and JPG.
- **Upload Sources:** A file may be uploaded by drag-and-drop, file browser, Google Workspace import, or selection from the existing repository.
- **Parent-Task Association:** Where a parent framework task exists for the selected clause, the upload creates a document sub-task carrying the clause reference, document ID, owner, and initial `draft` state.
- **AI Evaluation Trigger:** AI evidence evaluation begins only after a document is mapped to a framework clause.
- **General Repository Upload:** A document may be uploaded without a framework mapping. It is stored as an organisational record, remains searchable and shareable, and can be mapped retrospectively.
- **General Upload Inputs:** File, title, description, category tag, and optional department tag.
- **Unclassified Records:** General uploads are marked `unclassified` until mapped or otherwise categorized.
- **Central Repository:** All framework-mapped and general documents converge in one organisation repository.
- **Repository Search and Filtering:** Search and filter by title, type, framework, clause, department, status, and date range.
- **Repository Views:** All Documents, By Framework, By Clause, By Department, Unclassified, and Pending Action.
- **Document Metadata:** Document records include identity, title, description, file reference, file type, version, status, creator, creation time, framework mappings, clause references, optional parent task, and optional change type.

### C. Framework Mapping, Evidence Gaps, and AI Recommendations

- **Many-to-Many Mapping:** One document may map to multiple frameworks and multiple clauses.
- **Mapping Provenance:** Each mapping records whether it was created by a user or suggested by AI.
- **AI Confirmation Gate:** AI-suggested mappings do not become active until an authorized user confirms them.
- **Evidence Type:** Mapping records identify the evidence type, including policy, procedure, record, configuration, or certificate.
- **Continuous Gap Detection:** The system compares active framework tasks and clause obligations against approved document mappings.
- **Gap States:**
  - `no_document`: no document mapped to the clause.
  - `draft_only`: a document exists but is not approved.
  - `pending_ack`: approved evidence is not fully acknowledged.
  - `satisfied`: evidence is approved and all required acknowledgements are complete.
- **Gap Visibility:** Gaps appear in the framework task view and compliance posture dashboard.
- **Gap Grace Period:** A configurable grace period applies per framework; the PRD specifies a default of 30 days.
- **Gap Risk Escalation:** An unresolved gap creates a Risk Register record after the grace period.
- **Gap Risk Ownership:** The Compliance Manager is the default owner.
- **Gap Risk Closure:** The risk auto-closes when the clause reaches `satisfied`.
- **Qualitative AI Findings:** AI findings identify strengths, gaps, recommendations, relevant evidence extracts, and clause sub-components. They must not issue a binary pass/fail verdict.
- **Cross-Framework Intelligence:** AI may identify overlapping clauses in other active registered frameworks.
- **Unregistered Framework Restriction:** Relevance to an unregistered framework produces a recommendation to the Compliance Manager; it does not create a mapping or automatically register the framework.
- **Recommendation Resolution:** A user may confirm an AI cross-reference to create a mapping or dismiss it. Dismissal is logged.

### D. Deduplication and Repetition Control

- **Pre-Save Detection:** Duplicate and repetition checks run before the document is saved.
- **Primary Clause Check:** The system first checks whether the selected clause already has an active approved or in-review document.
- **Primary Warning Resolution:** The user must choose New Version, Supplementary Document, or Cancel.
- **Secondary Content Similarity:** Semantic similarity is used as a secondary signal against existing repository content.
- **Similarity Warning:** A likely match is presented as a suggestion rather than an automatic block.
- **Resolution Paths:**
  - New Version creates a version under the existing document.
  - Supplementary Document creates a separate document mapped to the same clause.
  - Replace archives the existing document and makes the new document the primary mapping.
  - Cancel aborts the upload.
- **Threshold Status:** The similarity threshold and embedding model remain unresolved. The PRD recommends 0.85 cosine similarity as an initial value but does not establish it as approved behaviour.

### E. Native Editing, Version Control, and Change Governance

- **Native DOCX Editing:** Rich text, H1–H4 headings, paragraph styles, ordered and unordered lists, tables, image insertion, comments, annotations, and tracked changes.
- **Native XLSX Editing:** Cell editing, basic formulas, and row/column operations.
- **Native PPTX Editing:** Slide text editing, image replacement, and slide reordering.
- **Read-Only Preview:** Users with document access may preview supported files even when they lack edit rights.
- **Track Changes:** Track changes is automatically enabled from `in_review` onward. Changes are attributed to the editing user.
- **Approved Document Lock:** Once approved, a document version is locked. Further editing requires creation of a new version.
- **Version History:** Every approved document maintains immutable version snapshots and predecessor references.
- **Version Labels:** The system assigns sequential versions and also permits a custom version label.
- **Change Summary:** A change summary is mandatory on every new-version save, including native editor saves, Google syncs, and file re-uploads.
- **Major Change:**
  - Returns the document to review.
  - Requires re-approval.
  - Triggers re-acknowledgement for previous acknowledgers.
  - Creates a risk if the re-acknowledgement deadline is missed.
- **Minor Change:**
  - Retains the existing document status.
  - Does not require re-approval or re-acknowledgement.
  - Sends an informational notification to previous acknowledgers.
  - Does not create a risk if the notification is unread.
- **Immutable Change Log:** Entries include version transition, change type, summary, user identity, timestamp, approval identity, approval timestamp, and optional diff reference.
- **Change Log Restrictions:** Existing entries cannot be edited or deleted. Compliance Manager and Admin may append a note without modifying the original record.

### F. Review, Approval, and Acknowledgement

- **Creator-Defined Workflow:** On submission, the creator selects reviewers, one approver, and acknowledgers.
- **Acknowledger Sources:** Individuals, roles, departments, or combinations.
- **Parallel Review and Approval:** Reviewers and the approver are notified simultaneously. Reviewers comment and mark review complete; the approver approves or rejects.
- **Submission State:** Submission changes the document to `in_review`.
- **Rejection State:** Approver rejection returns the document to `draft` and requires a rejection reason. The creator is notified.
- **Approval State:** Approval records the approver identity and timestamp, locks the version, and starts acknowledgement.
- **Acknowledgement Gate:** Acknowledgement cannot begin before approval.
- **Acknowledgement Tracking:** Each designated user is tracked as pending, acknowledged, or overdue, with the acknowledged version and timestamp recorded.
- **Frozen Recipient Set:** The acknowledgement list is frozen at submission. New staff are not automatically added, although the creator may explicitly add them.
- **Deadline Escalation:** Missed approval or acknowledgement deadlines create Risk Register records.
- **Per-User Acknowledgement Risk:** A missed acknowledgement creates a separate risk for each overdue acknowledger.
- **Automatic Closure:** Risks close when the overdue approval or acknowledgement is completed.

### G. AI-Generated and Manual Task Management

- **AI Task Generation:** At onboarding completion, AI generates organisation-specific baseline framework tasks.
- **Generation Inputs:** Industry, industry category, services, licence types, active frameworks, SWOT outputs, PESTLE outputs, organisation size, and organisation structure.
- **Generated Task Fields:** Title, description, framework, clause, document/non-document type, priority, suggested owner, suggested due date, and evidence requirements.
- **Human Review Gate:** AI-generated tasks remain inactive until reviewed by the Compliance Manager.
- **Review Actions:** The Compliance Manager may edit, reprioritize, reassign, delete, approve, or activate generated tasks.
- **Regeneration Triggers:** New framework registration, organisation profile changes, new services or licences, and relevant RegWatch regulatory circulars.
- **Manual Task Creation:** Compliance Manager and Admin may create standalone parent tasks or sub-tasks.
- **Required Manual Fields:** Title, description, task type, due date, and assignee.
- **Optional Manual Fields:** Framework, clause, parent task, priority, evidence requirements, and recurrence.

### H. Parent Tasks, Sub-Tasks, Assignment, and Completion

- **Parent Obligation Model:** A parent framework task represents the regulatory obligation.
- **Document Action Model:** Upload, review, approval, and acknowledgement actions are sub-tasks and do not exist as independent top-level tasks.
- **Parent Status Computation:**
  - `not_started`: no sub-tasks.
  - `in_progress`: at least one active sub-task.
  - `at_risk`: one or more sub-tasks overdue.
  - `complete`: all required sub-tasks complete.
  - `overdue`: parent due date passed while incomplete.
- **Automatic Sub-Task Creation:** Upload creates an upload sub-task. Workflow submission creates review, approval, and acknowledgement sub-tasks.
- **Assigned-User Rights:** A user assigned to a parent task may create document sub-tasks.
- **Non-Document Task Completion:** Completion requires both an attestation containing the date and activity description and a record-document upload.
- **Evidence Storage:** The record document is stored in the repository and may be framework-mapped.
- **Optional Evidence Workflow:** A record document does not automatically enter full review/approval/acknowledgement unless the Compliance Manager requires it.
- **Assignment Targets:** Individual, role, or department.
- **Department Routing:** Department assignment routes to the designated Department Lead.
- **Missing Lead Fallback:** Where no Department Lead exists, the task routes to the Compliance Manager.
- **Delegation:** Department Leads may delegate sub-tasks to team members but remain accountable for the parent task.

### I. Notifications, Risk Escalation, and Supporting Integrations

- **Dual-Channel Notifications:** Compliance notifications use in-app and email channels.
- **Mandatory Critical Notifications:** Users cannot opt out of compliance-critical notifications, including approval deadlines and overdue tasks.
- **Informational Preferences:** Users may configure digest preferences for non-critical notifications.
- **Task Timing Rules:**
  - Assignment notification immediately.
  - T-7 reminder to assignee and Department Lead.
  - T-1 reminder to assignee, Department Lead, and Compliance Manager.
  - Daily overdue notification until resolution.
- **Document Events:** Submission, approval, rejection, acknowledgement request, minor-version notification, and major-version re-acknowledgement.
- **Risk-Creating Events:** Missed approval, missed acknowledgement, missed task deadline, missed major-version re-acknowledgement, and unresolved evidence gaps.
- **Contextual Risk Records:** Risk records retain source document/task/user, framework, clause, creation time, owner, and status.
- **Risk Closure Notification:** Compliance Manager receives notice when a generated risk auto-closes.
- **Google Workspace:** OAuth 2.0 per user for Drive import and edit-in-Google workflows.
- **Identity Providers:** Okta and Azure AD through SAML/OIDC for SSO, provisioning, deprovisioning, group-role mapping, and Department Lead synchronization.
- **Email Provider:** SMTP or SendGrid is identified as the likely outbound email mechanism.
- **RegWatch:** Internal API feed from regulatory circulars to task-generation triggers.
- **Risk Register:** Internal API for risk creation and closure.
- **Audit Firm Dashboard:** Separate external-auditor access surface tied to active audit engagements.

---

## 3. User Stories

> **As a Compliance Manager,** I want to review and activate AI-generated framework tasks, so that the organisation’s obligation register reflects its actual industry, services, licences, frameworks, and regulatory changes.

> **As a Compliance Manager,** I want unresolved document, approval, acknowledgement, task, and evidence gaps to create contextual risks automatically, so that I can manage exceptions before they become audit failures.

> **As an Admin,** I want to create, assign, and monitor parent tasks and sub-tasks, so that compliance obligations are converted into accountable work.

> **As an Analyst,** I want to upload, edit, map, and evaluate documents against framework clauses, so that compliance evidence is accurate and traceable.

> **As an Internal Auditor,** I want read and review access only to documents shared with me, so that I can assess evidence without changing controlled records.

> **As a Department Lead,** I want department tasks routed to me and to delegate sub-tasks to team members, so that my department completes its obligations while I retain accountability.

> **As a Document Creator,** I want to select reviewers, one approver, and acknowledgers when submitting a document, so that review, authorization, and staff awareness follow a governed workflow.

> **As an Approver,** I want to approve or reject a specific document version with an auditable reason and timestamp, so that only authorized evidence becomes effective.

> **As an Acknowledger,** I want to view and acknowledge the approved version assigned to me, so that the organisation can prove that required staff received the document.

> **As a Task Assignee,** I want reminders, due dates, evidence requirements, and direct task links, so that I can complete obligations on time.

> **As a Non-Document Task Assignee,** I want to submit both an attestation and supporting record, so that operational compliance work is verifiable.

> **As an External Auditor,** I want temporary read-only access to documents shared with my Audit Firm during an active engagement, so that I can evaluate evidence without receiving internal task or editing rights.

> **As an Integration Administrator,** I want to connect Google Workspace and an identity provider, so that documents and users can be synchronized without bypassing RegComply governance.

> **As a Compliance Manager,** I want AI cross-framework recommendations to require confirmation, so that AI suggestions do not alter active compliance mappings without human authorization.

---

## 4. Core User Flows in Gherkin Syntax

The scenarios below define the PRD-derived behavioural coverage inventory. They are intended primarily for the QA environment. Repetitive, deterministic scenarios form the automation backlog; exploratory, visually subjective, newly introduced, or requirement-ambiguous behaviours remain manual until they become stable and objectively assertable. Production execution is limited to the separate read-only smoke and sanity policy defined in Section 8.

### Flow 1: Authorized Portal Authentication and Route Protection

```gherkin
Feature: RegComply role-based portal access

  Scenario Outline: An authorized account signs in to its assigned QA portal
    Given the <role> QA account is active
    And the corresponding portal URL and credentials are loaded from environment variables
    When the user submits valid credentials
    Then an authenticated session is established
    And the user can access only routes permitted for the <role>
    And credentials and session tokens are not exposed in logs, screenshots, or reports

    Examples:
      | role               |
      | RegTech Admin      |
      | Organisation Admin |
      | Audit Firm         |
      | Client             |

  Scenario: An authenticated user attempts to open a restricted document or task route
    Given the user is signed in with a role that lacks the required permission
    When the user navigates directly to the restricted route
    Then the system denies access
    And no protected document or task data is returned to the user
    And the denial does not change any underlying record
```

### Flow 2: Framework-Mapped Document Upload

```gherkin
Feature: Framework-mapped evidence upload

  Scenario: An authorized user uploads evidence for a framework clause
    Given the user has document-upload and framework-mapping permission
    And an active framework and clause are available
    And a parent framework task exists for the selected clause
    When the user selects the framework and clause
    And uploads a supported file with the required document metadata
    Then the system performs the clause-level duplication check before saving
    And the document is stored in the organisation repository
    And an active mapping is created for the selected framework and clause
    And an upload sub-task is created under the parent framework task with status "draft"
    And the sub-task stores the clause reference, document reference, and assigned owner
    And AI evidence evaluation is queued for the selected clause
```

### Flow 3: Duplicate Evidence Resolution

```gherkin
Feature: Duplicate and repetition control

  Scenario Outline: A clause already has an active document
    Given an approved or in-review document is already mapped to the selected clause
    And the user attempts to upload another document to the same clause
    When the system displays the existing document and the user selects <resolution>
    Then the system applies the <expected_result>
    And the selected action is recorded in the document audit trail

    Examples:
      | resolution             | expected_result                                                        |
      | New Version            | new file is stored as a version under the existing document             |
      | Supplementary Document | new file is stored separately and mapped to the same clause              |
      | Replace                | existing document is archived and the new document becomes primary       |
      | Cancel                 | upload is aborted and no document or mapping record is created           |

  Scenario: Content similarity warns without automatically blocking the upload
    Given no active document is mapped to the selected clause
    And the uploaded content exceeds the configured similarity threshold against an existing document
    When the secondary similarity check completes
    Then the user is shown the likely matching document
    And the user may continue as a separate document or link it as a version
    But the system does not merge, replace, or reject the upload without user confirmation
```

### Flow 4: General Repository Upload and Retrospective Mapping

```gherkin
Feature: Unclassified repository documents

  Scenario: A user uploads a general organisational document
    Given the user has document-upload permission
    When the user uploads a supported file with title, description, and category
    And does not select a framework or clause
    Then the document is stored in the central repository
    And the document is marked "unclassified"
    And it is searchable by its metadata
    And no framework mapping is created
    And AI evidence evaluation is not triggered

  Scenario: An authorized user maps an unclassified document later
    Given an unclassified document exists
    And the user has framework-mapping permission
    When the user confirms an active framework and clause mapping
    Then the mapping is stored with the user as its source
    And the document is removed from the Unclassified view
    And AI evidence evaluation is queued for the confirmed clause
```

### Flow 5: Review, Approval, Rejection, and Acknowledgement

```gherkin
Feature: Controlled document workflow

  Scenario: A creator submits a document for parallel review and approval
    Given a draft document exists
    And the creator selects one or more reviewers
    And the creator selects exactly one approver
    And the creator selects acknowledgement recipients
    When the creator submits the document
    Then the document status changes to "in_review"
    And review and approval sub-tasks are created
    And reviewers and the approver receive in-app and email notifications
    And acknowledgement does not begin before approval

  Scenario: The approver rejects the submitted version
    Given a document is "in_review"
    When the approver rejects it with a reason
    Then the document returns to "draft"
    And the rejection reason, approver identity, version, and timestamp are recorded
    And the creator is notified
    And no acknowledgement requests are issued

  Scenario: The approver approves the submitted version
    Given a document is "in_review"
    When the approver approves the version
    Then the document status changes to "approved"
    And the version is locked against direct editing
    And the approval identity and timestamp are recorded
    And acknowledgement sub-tasks are created for the frozen recipient set
    And each acknowledger receives an in-app and email request
```

### Flow 6: Acknowledgement Progress and Deadline Risk

```gherkin
Feature: Document acknowledgement tracking

  Scenario: A designated user acknowledges the approved version
    Given an approved document has been assigned to the user for acknowledgement
    And the acknowledgement is pending
    When the user opens the assigned document
    And submits acknowledgement
    Then the acknowledgement stores the user identity, timestamp, and document version
    And the user status changes from "pending" to "acknowledged"
    And the Compliance Manager progress view is updated

  Scenario: An acknowledgement deadline passes without action
    Given a required acknowledgement remains pending after its deadline
    When the deadline-monitoring process evaluates the record
    Then the acknowledgement status becomes "overdue"
    And one contextual risk is created for that overdue user
    And the Compliance Manager is notified
    When the overdue user later acknowledges the same required version
    Then the originating risk is closed automatically
    And the Compliance Manager receives a risk-closed notification
```

### Flow 7: Major and Minor Document Changes

```gherkin
Feature: Version and change governance

  Scenario Outline: An authorized editor saves a new version
    Given an approved document version exists
    And the user has edit permission
    When the user creates a new version
    And provides a mandatory change summary
    And classifies the change as <change_type>
    Then an immutable version snapshot and change-log entry are created
    And the system applies <workflow_effect>
    And the previous approved version remains available in version history

    Examples:
      | change_type | workflow_effect                                                                 |
      | major       | document returns to review, re-approval and re-acknowledgement are required     |
      | minor       | document status remains unchanged and previous acknowledgers are only notified |

  Scenario: Track changes is enforced after review begins
    Given a document is "in_review" or later
    When an authorized user edits the document through the native editor
    Then track changes is enabled automatically
    And each change is attributed to the editing user
    And the user cannot disable the governed change record for that version
```

### Flow 8: Manual Task Creation and Department Routing

```gherkin
Feature: Manual compliance task management

  Scenario: A Compliance Manager creates and assigns a parent task to a department
    Given the Compliance Manager is authenticated
    And a Department Lead is designated for the selected department
    When the Compliance Manager enters the required title, description, task type, due date, and department
    And optionally adds framework, clause, priority, evidence, or recurrence details
    Then the parent task is created
    And responsibility is routed to the Department Lead
    And the Department Lead receives an in-app and email assignment notification
    And the Department Lead may delegate sub-tasks to department members
    But the Department Lead remains accountable for the parent task

  Scenario: A department has no designated lead
    Given the Compliance Manager assigns a task to a department without a Department Lead
    When the assignment is saved
    Then responsibility falls back to the Compliance Manager
    And the fallback routing is visible in the task record
```

### Flow 9: Non-Document Evidence Completion and Parent Status

```gherkin
Feature: Evidence-backed operational tasks

  Scenario: A non-document task cannot complete with only one evidence component
    Given a non-document task is assigned to a user
    When the user submits an attestation without a record document
    Then the task remains incomplete
    And the system identifies the missing record-document requirement
    When the user uploads a record document without an attestation
    Then the task remains incomplete
    And the system identifies the missing attestation requirement

  Scenario: A non-document task completes with both evidence components
    Given the assigned user has submitted a dated attestation with an activity description
    And has uploaded a supporting record document
    When the completion request is submitted
    Then the non-document task changes to "complete"
    And the record document is stored in the repository
    And the parent task status is recalculated from all required sub-tasks

  Scenario: A parent task reflects overdue sub-task risk
    Given a parent task has at least one required overdue sub-task
    When the task status is recalculated
    Then the parent task is marked "at_risk"
    And the overdue condition is visible to the accountable owner
```

---

## 5. Expected Outcomes and Success Measures

### Functional Outcomes

- Every compliance-evidence document can be traced to the framework clauses it satisfies.
- General organisational documents remain searchable without being falsely treated as compliance evidence.
- A document can satisfy multiple active framework clauses without duplicate uploads.
- AI recommendations cannot create active mappings without authorized human confirmation.
- Document statuses, task statuses, version changes, approvals, rejections, and acknowledgements follow system-enforced transitions.
- Approved versions are immutable and remain retrievable in version history.
- Review, approval, acknowledgement, and change actions retain actor identity, version, reason where applicable, and timestamps.
- Acknowledgement records prove which version each user acknowledged.
- Parent task status reflects the state of required sub-tasks.
- Non-document tasks cannot complete without both attestation and record evidence.
- Department assignments route to the Department Lead or fall back to the Compliance Manager.
- External Audit Firm access remains read-only, engagement-scoped, and automatically revocable.
- Missed deadlines and unresolved evidence gaps create contextual risks rather than generic alerts.
- Generated risks close automatically when the originating condition is resolved.

### Quantitative and Time-Based Rules Defined by the PRD

- Evidence-gap grace period defaults to **30 days** and is configurable per framework.
- Task reminders occur at **T-7 days** and **T-1 day**.
- Overdue task notifications recur **daily until resolution**.
- Approval and acknowledgement deadlines create risks when missed.
- The approved file types are **PDF, DOCX, XLSX, PPTX, PNG, and JPG**.
- One document workflow has **one approver** and one or more reviewers and acknowledgers.
- An overdue acknowledgement creates a risk **per overdue user**.

### Security and Access Outcomes

- Restricted roles cannot create, assign, approve, edit, map, or administer integrations outside their PRD-defined permissions.
- Direct-route access does not bypass role checks.
- External Auditors cannot access unshared documents, organisation task management, native editing, or administrative features.
- Secrets, tokens, and personal data are masked from automation logs, screenshots, videos, and reports.
- Production automation remains non-destructive unless separately authorized.

### Auditability Outcomes

- Change logs are immutable.
- Existing change-log entries cannot be edited or deleted.
- Document mappings retain source and confirmation status.
- Risk records retain their originating document, task, user, framework, and clause context.
- Notification-triggering events and resulting state changes are traceable.
- AI findings retain framework, clause, sub-component, finding type, generation time, and supporting extract where available.

### Measures Not Yet Defined

The PRD does not define accepted response-time thresholds, upload-size limits, AI evaluation duration, notification delivery service levels, search latency, synchronization latency, concurrency limits, uptime targets, accessibility criteria, supported browsers, or target defect/error rates. These require explicit acceptance criteria before performance or service-level automation can be considered complete.

---

## 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
|---|---|---|
| Authentication and Sessions | Account login, session creation, route protection, logout | Front-end authentication routes and identity/session API; exact provider and token model require network inspection |
| Role-Based Access Control | Permission checks for document, task, mapping, approval, risk, and integration actions | Authorization middleware and role/permission data persisted by the platform; direct-route and API enforcement must both be verified |
| Department Lead Designation | Adds department task-routing rights to an existing user role | User/department relationship in the People service; may also synchronize from IdP groups |
| Direct File Upload | Store supported files and metadata | Multipart or pre-signed upload API plus object/file storage; malware scanning and size constraints are not specified |
| Repository | Search, filter, view, and retrieve documents | Document metadata API, database persistence, and file-storage retrieval |
| Framework Mapping | Many-to-many document/framework/clause relationships | Mapping API and relational/document database records |
| Gap Detection | Compare obligations with approved evidence state | Background evaluator or event-driven service reading framework tasks, mappings, document status, and acknowledgements |
| Duplicate Clause Check | Detect active evidence already mapped to a clause | Synchronous repository/mapping query before save |
| Semantic Similarity | Detect similar content across repository documents | AI/embedding service and indexed document representations; model and threshold require confirmation |
| AI Evidence Evaluation | Generate clause-specific strengths, gaps, recommendations, and extracts | AI inference service, document parsing pipeline, findings persistence, and background job/queue |
| Cross-Framework Recommendations | Suggest additional registered-framework mappings | AI cross-reference knowledge base plus recommendation/confirmation API |
| Native Editor | Edit DOCX, XLSX, and PPTX with controlled capabilities | Browser document editor and conversion/rendering service; implementation library not specified |
| Track Changes | Attribute edits after review submission | Editor event/change stream and immutable change-log persistence |
| Version Control | Immutable snapshots, custom labels, predecessor links | File-version storage and version metadata API |
| Review and Approval | Parallel reviewer/approver tasks, comments, approval/rejection | Workflow orchestration API, comments store, task creation, notification events |
| Acknowledgement | Frozen recipient resolution, per-user status, version proof | People/role/department expansion service, acknowledgement records, deadline scheduler |
| AI Task Generation | Generate task baseline from organisation profile | Organisation Profile, SWOT/PESTLE, framework, and AI generation services |
| Manual Task Creation | Parent and sub-task CRUD | Task API and database persistence |
| Parent Status Computation | Derive parent state from child-task states and deadlines | Task aggregation logic; likely event-driven or recalculated on task changes |
| Non-Document Completion | Require attestation and record upload | Task completion API plus repository/file-storage integration |
| Department Assignment | Route department work to lead, delegate sub-tasks | People/department service and task assignment logic |
| Reminders and Notifications | In-app and email events, T-7, T-1, overdue daily | Notification service, scheduler/background jobs, in-app notification store, email provider |
| Risk Escalation | Create and auto-close risks from missed conditions | Internal Risk Register API with source correlation and idempotent create/close behaviour |
| Google Drive Import | One-time copy from Google Drive | Google Drive API using per-user OAuth 2.0 |
| Google Edit Mode | Edit in Docs/Sheets/Slides and sync back with version bump | Google Workspace APIs, OAuth 2.0, save/change callbacks or polling, conflict lock |
| Identity Provider | SSO, provisioning, deprovisioning, group-role and lead sync | SAML/OIDC for authentication and likely SCIM or provider-specific provisioning; exact contract is not stated |
| RegWatch | New regulatory circular triggers task regeneration | Internal API/event feed from RegWatch to task-generation workflow |
| Audit Firm Access | Engagement-scoped external read-only document access | Audit engagement service, sharing/ACL service, and automatic revocation event |
| Dashboards | Gap state, acknowledgement progress, task status, risk visibility | Aggregation/query APIs; formulas and refresh model require confirmation |

---

## 7. Integration-Based Gherkin Flows

These integration scenarios are designed for QA or an approved provider sandbox. Contract, retry, failure-recovery, idempotency, and data-synchronization checks may be automated in QA. Stress and resilience tests must run as separately scheduled suites with agreed limits; they must not run as part of the ordinary browser regression suite or against Production.

### Integration Flow 1: Google Drive Import

```gherkin
Feature: Google Drive document import

  Scenario: A connected user imports a file as a one-time copy
    Given the user has authorized Google Workspace through OAuth
    And has document-upload permission
    When the user selects a supported Google Drive file for import
    Then the system copies the file into the RegComply repository
    And creates the RegComply document metadata record
    And the original Google Drive file remains unchanged
    And subsequent native edits do not write back to the original file
```

### Integration Flow 2: Edit-in-Google Synchronization

```gherkin
Feature: Google Workspace edit synchronization

  Scenario: A Google edit is synchronized as a governed new version
    Given a RegComply document is opened in the supported Google editor
    And the user holds the active edit lock
    When the user saves changes in Google Workspace
    Then the updated file is synchronized back to RegComply
    And a new document version is created
    And the user must provide or confirm the required change classification and summary
    And RegComply applies the major or minor downstream workflow
    And the change is recorded in the immutable change log

  Scenario: A second user attempts to edit during an active Google edit session
    Given one user holds the document edit lock
    When another user attempts to open the same document for editing
    Then concurrent editing is prevented or resolved according to the approved lock strategy
    And no silent overwrite or untracked version replacement occurs
```

### Integration Flow 3: Identity Provider Provisioning and Revocation

```gherkin
Feature: Identity-provider synchronization

  Scenario: An identity-provider user is provisioned into RegComply
    Given the organisation has connected an approved identity provider
    And an external identity is assigned to a mapped group
    When the provisioning event is processed
    Then the user identity is created or updated in RegComply
    And the mapped base role is applied
    And any mapped Department Lead designation is synchronized
    But compliance-specific roles remain governed by RegComply configuration

  Scenario: A deprovisioned identity loses RegComply access
    Given an active RegComply user is removed or disabled in the identity provider
    When the deprovisioning event is processed
    Then the user can no longer authenticate
    And existing audit records remain intact
    And outstanding ownership or assignment consequences are surfaced for reassignment
```

### Integration Flow 4: RegWatch Regulatory Trigger

```gherkin
Feature: RegWatch-driven obligation updates

  Scenario: A relevant regulatory circular triggers task regeneration
    Given RegWatch processes a new circular that creates an obligation applicable to the organisation
    When RegComply receives the regulatory trigger
    Then the organisation task-generation workflow is initiated
    And proposed tasks retain the applicable framework or regulatory context
    And the proposed tasks remain inactive pending Compliance Manager review
    And duplicate trigger processing does not create duplicate active tasks
```

### Integration Flow 5: Notification Queue and Delivery Failure

```gherkin
Feature: Compliance notification delivery

  Scenario: A compliance-critical event produces in-app and email notifications
    Given a task assignment, review request, approval request, or acknowledgement request is committed
    When the notification event is processed
    Then an in-app notification is created for each intended recipient
    And an email delivery request is created for each intended recipient
    And the notification links to the relevant document or task
    And the event is not duplicated when the same message is retried

  Scenario: The external email provider temporarily fails
    Given the in-app notification has been persisted
    And the email provider returns a transient failure
    When the notification service applies its configured retry policy
    Then the in-app notification remains available
    And the email attempt is retained for retry or operational review
    And the document or task transaction is not rolled back
    And the failure is visible without exposing recipient secrets
```

### Integration Flow 6: Risk Register Creation and Auto-Closure

```gherkin
Feature: Internal Risk Register synchronization

  Scenario Outline: A missed compliance condition creates one contextual risk
    Given a monitored <condition> is overdue or unresolved
    When the deadline or gap evaluator processes the condition
    Then one open risk is created with the originating document, task, user, framework, and clause context where applicable
    And the Compliance Manager is assigned as the default owner
    And repeated evaluation does not create duplicate open risks for the same source condition

    Examples:
      | condition                    |
      | document approval            |
      | user acknowledgement         |
      | task completion              |
      | major-version acknowledgement|
      | evidence gap                 |

  Scenario: Resolving the source condition closes the linked risk
    Given an open generated risk exists
    When the originating overdue action or evidence gap is resolved
    Then the linked risk is closed automatically
    And the closure time and resolving action are recorded
    And the Compliance Manager receives a risk-closed notification
```

### Integration Flow 7: AI Evidence and Cross-Framework Recommendation

```gherkin
Feature: AI evidence evaluation integration

  Scenario: A mapped document produces clause-specific findings
    Given a document is actively mapped to a registered framework clause
    When AI evaluation completes
    Then findings identify covered sub-components, missing sub-components, and recommendations
    And findings may include supporting extracts from the document
    But the AI does not issue a binary pass or fail verdict

  Scenario: AI identifies relevance to another registered framework
    Given the organisation has another active registered framework
    And AI identifies an overlapping clause
    When the recommendation is displayed
    Then no mapping is created until an authorized user confirms it
    When the user confirms the recommendation
    Then the additional mapping is created
    And the relevant framework gap state is recalculated

  Scenario: AI identifies relevance to an unregistered framework
    Given AI identifies relevance to a framework not registered by the organisation
    When the evaluation completes
    Then the Compliance Manager receives a registration recommendation
    But no framework registration or document mapping is created automatically
```

### Integration Flow 8: External Auditor Engagement Access

```gherkin
Feature: Audit Firm document sharing

  Scenario: An External Auditor accesses a shared document during an active engagement
    Given an Audit Firm engagement is active
    And a document has been explicitly shared with that firm
    When the External Auditor opens the document
    Then the document is available in read-only mode
    And the auditor cannot edit, map, approve, acknowledge, create tasks, or access unshared documents

  Scenario: Audit closure revokes external access
    Given an External Auditor previously had engagement-scoped access
    When the audit engagement closes
    Then the shared access grant is revoked automatically
    And direct links no longer return the protected document
    And the prior access history remains auditable
```

---

## 8. System Instruction for the RegComply Document & Task Management QA Automation Agent

### Role

You are the **Senior QA Automation Engineer for RegComply Document & Task Management**. You are responsible for designing and maintaining the automated checks that remove repetitive QA effort while preserving manual capacity for exploratory testing, usability assessment, novel risk investigation, and ambiguous or unstable product behaviour.

You must operate differently by environment:

- **QA:** Execute automated and manual tests, validate new features and bug fixes, run regression suites, test negative and edge cases, exercise APIs and integrations, perform controlled stress testing, and document defects.
- **Production:** Monitor logs and error reports, run only authorized smoke and sanity checks, support rollback verification, and correlate customer feedback with observable system behaviour. Do not treat Production as a general regression or stress environment.

Your objective is not to automate every possible test. Your objective is to automate the **repetitive, deterministic, business-relevant checks** that must be rerun after deployments, code changes, and bug fixes.

### Platform Overview

RegComply Document & Task Management converts regulatory obligations into traceable tasks and controlled evidence.

The platform includes:

- Framework-mapped and general document uploads
- Central repository search and filtering
- Many-to-many framework and clause mapping
- Gap-state calculation and Risk Register escalation
- Native DOCX, XLSX, and PPTX editing
- Review, approval, rejection, and acknowledgement workflows
- Deduplication and semantic repetition warnings
- Immutable document versions and change logs
- AI clause evaluation and cross-framework recommendations
- AI-generated and manually created compliance tasks
- Parent/sub-task status computation
- Non-document evidence completion
- Department Lead routing and delegation
- In-app and email notifications
- Google Workspace, identity-provider, RegWatch, Risk Register, and Audit Firm integrations

The PRD roles are Compliance Manager, Admin, Analyst, Internal Auditor, Department Lead designation, and External Auditor. The supplied environments also expose RegTech Admin, Organisation Admin, Audit Firm, and Client accounts. Do not assume that the environment-account labels are equivalent to the PRD roles until browser and API evidence confirms the mapping.

### Automation Test Basis

Build the automation backlog from the following test bases:

1. **PRD Functional Requirements**
   - Feature rules
   - Required fields
   - Validation rules
   - Workflow dependencies
   - Supported file types
   - Notification timing
   - Evidence and audit requirements

2. **Role and Permission Matrix**
   - Allowed actions
   - Prohibited actions
   - Department-level scope
   - External-auditor restrictions
   - UI and API authorization boundaries

3. **Business-State Transitions**
   - Document lifecycle
   - Version lifecycle
   - Approval and acknowledgement states
   - Parent/sub-task state computation
   - Gap-to-risk creation and closure

4. **Repetitive User Journeys**
   - Login and logout
   - Document upload
   - Repository search and filtering
   - Framework mapping
   - Review, approval, rejection, and acknowledgement
   - Task creation, assignment, delegation, and completion
   - Dashboard or aggregate-state validation
   - Report, notification, and integration checks

5. **Known Defects and Bug-Fix History**
   - Every confirmed repeatable defect should become a regression candidate after the fix is validated.

6. **Integration Contracts**
   - Request and response behaviour
   - Synchronization
   - Retry handling
   - Idempotency
   - Failure recovery
   - Data consistency across services

### Automation Selection Rule

Automate a test in QA when the behaviour is:

- Repeated after deployments, feature changes, or bug fixes
- Deterministic enough to produce objective pass/fail assertions
- Business-critical or likely to regress
- Supported by stable setup and cleanup
- Safe to execute repeatedly
- Observable through the UI, API, database-facing response, queue result, or generated record
- Valuable enough to justify maintenance cost

Keep a test primarily manual when it involves:

- Exploratory discovery
- Usability or workflow comprehension
- Visual quality that cannot be asserted reliably
- Subjective AI-output quality
- New or rapidly changing functionality
- Ambiguous requirements
- Rare combinations whose automation setup is disproportionately expensive
- Complex concurrency or provider behaviour that is not yet controllable
- High-risk investigation requiring human judgment

High risk alone is not a reason to keep a test manual. Automate **high-risk deterministic checks**; manually explore **high-risk unpredictable checks**.

### Environment and Secret Management

- Load every URL, username, password, token, OAuth value, and API configuration from environment variables.
- Never hard-code secrets in tests, fixtures, page objects, support files, configuration committed to source control, or generated reports.
- Mask authentication values, access tokens, refresh tokens, cookies, signed URLs, user personal data, and uploaded-file contents in logs and evidence.
- Keep QA, staging, and Production configuration isolated.
- Use synthetic organisations, users, documents, clauses, tasks, acknowledgements, and audit engagements.
- Do not use real regulated or personal information unless formally authorized.
- Treat the credentials table in this specification as an onboarding reference only; runtime automation must consume environment variables.
- Require an explicit environment guard before any suite can target Production.

### Core Responsibilities

#### 1. PRD Decomposition and Traceability

- Convert PRD requirements into testable behaviours.
- Preserve source identifiers such as `F-DOC-01`, `F-TASK-03`, `F-VER-02`, `F-NOTIF-02`, and `F-AI-01`.
- Assign stable automated-test identifiers such as `DOC-UPLOAD-001`, `RBAC-APPROVAL-003`, and `TASK-ROUTING-002`.
- Maintain a coverage matrix showing:
  - Requirement identifier
  - Business behaviour
  - Role
  - Environment
  - Test type
  - Automation suitability
  - Automation status
  - Manual coverage status
  - Last execution result
  - Defect or clarification dependency
- Do not mark a requirement automated merely because the page or control is reachable.

#### 2. QA Environment Automation

Use QA as the primary automation environment.

Automate the following repetitive checks first:

**P0 — Deployment Smoke**

- QA portal availability
- Login for supplied accounts
- Session creation and logout
- Core route access
- Restricted-route denial
- Document and Task Management module availability
- Critical API availability

**P0 — Critical Regression**

- Framework-mapped document upload
- General repository upload
- Repository persistence
- Framework and clause mapping
- Duplicate resolution
- Review and approval initiation
- Approval and rejection
- Acknowledgement initiation and completion
- Manual task creation and assignment
- Parent/sub-task relationship and status updates
- Non-document evidence completion
- Approved-version locking

**P1 — Repetitive Functional Regression**

- Search, filtering, and repository views
- Major and minor version behaviour
- Change-summary validation
- Immutable version and change-log history
- Department Lead routing and missing-lead fallback
- Notification-event creation
- Risk creation and automatic closure
- External Auditor read-only restrictions

**P1 — RBAC Regression**

- Allowed UI actions by role
- Prohibited UI actions by role
- Direct-route denial
- API-level permission denial
- Department-boundary enforcement
- Engagement-scoped Audit Firm access

**P1 — Bug Regression**

- Validate every fixed defect in QA.
- Add a permanent automated regression check when the defect is repeatable and deterministic.
- Link the regression test to the defect identifier.

**P2 — API and Integration Regression**

- Request and response validation
- UI/API state consistency
- Retry behaviour
- Idempotency
- Provider-failure handling
- Google synchronization
- Notification queue behaviour
- RegWatch triggers
- Risk Register synchronization
- Identity lifecycle
- Audit-engagement access revocation

#### 3. QA Manual Testing Allocation

Preserve manual effort for:

- Exploratory testing of newly delivered features
- Requirement-gap investigation
- Usability and workflow comprehension
- Native document-editor formatting quality
- Visual rendering and layout defects
- Subjective AI finding relevance and compliance usefulness
- Complex concurrent editing
- Unusual file content
- Novel integration behaviour
- Security and abuse exploration not yet stable enough for automation
- High-risk combinations requiring human judgment

Create explicit exploratory charters instead of leaving manual coverage undefined.

#### 4. QA Stress and Resilience Testing

- Run stress testing only in QA or an approved sandbox.
- Keep stress suites separate from ordinary UI smoke and regression suites.
- Stress APIs, queues, notification processing, synchronization, uploads, and integration boundaries only after limits are agreed.
- Use controlled data volumes and synthetic records.
- Record the agreed concurrency, throughput, duration, and recovery expectations.
- Verify graceful degradation, retry behaviour, timeout handling, and recovery.
- Do not run uncontrolled “bombardment” tests.
- Do not run stress, load, soak, or volume tests against Production unless a separately approved operational exercise exists.

#### 5. Production Environment Policy

Production is not a general automation target.

Permitted Production activities, subject to authorization:

- Application availability checks
- Login-page and core-route health
- Read-only API health checks
- Authorized authentication checks with isolated accounts
- Non-destructive navigation
- Critical dependency monitoring
- Log and error-report review
- Release smoke checks
- Occasional sanity checks
- Rollback verification after a release incident
- Correlation of customer feedback with logs, traces, and known defects

Prohibited by default:

- Full regression execution
- Stress, load, soak, or volume testing
- Bulk data creation
- Routine document upload
- Task creation or assignment
- Approval, rejection, or acknowledgement
- Notification delivery tests
- Risk-generation tests
- Identity provisioning tests
- Google synchronization tests
- Deletion
- Destructive cleanup
- Any test that can alter real customer data

Do not run a state-changing Production test unless written authorization defines:

- The exact scenario
- The isolated synthetic account and tenant
- The permissible data
- The execution window
- Monitoring ownership
- Cleanup
- Rollback
- Success and abort criteria

#### 6. Environment and Network Audit

- Audit each supplied QA portal and determine the actual post-login route, role, organisation context, and visible modules.
- Locate the actual Document Management and Task Management routes.
- Record front-end routes, network requests, response patterns, asynchronous jobs, storage calls, and state transitions.
- Build a route and API inventory without exposing credentials or sensitive payloads.
- Confirm which APIs enforce authorization independently of the UI.
- Identify feature flags or environment drift between staging and QA.
- Request Swagger/OpenAPI documentation, event contracts, and sandbox credentials when network inspection cannot establish a safe contract.
- Never invent endpoint paths or payload schemas.

#### 7. Automation Framework Standards

- Use Cypress or Playwright according to the repository standard.
- Use JavaScript or TypeScript according to the existing project convention.
- Preserve the current project runner, reporter, linting, CI, and directory conventions.
- Implement reusable authentication helpers for each verified role.
- Use API-assisted setup where safe and documented.
- Use UI flows for the business behaviour under test, not for every prerequisite.
- Use stable `data-*` test hooks. Report missing stable selectors as testability defects.
- Avoid selectors based on transient text, generated class names, list position, or deep DOM structure.
- Avoid fixed waits.
- Synchronize against observable UI state, aliased network requests, polling completion, or documented job state.
- Create isolated synthetic records with run-specific identifiers.
- Ensure tests are independently executable, retry-safe, and parallel-safe.
- Clean up generated records where supported.
- Preserve failure artifacts while masking secrets.
- Validate UI outcomes and persisted/API state for critical workflows.
- Clearly label mocked, contract, sandbox-integration, and end-to-end tests.

#### 8. Negative and Edge-Case Coverage

Automate stable, repeatable negative cases such as:

- Invalid credentials
- Expired sessions
- Unauthorized route and API access
- Missing required fields
- Unsupported file types
- Duplicate records
- Duplicate submissions
- Invalid state transitions
- Acknowledgement by an unassigned user
- Version save without change summary
- Attestation without evidence
- Evidence without attestation
- Department assignment without a lead
- Provider retry and duplicate callback handling
- Duplicate risk creation
- Failed export or notification events
- Stale session or stale data behaviour

Keep unusual, visually subjective, provider-dependent, or poorly specified edge cases in manual exploratory coverage until they become deterministic.

#### 9. API and Integration Verification

- Validate request initiation, authorization, response handling, persistence, UI synchronization, retries, and idempotency.
- Verify that downstream notification failure does not roll back a committed document or task transaction.
- Verify that repeated deadline evaluation does not create duplicate open risks.
- Verify that repeated RegWatch triggers do not create duplicate tasks.
- Verify that Google synchronization creates one governed version per accepted change.
- Verify that IdP deprovisioning revokes access while preserving historical records.
- Verify that external-auditor revocation applies at UI, route, and API levels.
- Use mocks for failure and rare-condition coverage, but retain separate real sandbox integration tests.
- Record undocumented behaviour as an observation rather than a requirement.

#### 10. Defect Evidence and Reporting

For every failed critical test:

- Capture visible application state.
- Capture relevant network metadata with secrets masked.
- Record requirement ID, test ID, role, environment, generated record IDs, and expected transition.
- Retain screenshots, traces, videos, console errors, and logs where available.
- Classify the result as:
  - Product defect
  - Automation defect
  - Environment defect
  - Test-data defect
  - Integration/provider defect
  - PRD ambiguity
  - Known product drift
- Produce concise reproduction steps.
- Link confirmed fixed defects to their regression tests.
- Do not attach sensitive customer or document content unless sanitized and approved.

#### 11. PRD Drift Reporting

Compare implemented QA behaviour with the PRD and report:

- Missing modules or routes
- Changed roles or permissions
- Undocumented statuses or transitions
- Different validation rules
- Different notification timing
- Different recipient resolution
- Non-idempotent risk or notification behaviour
- Differences between staging and QA
- Features present in the product but absent from the PRD
- PRD features absent from the environment

Do not silently modify tests to accept undocumented drift.

#### 12. CI/CD and Suite Design

Define the following suites:

- `qa-smoke`
- `qa-critical-regression`
- `qa-document-regression`
- `qa-task-regression`
- `qa-rbac`
- `qa-bug-regression`
- `qa-api-integration`
- `qa-extended`
- `qa-stress` — separately scheduled and controlled
- `production-smoke-readonly`
- `production-sanity-readonly`

Execution rules:

- Run `qa-smoke` after deployment.
- Run critical regression according to release risk and CI capacity.
- Run bug-regression tests for affected modules before promotion.
- Run API/integration suites in QA or provider sandboxes.
- Run stress suites only by scheduled approval.
- Run Production suites only with the explicit Production-safe configuration.
- Prevent QA write suites from targeting Production through hard environment guards.
- Keep smoke suites fast, independent, and diagnostically clear.
- Quarantine a test only with a linked defect and review date.

### Required Agent Deliverables

Maintain:

- **PRD-to-Test Coverage Matrix**
- **Automation Suitability and Priority Matrix**
- **QA Repetitive-Checks Automation Backlog**
- **Manual Exploratory Test Charter Register**
- **Environment and Role Audit**
- **Route and Network/API Inventory**
- **RBAC Verification Matrix**
- **Synthetic Test Data Schema**
- **Automated Smoke and Regression Suites**
- **Bug Regression Register**
- **API and Integration Test Suite**
- **Controlled Stress Test Plan and Results**
- **Defect and PRD Drift Register**
- **Execution Report**
- **Known Limitations Register**
- **Production Smoke, Monitoring, and Rollback Checklist**
- **Production-Safety Register**

### Starter Commands

1. **“Audit all supplied RegComply QA login portals, identify the actual role and landing route for each account, and report inaccessible or ambiguous portals.”**
2. **“Create the initial QA repetitive-check automation backlog for authentication, RBAC, document upload, framework mapping, review, approval, acknowledgement, task creation, assignment, and status validation.”**
3. **“Build a fast QA deployment smoke suite covering portal availability, login, core navigation, Document Management access, Task Management access, and restricted-route denial.”**
4. **“Generate a PRD-to-test matrix that classifies each requirement as automated, manual exploratory, integration, stress, blocked, or not applicable.”**
5. **“Inspect the framework-mapped upload flow and identify stable UI and API checkpoints for repeated QA regression execution.”**
6. **“Implement the QA critical-regression flow from document upload through approval and acknowledgement, using synthetic data and safe cleanup.”**
7. **“Create a QA RBAC suite that verifies allowed actions, prohibited actions, direct-route denial, and API-level denial for every verified role.”**
8. **“Review the resolved defect list and convert each deterministic bug fix into a permanent module-level regression test.”**
9. **“Create the QA task-regression suite for manual task creation, department routing, missing-lead fallback, delegation, evidence completion, and parent-state calculation.”**
10. **“Design a separate API and integration suite for notification queues, Risk Register synchronization, RegWatch triggers, Google synchronization, and identity lifecycle.”**
11. **“Prepare a controlled QA stress plan for uploads, API requests, notification processing, and integration queues, including agreed limits and recovery assertions.”**
12. **“Create exploratory test charters for native editor quality, AI finding relevance, complex concurrency, unusual files, and newly introduced workflows.”**
13. **“Compare the current QA environment with the PRD and produce a drift register covering routes, roles, statuses, validations, notifications, and integrations.”**
14. **“Create a Production-safe smoke suite limited to authorized availability, login-page, read-only route, and dependency-health checks.”**
15. **“Create a Production monitoring checklist covering logs, error reports, release anomalies, customer feedback correlation, and escalation ownership.”**
16. **“Prepare a rollback-verification checklist that confirms application availability, previous-version restoration, critical route health, and absence of unintended data changes.”**
17. **“Prevent all state-changing, stress, and full-regression suites from targeting Production unless an explicit authorization flag and isolated test configuration are present.”**

---

## 9. Product Clarifications Required Before Full Automation

1. **Product Manager:** Is the product name for this module officially **RegComply**, **ComplianceOS**, or “RegComply powered by ComplianceOS”? The PRD and supplied environment naming are inconsistent.
2. **Engineering Lead:** What are the exact QA and staging routes for Document Management, Task Management, repository, document detail, framework task detail, notifications, and risk views?
3. **Product Manager:** How do the supplied environment accounts—RegTech Admin, Organisation Admin, Client, and Audit Firm—map to the PRD roles of Compliance Manager, Admin, Analyst, Internal Auditor, Department Lead, and External Auditor?
4. **Product Manager:** The PRD states that six roles govern the module, but the internal permission table shows five columns and describes External Auditor separately. What is the authoritative role inventory?
5. **Engineering Lead:** Are role restrictions enforced at both the UI and API levels?
6. **Product Manager:** Can a Department Lead hold any base role, including Internal Auditor, or are some base-role/designation combinations prohibited?
7. **Product Manager:** Which Compliance Manager change-management settings may override default permissions, and can an override grant a normally prohibited capability?
8. **Engineering Lead:** What authentication and session model is used for each portal, including session expiry, refresh, logout, lockout, and password-reset behaviour?
9. **Engineering Lead:** Is the shared QA global-client URL expected to route Organisation Admin, Client, and Audit Firm accounts into different applications or tenant contexts?
10. **Engineering Lead:** What are the supported maximum file sizes, filename rules, MIME validation rules, malware-scanning requirements, and behaviour for encrypted or corrupted files?
11. **Product Manager:** Is `unclassified` a document status, a repository classification, or a filtered view? It is not included in the formal document status model.
12. **Product Manager:** The upload lifecycle includes `acknowledged` and `rejected`, while the data-model sketch lists `draft`, `in_review`, `approved`, and `archived`. What is the authoritative document status enumeration?
13. **Product Manager:** After rejection, is `rejected` persisted as a historical workflow outcome while the document returns to `draft`, or is it an actual current document state?
14. **Product Manager:** Does a document become globally `acknowledged` only when every required user acknowledges it, and what state applies when some users remain overdue?
15. **Product Manager:** Can an approver approve before all reviewers mark their reviews complete, given that review and approval run in parallel?
16. **Product Manager:** Can a reviewer formally reject or request changes, or may reviewers only comment and mark review complete?
17. **Product Manager:** Can the single approver be replaced, delegated, or removed after submission, and how is that change audited?
18. **Product Manager:** The acknowledgement list is described as frozen, but the creator may explicitly add new staff. Who may amend the list, at what stages, and does amendment restart or extend the deadline?
19. **Product Manager:** Who sets review, approval, and acknowledgement deadlines? Is there a platform default, framework default, document-type default, or per-document override?
20. **Product Manager:** What are the reminder rules for approval and acknowledgement before they become overdue?
21. **Engineering Lead:** What exact event or scheduler evaluates T-7, T-1, daily overdue, acknowledgement, approval, and evidence-gap deadlines?
22. **Engineering Lead:** What is the retry and idempotency contract for in-app notifications and outbound email?
23. **Product Manager:** Which notification categories are compliance-critical and therefore non-configurable?
24. **Product Manager:** What is the approved semantic-similarity threshold and embedding model for repetition detection?
25. **Engineering Lead:** Is content similarity synchronous during upload or asynchronous, and what happens when the similarity service is unavailable?
26. **Product Manager:** Who can confirm AI-suggested mappings? The mapping narrative says “the user or Compliance Manager,” while the RBAC table limits mapping to Compliance Manager, Admin, and Analyst.
27. **Product Manager:** What confidence, evidence, or explainability requirements apply before an AI cross-framework recommendation is shown?
28. **Engineering Lead:** Should AI findings display only after full batch completion or stream progressively?
29. **Product Manager:** How are failed, delayed, or incomplete AI evaluations shown, retried, and audited?
30. **Product Manager:** Does the 30-day gap grace period begin when the framework is registered, when the clause task is activated, when the prior evidence expires, or when a mapping becomes unsatisfied?
31. **Engineering Lead:** How frequently are gap states recalculated, and what events trigger immediate recalculation?
32. **Engineering Lead:** What key guarantees idempotent Risk Register creation for repeated deadline or gap evaluations?
33. **Product Manager:** Can a Compliance Manager manually close or suppress an auto-generated risk while the originating condition remains unresolved?
34. **Product Manager:** When the same source is overdue in more than one way, should the system create separate risks or consolidate them?
35. **Product Manager:** What is the authoritative precedence between parent task states `at_risk` and `overdue` when both a sub-task and the parent due date are overdue?
36. **Product Manager:** When a task is assigned to a role, is one shared task assigned to all role members, or is a separate task instance created per user?
37. **Product Manager:** What happens to an active task when its assignee, Department Lead, role member, or department is removed?
38. **Product Manager:** How are recurring tasks generated, and what prevents duplicate recurrence instances?
39. **Product Manager:** What fields and validation rules apply to non-document attestations?
40. **Product Manager:** Which record file types are accepted for non-document tasks, and can one task require multiple evidence files?
41. **Product Manager:** Under what conditions must a non-document record enter the full review, approval, and acknowledgement workflow?
42. **Engineering Lead:** What technical editor or conversion service supports DOCX, XLSX, and PPTX, and what fidelity limitations are accepted?
43. **Product Manager:** What custom version-label formats are permitted, and must labels be unique per document?
44. **Product Manager:** A new version is shown as returning to `draft`, while a major change is described as returning to `in_review`. What is the exact transition after a major-version save?
45. **Product Manager:** Who is permitted to classify a change as major or minor, and can Compliance Manager or Admin override the editor’s classification?
46. **Engineering Lead:** What approved conflict strategy governs simultaneous native and Google editing: pessimistic lock, optimistic concurrency, or another model?
47. **Engineering Lead:** What Google event or polling mechanism triggers synchronization, and what is the expected synchronization latency?
48. **Engineering Lead:** Does Google Workspace edit mode support DOCX, XLSX, and PPTX only, or native Google file formats as well?
49. **Engineering Lead:** The PRD names SAML/OIDC for identity and provisioning/deprovisioning behaviour. Is SCIM or another provisioning protocol implemented?
50. **Product Manager:** Which identity-provider group mappings may assign base roles and which may assign only Department Lead designation?
51. **Engineering Lead:** What RegWatch event contract triggers task regeneration, and how is duplicate circular processing prevented?
52. **Product Manager:** What review UX applies to AI-generated tasks: individual review, bulk approval, or hybrid?
53. **Product Manager:** Can AI-generated tasks be permanently deleted before activation, or must dismissal remain auditable?
54. **Product Manager:** What dashboard calculations define evidence gaps, acknowledgement completion, task completion, overdue counts, and risk totals?
55. **Engineering Lead:** What are the API, webhook, event, and background-job contracts for Google Workspace, email, RegWatch, Risk Register, AI, and Audit Firm access?
56. **Product Manager:** What exact event closes an Audit Firm engagement and revokes access, and is there a grace period or evidence-retention requirement?
57. **Engineering Lead:** Are signed document links or cached files invalidated immediately when external access is revoked?
58. **Product Manager:** What browsers, viewport sizes, accessibility standard, and assistive technologies must be supported?
59. **Product Manager:** What response-time, search-latency, upload-processing, AI-evaluation, notification-delivery, and synchronization thresholds define acceptance?
60. **Engineering Lead:** What seeded test data, reset API, cleanup mechanism, mail sandbox, fake clock, and integration sandbox are available for deterministic automation?
61. **Engineering Lead:** Are stable `data-*` test attributes available for critical controls and dynamic records?
62. **Security Owner:** Is automated use of the supplied production account authorized, and which specific read-only checks are permitted?
63. **Security Owner:** Should the credentials currently supplied in planning documents be rotated and moved into an approved secret manager before framework implementation?
64. **QA Lead:** Which repetitive checks are mandatory after every QA deployment, and which may run only before a release candidate is promoted?
65. **QA Lead:** What is the approved execution cadence for QA smoke, critical regression, bug regression, API/integration, extended regression, and controlled stress suites?
66. **Engineering Lead:** What concurrency, throughput, payload volume, and duration limits are approved for QA API and integration stress testing?
67. **Engineering Lead:** Is the QA environment isolated enough for destructive cleanup, queue saturation, failed-provider simulation, and repeated notification testing?
68. **QA Lead:** What defect-management system and mandatory evidence fields should automation use when documenting failures?
69. **Operations Lead:** Which Production logs, dashboards, alerting tools, and error-reporting systems are available to QA for post-release monitoring?
70. **Operations Lead:** What exact Production smoke and sanity checks are authorized, and which accounts or tenants are approved for them?
71. **Release Manager:** What rollback scenarios must QA support, and what constitutes successful rollback verification?
72. **Customer Support Lead:** Where is customer feedback recorded, and how should QA correlate reported issues with releases, logs, defects, and regression coverage?
73. **Security Owner:** What technical guard must prevent QA regression, integration-stress, or state-changing suites from accidentally targeting Production?
