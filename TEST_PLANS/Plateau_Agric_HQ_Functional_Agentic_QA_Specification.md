# Plateau Agric HQ — Functional and Agentic QA Specification

- **Source:** Plateau Agric HQ Platform — Comprehensive Role Distribution & Access Control Specification
- **Product Type:** Government agricultural administration, governance, procurement, finance, farmer-data, allocation, audit, and reporting control platform
- **Specification Status:** Draft — derived from the supplied role and access-control specification; no browser, API, audit-log, network, or database verification has been performed
- **Scope Note:** Covers transitional setup governance, permanent institutional roles, maker–checker controls, procurement and budget approvals, farmer-data validation, allocation administration, monitoring and reporting, role-based dashboards, status locking, role modification, immutable audit records, and Super Admin decommissioning. Detailed procurement forms, budget formulas, farmer-registration fields, allocation rules, report calculations, and external integrations are outside the supplied specification unless directly required to validate role or workflow enforcement.
- **Environment Note:** One staging HQ URL and one HQ email address are supplied. No password, secondary role accounts, API base URL, Production URL, 2FA method, audit-log access details, or test data are supplied.
- **Automation Test Basis:** Automation candidates are derived from the role-permission matrices, maker–checker rules, separation-of-duties controls, workflow transitions, immutable audit requirements, UI/API access rules, and role-decommission conditions. Implementation priority is filtered by repetition, determinism, governance impact, execution safety, and environment suitability.
- **Environment Execution Policy:** Staging/QA is the primary environment for automated RBAC, approval, workflow, status-locking, negative, bug-regression, API authorization, and controlled concurrency testing. Manual effort remains focused on exploratory governance testing, ambiguous approval thresholds, usability, complex role combinations, and security investigation. Production must be limited to authorized monitoring, smoke, sanity, and rollback-support checks.

---

## 1. Platform Access and Roles

### QA / Staging Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Plateau Agric HQ Staging | HQ user — exact role requires confirmation | `https://plateau-agri-control-staging-197301616810.us-central1.run.app/` | `uzor.nwagu@opexconsult.co.uk` | `<ENTER_HQ_PASSWORD>` | `PLATEAU_AGRIC_HQ_STAGING_URL`, `PLATEAU_AGRIC_HQ_EMAIL`, `PLATEAU_AGRIC_HQ_PASSWORD` |
| Transitional Super Admin | Temporary setup role | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_TRANSITIONAL_ADMIN_EMAIL>` | `<ENTER_TRANSITIONAL_ADMIN_PASSWORD>` | `PLATEAU_AGRIC_HQ_TRANSITIONAL_EMAIL`, `PLATEAU_AGRIC_HQ_TRANSITIONAL_PASSWORD` |
| Executive Oversight Admin | High-risk approval authority | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_EXECUTIVE_EMAIL>` | `<ENTER_EXECUTIVE_PASSWORD>` | `PLATEAU_AGRIC_HQ_EXECUTIVE_EMAIL`, `PLATEAU_AGRIC_HQ_EXECUTIVE_PASSWORD` |
| Finance & Procurement Authority | Financial checker | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_FINANCE_EMAIL>` | `<ENTER_FINANCE_PASSWORD>` | `PLATEAU_AGRIC_HQ_FINANCE_EMAIL`, `PLATEAU_AGRIC_HQ_FINANCE_PASSWORD` |
| Audit & Compliance Authority | Independent read-only oversight | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_AUDIT_EMAIL>` | `<ENTER_AUDIT_PASSWORD>` | `PLATEAU_AGRIC_HQ_AUDIT_EMAIL`, `PLATEAU_AGRIC_HQ_AUDIT_PASSWORD` |
| Platform Operations Admin | User and workflow administration | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_OPERATIONS_EMAIL>` | `<ENTER_OPERATIONS_PASSWORD>` | `PLATEAU_AGRIC_HQ_OPERATIONS_EMAIL`, `PLATEAU_AGRIC_HQ_OPERATIONS_PASSWORD` |
| Procurement Officer | Procurement maker | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_PROCUREMENT_EMAIL>` | `<ENTER_PROCUREMENT_PASSWORD>` | `PLATEAU_AGRIC_HQ_PROCUREMENT_EMAIL`, `PLATEAU_AGRIC_HQ_PROCUREMENT_PASSWORD` |
| Data & Registration Admin | Farmer-data validation authority | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_DATA_ADMIN_EMAIL>` | `<ENTER_DATA_ADMIN_PASSWORD>` | `PLATEAU_AGRIC_HQ_DATA_ADMIN_EMAIL`, `PLATEAU_AGRIC_HQ_DATA_ADMIN_PASSWORD` |
| Allocation & Distribution Admin | Input-allocation authority | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_ALLOCATION_EMAIL>` | `<ENTER_ALLOCATION_PASSWORD>` | `PLATEAU_AGRIC_HQ_ALLOCATION_EMAIL`, `PLATEAU_AGRIC_HQ_ALLOCATION_PASSWORD` |
| Monitoring & Reporting Officer | Analytics and reporting | `<ENTER_HQ_STAGING_URL_OR_ROUTE>` | `<ENTER_REPORTING_EMAIL>` | `<ENTER_REPORTING_PASSWORD>` | `PLATEAU_AGRIC_HQ_REPORTING_EMAIL`, `PLATEAU_AGRIC_HQ_REPORTING_PASSWORD` |

**QA execution policy:** Use staging or a dedicated QA tenant for automated smoke, RBAC, maker–checker, workflow, status-locking, role-modification, audit-log, bug-regression, API authorization, and controlled concurrency tests. Use synthetic procurement, budget, farmer, allocation, role, and report records. Every repeatable governance defect should become a permanent regression test after the fix is validated.

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Plateau Agric HQ Production | Authorized HQ role | `<ENTER_PRODUCTION_HQ_URL>` | `<ENTER_PRODUCTION_HQ_EMAIL>` | `<ENTER_PRODUCTION_HQ_PASSWORD>` | `PLATEAU_AGRIC_HQ_PROD_URL`, `PLATEAU_AGRIC_HQ_PROD_EMAIL`, `PLATEAU_AGRIC_HQ_PROD_PASSWORD` |

**Production execution policy:** Production is the live government control environment. Use logs, alerts, approval failures, audit exceptions, and user reports as the primary defect signals. Automated execution must be limited to authorized, non-destructive smoke, health, and sanity checks. Do not create procurement requests, modify budgets, alter roles, approve transactions, validate farmers, allocate inputs, generate official reports, delete users, decommission roles, or run stress tests in Production without written authorization, isolated synthetic records, and an approved rollback and cleanup plan.

---

## 2. Feature List

### A. Role Architecture and Lifecycle

- **Transitional Super Admin:** Exists only during system initialization.
- **Permanent Governance Roles:** Executive Oversight Admin, Finance & Procurement Authority, and Audit & Compliance Authority.
- **Permanent Operational Roles:** Platform Operations Admin, Procurement Officer, Data & Registration Admin, Allocation & Distribution Admin, and Monitoring & Reporting Officer.
- **Role Count:** One temporary role and eight permanent roles, for a total of nine roles during setup.
- **Post-Setup State:** Transitional Super Admin must be deactivated after institutional roles become active.
- **Reactivation Restriction:** Transitional Super Admin cannot be reactivated without formal authorization.
- **Role Modification Control:** Role modification requires dual authorization.
- **Role-Based Dashboards:** Each role receives a dashboard aligned to permitted modules and actions.

### B. Global Governance and Separation of Duties

- **No Self-Approval:** No user may initiate and approve the same transaction.
- **Maker–Checker:** All critical transactions require separate initiation and approval actors.
- **Dual Authorization:** Role modification requires two authorized parties.
- **Restricted Deletion:** Delete permissions are highly restricted.
- **Immutable Audit Logs:** Audit records cannot be modified or deleted.
- **Status-Based Locking:** Transactions follow Draft, Submitted, and Approved states, with editing restricted by state.
- **UI Enforcement:** Controls must be hidden or disabled where permission is false.
- **Route Enforcement:** Direct URL access to unauthorized modules must be blocked.
- **API Enforcement:** APIs must enforce the same restrictions as the UI.
- **Audit Capture:** Every governed action records User ID, Role, Timestamp, IP address, MAC ID, Previous value, and New value.

### C. Transitional Super Admin

- Can view, create, modify, approve, and delete Users, Roles, and System Configuration during setup.
- Can view Procurement and Finance but cannot create, modify, approve, or delete their records.
- Must be decommissioned after permanent roles are active.
- Cannot authenticate after decommissioning.
- Cannot be reactivated without formal authorization.

### D. Executive Oversight Admin

- Can view all modules.
- Approves high-value procurement, budget ceilings, major configuration changes, and role restructuring.
- Has limited creation and modification rights requiring clarification.
- Cannot enter procurement transactions.
- Cannot modify audit logs.

### E. Finance & Procurement Authority

- Can view and approve procurement but cannot create or modify procurement entries.
- Can view, create, modify, and approve budget records subject to no-self-approval.
- Can view, create, and modify reports but cannot approve them.
- Cannot delete financial records.
- Acts as procurement checker.

### F. Audit & Compliance Authority

- Can view all operational data and audit logs.
- Can create audit or compliance reports only.
- Cannot create, modify, approve, or delete operational data.
- Cannot approve any transaction.
- Must remain independent from maker and checker operations.

### G. Platform Operations Admin

- Can view, create, and modify users.
- Can approve routine user actions where permitted.
- Can view, create, modify, and approve workflow configuration.
- Can view procurement but cannot create, modify, approve, or delete it.
- Cannot approve financial disbursement or modify audit logs.

### H. Procurement Workflow

- Procurement Officer creates procurement requests.
- Procurement Officer can modify only while status is Draft.
- Submission locks the request against further maker editing.
- Procurement Officer cannot approve or delete a procurement request.
- Finance reviews and approves submitted procurement.
- Executive Oversight approves procurement above the configured threshold.
- Audit can view but cannot modify or approve.

### I. Farmer Data Governance

- Data & Registration Admin can view, create, modify, and approve farmer data.
- Validates farmer registrations uploaded by external agents.
- Can perform authorized corrections.
- Cannot delete an approved farmer.
- Cannot modify financial records.
- Validated farmers proceed to Allocation & Distribution Admin.

### J. Allocation & Distribution Governance

- Allocation & Distribution Admin can view, create, modify, and approve allocations.
- Can view redemption reports only.
- Cannot change procurement cost or budget data.
- Assigns validated farmers to input allocations or distribution programs.

### K. Monitoring, Reporting, and Dashboards

- Monitoring & Reporting Officer can view, create, and modify reports and dashboards.
- Cannot approve reports, dashboards, or operational transactions.
- Cannot modify farmer, procurement, finance, or allocation records.
- Reporting configuration must not mutate underlying operational data.

### L. Budget Adjustment Workflow

- Finance proposes a budget adjustment.
- Executive Oversight approves it.
- Audit monitors the transaction and audit trail.
- The Finance maker cannot complete final approval.
- Submitted and approved adjustments are locked against unauthorized modification.

---

## 3. User Stories

> **As a Transitional Super Admin,** I want to create institutional users and roles during setup, so that the platform can transition into permanent governance.

> **As an Executive Oversight Admin,** I want to approve high-risk transactions and structural changes, so that strategic decisions receive senior authorization.

> **As a Finance & Procurement Authority user,** I want to approve procurement created by another user, so that costs receive independent validation.

> **As an Audit & Compliance Authority user,** I want read-only access to operational records and immutable logs, so that oversight remains independent.

> **As a Platform Operations Admin,** I want to manage users and workflows without financial approval rights, so that operational administration remains separated from finance.

> **As a Procurement Officer,** I want to edit requests only while they are Draft, so that submitted records remain controlled.

> **As a Data & Registration Admin,** I want to validate and correct farmer records, so that only accurate data proceeds to allocation.

> **As an Allocation & Distribution Admin,** I want to assign validated farmers to input allocations, so that distribution follows approved data.

> **As a Monitoring & Reporting Officer,** I want to create dashboards and reports without changing operational data, so that analytics remains independent from execution.

> **As a Governance Owner,** I want the Transitional Super Admin decommissioned after setup, so that temporary concentration of privilege does not persist.

> **As an Auditor,** I want every sensitive action to retain actor, role, timestamp, network identity, and before-and-after values, so that changes remain traceable.

---

## 4. Core User Flows in Gherkin Syntax

These scenarios are intended primarily for staging or QA. Repetitive, deterministic governance checks form the automation backlog. Ambiguous thresholds, unusual role combinations, usability, and policy interpretation remain manual until objectively defined.

### Flow 1: Authentication and Role-Based Dashboard

```gherkin
Feature: Plateau Agric HQ role-based access

  Scenario Outline: An institutional role signs in successfully
    Given the <role> account is active
    And the portal URL and credentials are loaded from environment variables
    When the user submits valid credentials
    Then an authenticated session is established
    And the role-appropriate dashboard is displayed
    And prohibited modules and actions are unavailable
    And credentials and tokens are not exposed in logs

    Examples:
      | role                            |
      | Executive Oversight Admin       |
      | Finance & Procurement Authority |
      | Audit & Compliance Authority    |
      | Platform Operations Admin       |
      | Procurement Officer             |
      | Data & Registration Admin       |
      | Allocation & Distribution Admin |
      | Monitoring & Reporting Officer  |

  Scenario: A role opens a prohibited route directly
    Given the user is authenticated
    And the role lacks permission for the target module
    When the user navigates directly to the route
    Then access is denied
    And protected data is not returned
    And no operational record is changed
```

### Flow 2: Procurement Maker–Checker

```gherkin
Feature: Procurement separation of duties

  Scenario: Procurement Officer creates and submits a request
    Given the Procurement Officer is authenticated
    When the officer creates a valid procurement request
    Then the request is saved in "Draft"
    And the officer can modify it
    When the officer submits the request
    Then the status changes to "Submitted"
    And the request is locked against further maker modification
    And the request becomes available to Finance

  Scenario: The maker attempts to approve the submitted request
    Given a Procurement Officer created a submitted request
    When the same user attempts to approve it through the UI or API
    Then approval is denied
    And the request remains unapproved
    And the denied attempt is audit-logged
```

### Flow 3: Procurement Approval and Escalation

```gherkin
Feature: Procurement approval routing

  Scenario: Finance approves procurement below the Executive threshold
    Given a submitted request is below the configured threshold
    When an authorized Finance user approves it
    Then the Finance approval is recorded
    And the request enters the configured approved state
    And the Procurement Officer cannot modify it

  Scenario: High-value procurement requires Executive approval
    Given a request exceeds the configured threshold
    And Finance has completed its review
    When Finance approves its stage
    Then the request is routed to Executive Oversight
    And final approval is not recorded before Executive action
    When Executive Oversight approves it
    Then the final approval is recorded
    And both approval actors remain visible in the audit trail
```

### Flow 4: Audit Read-Only Enforcement

```gherkin
Feature: Independent audit access

  Scenario: Audit views operational data
    Given the Audit & Compliance Authority user is authenticated
    When the user opens procurement, finance, farmer, allocation, or reporting records
    Then the records are available in read-only mode
    And audit-log access is available

  Scenario Outline: Audit attempts to change operational data
    Given the Audit & Compliance Authority user is authenticated
    When the user attempts to <action> an operational record through the UI or API
    Then the action is denied
    And the underlying record remains unchanged
    And the denied attempt is logged

    Examples:
      | action  |
      | create  |
      | modify  |
      | approve |
      | delete  |
```

### Flow 5: Farmer Validation and Allocation Handoff

```gherkin
Feature: Farmer registration governance

  Scenario: Data Admin validates an uploaded farmer record
    Given an external Agent uploaded a farmer record
    And the Data & Registration Admin is authenticated
    When the Data Admin approves the record
    Then the farmer status reflects successful validation
    And the actor and timestamp are recorded
    And the farmer becomes eligible for allocation
    And the Data Admin cannot delete the approved farmer

  Scenario: Allocation Admin assigns a validated farmer
    Given a farmer record is validated
    And the Allocation & Distribution Admin is authenticated
    When the admin creates an allocation
    Then the allocation is stored
    And the farmer-to-allocation relationship is recorded
    And procurement cost and budget records remain unchanged
```

### Flow 6: Budget Adjustment Maker–Checker

```gherkin
Feature: Budget adjustment governance

  Scenario: Finance proposes a budget adjustment
    Given the Finance & Procurement Authority user is authenticated
    When the user creates and submits a valid adjustment proposal
    Then the proposal records the Finance user as maker
    And it is routed to Executive Oversight
    And the maker cannot complete final approval

  Scenario: Executive Oversight approves the proposal
    Given a submitted budget adjustment was created by Finance
    When an Executive Oversight user approves it
    Then final approval is recorded
    And Audit can view the complete history
```

### Flow 7: Status-Based Locking

```gherkin
Feature: Transaction state enforcement

  Scenario Outline: Editing rights change with transaction status
    Given a transaction exists in <status>
    And the user is the original maker
    When the user attempts to modify it
    Then the system applies <expected_result>

    Examples:
      | status    | expected_result                                  |
      | Draft     | modification is permitted according to role rules |
      | Submitted | modification is denied                            |
      | Approved  | modification is denied                            |
```

### Flow 8: Transitional Super Admin Decommissioning

```gherkin
Feature: Temporary super-admin lifecycle

  Scenario: Transitional Super Admin is decommissioned after setup
    Given all permanent institutional roles are active
    And setup completion is formally confirmed
    When the authorized decommission process completes
    Then the Transitional Super Admin becomes inactive
    And the role cannot authenticate
    And the decommission action is audit-logged
    And permanent roles remain active

  Scenario: Unauthorized reactivation is blocked
    Given the Transitional Super Admin is decommissioned
    When a user attempts reactivation without formal authorization
    Then reactivation is denied
    And the denied attempt is audit-logged
```

### Flow 9: Immutable Audit Trail

```gherkin
Feature: Governance audit logging

  Scenario: A governed update records complete audit metadata
    Given an authorized user changes a governed record
    When the transaction commits
    Then the audit entry records the user ID
    And the acting role
    And the timestamp
    And the IP address
    And the MAC ID where technically available
    And the previous value
    And the new value

  Scenario: A user attempts to alter an audit record
    Given an audit-log entry exists
    When any role attempts to modify or delete it
    Then the action is denied
    And the original record remains unchanged
```

---

## 5. Expected Outcomes and Success Measures

### Functional Outcomes

- No maker can approve the same transaction.
- Critical procurement, budget, role, and configuration actions follow the configured checker sequence.
- Procurement requests are editable in Draft and locked after submission.
- High-value procurement receives Executive approval where required.
- Audit users inspect records without changing operational data.
- Validated farmers become available for allocation.
- Allocation actions cannot modify procurement costs or budgets.
- Reporting changes cannot alter operational source data.
- Transitional Super Admin access ends after setup.
- Direct-route and API access cannot bypass permissions.
- Every governed state change preserves before-and-after values.

### Quantitative and Structural Rules

- **1 temporary role** during setup.
- **8 permanent roles** after setup.
- **9 total roles** during initialization.
- Critical transactions require at least **2 distinct actors**.
- Role modification requires **dual authorization**.
- Procurement requires maker creation, Finance review, Executive approval where threshold applies, and Audit visibility.
- Budget adjustment requires Finance proposal, Executive approval, and Audit monitoring.

### Security and Access Outcomes

- Unauthorized buttons are hidden or disabled.
- Direct URLs and APIs enforce RBAC.
- Audit cannot modify or approve transactions.
- Finance cannot create procurement.
- Procurement cannot approve its own entry or modify after submission.
- Data Admin cannot delete approved farmers.
- Allocation Admin cannot modify procurement cost or budget.
- Reporting cannot modify operational data.
- Production automation remains non-destructive unless separately authorized.

### Auditability Outcomes

- User ID, role, timestamp, IP, approved device identifier, previous value, and new value are recorded.
- Approval history preserves makers and checkers.
- Decommission and reactivation attempts remain traceable.
- Denied authorization attempts are logged where required.
- Role changes retain both authorizers.

### Measures Not Yet Defined

The specification does not define procurement thresholds, routine versus high-risk actions, approval deadlines, authentication and 2FA details, inactivity timeout, password rules, API response targets, dashboard refresh intervals, audit retention, MAC-address feasibility, concurrency limits, supported browsers, accessibility, or Production monitoring thresholds.

---

## 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
|---|---|---|
| Authentication | HQ login and sessions | Identity/session API; exact method requires inspection |
| Role Dashboards | Display modules by role | Front-end routing plus permission API |
| RBAC | UI, route, and API checks | Authorization middleware and role-permission store |
| Maker–Checker | Separate maker and checker | Workflow engine storing actors, states, and approvals |
| Dual Authorization | Two-party role modification | Role-governance workflow and approval records |
| Procurement | Draft, submit, Finance approval, Executive escalation | Procurement API, workflow orchestration, threshold rules |
| Budget Adjustments | Finance proposal and Executive approval | Finance API and approval workflow |
| Farmer Validation | External upload and Data Admin approval | Farmer registry API and status workflow |
| Allocation | Assign validated farmers | Allocation API and farmer eligibility data |
| Reporting | Dashboards and reports | Reporting service and aggregate-query APIs |
| Audit Logs | Immutable before-and-after records | Append-only audit or event store |
| IP Capture | Request source | Gateway or server request context |
| MAC/Device ID | Device identity where feasible | Trusted client or approved device-identity mechanism |
| Role Decommission | Disable temporary account | Identity and role-lifecycle API |
| Status Locking | Prevent edits after submission | Workflow-state authorization |
| Threshold Routing | Escalate high-value procurement | Configured rules engine |
| Monitoring | Errors and approval exceptions | Logs, monitoring, and alerting platform |

---

## 7. Integration-Based Gherkin Flows

These scenarios are for staging or QA. Authorization, concurrency, audit, and retry tests may be automated there. Stress testing must remain separate from normal browser regression and must not target Production.

### Integration Flow 1: API Self-Approval Prevention

```gherkin
Feature: Maker-checker enforcement at the API layer

  Scenario: The maker calls the approval API directly
    Given a user created a submitted transaction
    When the same user sends an approval request directly
    Then the API rejects the request
    And the transaction state remains unchanged
    And the denial is recorded
```

### Integration Flow 2: High-Value Procurement Routing

```gherkin
Feature: Procurement threshold workflow

  Scenario: Procurement above threshold is escalated
    Given a submitted request exceeds the configured threshold
    When Finance approves its stage
    Then the workflow activates Executive approval
    And final approval is not recorded before Executive action
```

### Integration Flow 3: Dual Authorization for Role Changes

```gherkin
Feature: Role-modification governance

  Scenario: One authorization is insufficient
    Given a role change is proposed
    When only one authorized approver confirms it
    Then the role change remains pending
    And the target user retains the previous role

  Scenario: Two distinct authorizers complete the change
    Given a role change is pending
    When both required authorizers approve it
    Then the target role is updated
    And both identities are stored in the audit history
```

### Integration Flow 4: Audit-Log Immutability

```gherkin
Feature: Append-only audit records

  Scenario: An operational update appends an audit event
    Given a governed record has an existing value
    When an authorized update commits
    Then an audit event records the previous and new values
    And links the event to the user, role, timestamp, and request identity

  Scenario: A client attempts audit update or deletion
    Given an audit event exists
    When any client sends a modify or delete request
    Then the service rejects it
    And the original event remains unchanged
```

### Integration Flow 5: Farmer Validation to Allocation

```gherkin
Feature: Cross-module farmer handoff

  Scenario: A validated farmer becomes available to allocation
    Given an external farmer record awaits validation
    When Data & Registration Admin approves it
    Then the Allocation module can retrieve the farmer as eligible
    And validation history remains intact
```

### Integration Flow 6: Role Decommission and Session Revocation

```gherkin
Feature: Transitional administrator decommissioning

  Scenario: Decommissioning revokes active and future access
    Given the Transitional Super Admin has an active session
    When decommissioning completes
    Then future authentication is denied
    And the active session is revoked according to policy
    And privileged requests using the former session are rejected
```

### Integration Flow 7: Concurrent Approval

```gherkin
Feature: Approval concurrency control

  Scenario: Two checkers approve the same stage concurrently
    Given one approval is required at the current stage
    And two eligible users load the record
    When both submit approval near-simultaneously
    Then only one effective transition is recorded
    And the second request receives the approved conflict response
```

### Integration Flow 8: Report Isolation

```gherkin
Feature: Reporting does not mutate operational data

  Scenario: Reporting Officer changes a dashboard configuration
    Given the officer can modify dashboards
    When a filter, layout, or report definition is saved
    Then the reporting configuration changes
    But procurement, finance, farmer, and allocation source data remains unchanged
```

---

## 8. System Instruction for the Plateau Agric HQ QA Automation Agent

### Role

You are the **Senior QA Automation Engineer for Plateau Agric HQ**. You design and maintain automated controls that remove repetitive QA effort across authentication, role-based access, maker–checker workflows, approvals, status locking, farmer validation, allocation, reporting, role lifecycle, and immutable audit logging.

Operate differently by environment:

- **Staging / QA:** Validate new governance features and bug fixes, run smoke and regression suites, test role matrices, exercise approval and denial paths, inspect APIs, test concurrency, and document defects.
- **Production:** Monitor logs, approval failures, authorization denials, audit exceptions, and user reports. Run only authorized smoke and sanity checks and support rollback verification.

Your objective is to automate repetitive, deterministic, governance-critical checks that must be rerun after deployments, permission changes, workflow changes, and bug fixes.

### Platform Overview

Plateau Agric HQ provides institutional governance for:

- User and role provisioning
- Temporary Super Admin setup and decommissioning
- Executive oversight
- Finance and procurement approvals
- Independent audit access
- Workflow and user administration
- Procurement initiation
- Farmer-data validation
- Input allocation
- Reporting and dashboards
- Maker–checker and dual authorization
- Immutable audit logging
- Role-based dashboards
- Status-based locking

### Automation Test Basis

Use:

1. Role permission matrices
2. Global governance rules
3. Transaction workflow matrix
4. Status transitions
5. Audit data requirements
6. Negative authorization paths
7. Confirmed defects and regression history
8. API and workflow contracts

### Automation Selection Rule

Automate a staging check when it is:

- Repeated after deployment, permission, or workflow changes
- Deterministic
- Governance- or security-critical
- Safe with synthetic records
- Objectively assertable through UI, API, workflow state, or audit log
- Supported by reliable setup and cleanup

Keep coverage manual or hybrid when it involves:

- Ambiguous approval thresholds
- Usability and role comprehension
- Novel role combinations
- Complex organizational-policy interpretation
- Security abuse exploration
- Undocumented emergency overrides
- Human review of report usefulness

Automate high-risk deterministic controls. Manually explore high-risk unpredictable governance behaviour.

### Environment and Secret Management

- Load URLs, usernames, passwords, tokens, role IDs, and test configuration from environment variables.
- Never hard-code secrets.
- Mask credentials, tokens, personal data, procurement values, and audit identifiers in public reports.
- Keep staging and Production isolated.
- Use synthetic procurement, budget, farmer, allocation, role, and reporting records.
- Require hard environment guards before any test targets Production.
- Do not modify real roles or approvals in Production.

### Core Responsibilities

#### 1. Requirement Traceability

- Convert each role, permission, rule, and workflow into testable behaviour.
- Assign stable IDs such as `RBAC-AUDIT-001`, `PROC-MC-002`, `ROLE-DECOM-001`, and `AUDIT-IMMUT-003`.
- Maintain requirement, role, module, environment, test type, automation suitability, automation status, manual coverage, last result, and defect or clarification dependency.

#### 2. QA Automation Priorities

**P0 — Smoke and Access**

- Portal availability
- Login
- Session handling
- Role dashboard
- Direct-route denial
- Critical API health

**P0 — RBAC Regression**

- View, create, modify, approve, and delete permissions per role
- Hidden or disabled controls
- Direct URL blocking
- API authorization
- Cross-module restrictions
- Audit read-only enforcement

**P0 — Maker–Checker Regression**

- Procurement maker versus checker
- Budget proposal versus approval
- No self-approval
- Executive threshold escalation
- Dual authorization for role changes
- Concurrent approval protection

**P1 — Workflow and Status Regression**

- Draft editing
- Submission locking
- Approval locking
- Farmer validation handoff
- Allocation restrictions
- Reporting isolation
- User and workflow administration boundaries

**P1 — Role Lifecycle**

- Transitional Super Admin setup rights
- Decommissioning
- Login denial after decommission
- Session revocation
- Unauthorized reactivation denial
- Permanent role continuity

**P1 — Audit Regression**

- Required metadata
- Previous and new values
- Denied-attempt logging
- Immutable records
- Approval-chain traceability

**P1 — Bug Regression**

- Convert deterministic fixed defects into permanent tests.

**P2 — API, Concurrency, and Resilience**

- API-level RBAC
- Approval idempotency
- Concurrent checkers
- Threshold routing
- Role-change authorization
- Session revocation
- Audit append reliability

#### 3. Manual and Exploratory Allocation

Maintain charters for:

- Role-dashboard usability
- Governance workflow comprehension
- Ambiguous “limited” permissions
- Emergency access and override behaviour
- Complex threshold combinations
- Unusual organization-role assignments
- Report usefulness
- Audit investigation usability
- Security abuse and privilege escalation

#### 4. Controlled Stress and Concurrency Testing

- Run only in staging or QA.
- Test concurrent approval, role modification, and high-volume audit-event creation using agreed limits.
- Verify one effective transition per approval stage.
- Verify audit records are not lost or duplicated.
- Never run stress against Production without separate authorization.

#### 5. Production Policy

Permitted:

- URL and login-page health
- Read-only route checks
- Monitoring and log review
- Approval-error monitoring
- Authorization-denial monitoring
- Audit-pipeline monitoring
- Release smoke
- Sanity checks
- Rollback verification

Prohibited by default:

- User or role creation
- Role modification
- Procurement creation or approval
- Budget changes
- Farmer validation
- Allocation
- Official report generation
- Super Admin decommission or reactivation
- Deletion
- Full regression
- Stress or concurrency testing

#### 6. Environment and Network Audit

- Inspect the HQ staging portal.
- Determine the role assigned to the supplied email.
- Locate all role-specific modules and routes.
- Inventory authentication, user, role, procurement, finance, farmer, allocation, report, and audit requests.
- Confirm UI and API enforcement.
- Determine workflow-state values and approval contracts.
- Request API documentation where contracts remain unclear.
- Record observations separately from specification requirements.

#### 7. Automation Framework Standards

- Use Cypress or Playwright according to repository convention.
- Use JavaScript or TypeScript according to project standards.
- Implement reusable login and role-session helpers.
- Use API-assisted setup where safe.
- Use stable `data-*` selectors.
- Avoid fixed waits.
- Synchronize against network and workflow state.
- Generate unique synthetic records.
- Make tests retry-safe and parallel-safe.
- Validate UI and API state for critical controls.
- Capture audit evidence for governance tests.
- Separate mocked tests from real staging integration tests.

#### 8. Negative and Edge-Case Coverage

Automate stable cases including:

- Invalid credentials
- Expired session
- Unauthorized route
- Unauthorized API
- Hidden-button bypass
- Maker self-approval
- Finance procurement creation
- Audit modification
- Procurement edit after submission
- Procurement edit after approval
- Data Admin deletion of approved farmer
- Allocation cost or budget modification
- Reporting operational-data modification
- Single-authorization role change
- Duplicate approval
- Unauthorized Super Admin reactivation
- Audit-log update or delete
- Missing audit metadata
- Concurrent checker approval
- Threshold boundary values

#### 9. API and Integration Verification

- Validate authentication, authorization, request handling, persistence, workflow state, idempotency, and audit events.
- Verify UI restrictions cannot be bypassed through direct requests.
- Verify maker identity differs from checker identity.
- Verify role changes require distinct authorizers.
- Verify audit events remain immutable.
- Verify reporting changes do not mutate source data.
- Verify session revocation after decommissioning.
- Do not invent undocumented endpoints or payload fields.

#### 10. Defect Evidence and Reporting

For each failure:

- Record requirement ID, test ID, role, environment, record identifier, and expected state.
- Capture UI state and masked request metadata.
- Capture before-and-after workflow state.
- Capture related audit entries.
- Classify as product, automation, environment, test-data, authorization, workflow, specification ambiguity, or product drift.
- Link resolved defects to regression tests.

#### 11. Specification Drift Reporting

Report:

- Missing roles
- Different permissions
- Undocumented permissions
- Different workflow states
- Missing maker–checker controls
- Missing role dual authorization
- Different threshold routing
- Editable submitted records
- Mutable audit logs
- Active Transitional Super Admin after setup
- UI/API permission mismatch
- Features present but undocumented
- Requirements absent from staging

#### 12. CI/CD and Suite Design

Define:

- `qa-smoke`
- `qa-rbac`
- `qa-maker-checker`
- `qa-procurement`
- `qa-budget`
- `qa-farmer-allocation`
- `qa-role-lifecycle`
- `qa-audit`
- `qa-bug-regression`
- `qa-api-concurrency`
- `qa-extended`
- `qa-stress`
- `production-smoke-readonly`
- `production-sanity-readonly`

Execution rules:

- Run smoke after deployment.
- Run RBAC and maker–checker before release promotion.
- Run affected regression after permission or workflow changes.
- Run concurrency separately in staging.
- Prevent all write suites from targeting Production.
- Quarantine only with a linked defect and review date.

### Required Agent Deliverables

Maintain:

- **Specification-to-Test Coverage Matrix**
- **Role and Permission Verification Matrix**
- **Maker–Checker Coverage Matrix**
- **Automation Suitability and Priority Matrix**
- **QA Repetitive-Checks Backlog**
- **Manual Exploratory Charter Register**
- **Environment and Role Audit**
- **Route and API Inventory**
- **Synthetic Governance Test Data Schema**
- **Automated Smoke and Regression Suites**
- **Bug Regression Register**
- **Concurrency Test Plan**
- **Audit Integrity Report**
- **Defect and Drift Register**
- **Execution Report**
- **Known Limitations Register**
- **Production Monitoring and Rollback Checklist**
- **Production-Safety Register**

### Starter Commands

1. **“Audit the Plateau Agric HQ staging URL, identify the role assigned to the supplied email, and report visible modules, routes, and missing permissions.”**
2. **“Create the repetitive-check automation backlog for login, RBAC, maker–checker, procurement, budget, farmer validation, allocation, role lifecycle, and audit logging.”**
3. **“Build a fast HQ staging smoke suite covering availability, login, dashboard loading, core routes, and direct-route denial.”**
4. **“Generate a complete role-permission test matrix for all nine setup roles and eight permanent roles.”**
5. **“Implement the Procurement Officer Draft-to-Submitted flow and verify the maker cannot edit or approve after submission.”**
6. **“Implement Finance and Executive procurement approval, including threshold escalation and no-self-approval checks.”**
7. **“Build an Audit & Compliance suite that verifies read-only access across every operational module at UI and API levels.”**
8. **“Test Data & Registration Admin farmer validation and the handoff to Allocation & Distribution Admin.”**
9. **“Test Platform Operations Admin user and workflow rights while confirming financial and audit restrictions.”**
10. **“Implement the budget-adjustment maker–checker flow from Finance proposal to Executive approval and Audit visibility.”**
11. **“Test dual authorization for role modification, including one-approver failure and two-approver success.”**
12. **“Test Transitional Super Admin decommissioning, session revocation, login denial, and unauthorized reactivation.”**
13. **“Validate required audit metadata and attempt audit-log modification and deletion using privileged roles.”**
14. **“Run controlled concurrent approval tests to verify one workflow stage cannot be approved twice.”**
15. **“Compare staging behaviour with the role and access specification and produce a drift register.”**
16. **“Convert resolved permission and workflow defects into permanent regression tests.”**
17. **“Build an authorized Production read-only suite limited to availability, login-page, route-health, and monitoring checks.”**
18. **“Create a rollback checklist covering authentication, role assignments, workflow states, approval continuity, and audit-log integrity.”**
19. **“Add hard environment guards preventing user, role, procurement, budget, farmer, allocation, approval, decommission, stress, and destructive suites from targeting Production.”**

---

## 9. Product Clarifications Required Before Full Automation

1. **Engineering Lead:** What is the password for the supplied HQ staging email?
2. **Product Manager:** Which role is assigned to `uzor.nwagu@opexconsult.co.uk`?
3. **Engineering Lead:** Are separate staging accounts available for all nine roles?
4. **Product Manager:** Is the Transitional Super Admin deleted, disabled, archived, or soft-deactivated after setup?
5. **Product Manager:** What exact condition or flag indicates setup completion?
6. **Product Manager:** Who may decommission the Transitional Super Admin?
7. **Product Manager:** What constitutes formal authorization for reactivation?
8. **Engineering Lead:** Are active sessions revoked immediately when a role is deactivated?
9. **Product Manager:** What permissions are included under Executive Oversight “Limited” create and modify rights?
10. **Product Manager:** What actions qualify as high risk?
11. **Product Manager:** What procurement threshold triggers Executive approval?
12. **Product Manager:** Is the threshold global, configurable, or program-specific?
13. **Product Manager:** Can Finance create and approve the same budget record, or must distinct users act?
14. **Product Manager:** Which budget transactions require Executive approval?
15. **Product Manager:** What does “routine” approval mean for Platform Operations user management?
16. **Product Manager:** Which user or workflow changes require dual authorization?
17. **Product Manager:** Who may serve as the two role-change authorizers?
18. **Product Manager:** Must authorizers be different roles as well as different users?
19. **Product Manager:** What happens when a role change is rejected?
20. **Product Manager:** What is the authoritative procurement status model?
21. **Product Manager:** Can submitted procurement return to Draft?
22. **Product Manager:** Can Finance reject or request amendment, and what state follows?
23. **Product Manager:** Can Executive reject after Finance approval?
24. **Product Manager:** Can approved procurement be cancelled or reversed?
25. **Product Manager:** What fields and validations constitute a procurement request?
26. **Product Manager:** What is the authoritative budget-adjustment state model?
27. **Product Manager:** Can Data Admin create farmer records directly or only validate Agent uploads?
28. **Product Manager:** What is the farmer validation state model?
29. **Product Manager:** What corrections may Data Admin make after approval?
30. **Product Manager:** What is the workflow for duplicate or fraudulent farmer records?
31. **Product Manager:** What allocation states and approval rules apply?
32. **Product Manager:** Can one Allocation Admin create and approve an allocation?
33. **Product Manager:** Does global no-self-approval apply to farmer validation and allocation?
34. **Product Manager:** What redemption-report data may Allocation Admin view?
35. **Product Manager:** What source data may Monitoring & Reporting use?
36. **Product Manager:** Can reports be published without approval?
37. **Product Manager:** Who approves official reports or dashboards?
38. **Engineering Lead:** What authentication, password policy, 2FA, and session timeout apply?
39. **Engineering Lead:** What are the API base URL and documentation locations?
40. **Engineering Lead:** Are UI and API permissions generated from one authorization source?
41. **Engineering Lead:** What response is returned for unauthorized access?
42. **Engineering Lead:** How are IP addresses captured behind proxies?
43. **Security Owner:** Is MAC ID technically available from the browser application?
44. **Security Owner:** If MAC ID is unavailable, what approved device identifier should be stored?
45. **Engineering Lead:** What technology or access policy enforces audit immutability?
46. **Product Manager:** How long are audit records retained?
47. **Product Manager:** Which denied actions must be audit-logged?
48. **Engineering Lead:** What idempotency or locking prevents duplicate approvals?
49. **Engineering Lead:** What outcome is expected when two checkers approve concurrently?
50. **Engineering Lead:** What seeded users and synthetic records exist in staging?
51. **Engineering Lead:** Is there a reset or cleanup mechanism for automated data?
52. **QA Lead:** Which checks must run after every deployment?
53. **QA Lead:** Which suites are mandatory before release promotion?
54. **Engineering Lead:** What limits are approved for audit-event and approval-concurrency stress testing?
55. **QA Lead:** What defect system and evidence fields should automation use?
56. **Operations Lead:** Which Production logs, authorization alerts, approval errors, and audit monitors are available?
57. **Operations Lead:** Which exact Production smoke and sanity checks are authorized?
58. **Release Manager:** What rollback scenarios must QA support?
59. **Product Manager:** What browsers, screen sizes, accessibility standard, and devices apply?
60. **Security Owner:** What hard control prevents staging write suites from targeting Production?
61. **Security Owner:** Should the supplied HQ account be moved into an approved secret manager before automation begins?
