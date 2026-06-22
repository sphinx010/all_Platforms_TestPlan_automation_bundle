# Plateau Agric V1 — Functional and Agentic QA Specification

- **Source:** Plateau Agric Functional Requirements Document (FRD), Version 1.0, dated October 2025
- **Product Type:** Integrated agricultural administration, farmer registration, cooperative, input distribution, field-agent, and government reporting platform
- **Specification Status:** Draft — derived from the FRD and supplied access details; no browser, device, API, network, SMS, or offline-storage verification has been performed
- **Scope Note:** Covers Admin HQ oversight, agent administration, online and offline farmer registration, farmer registry management, cooperative management, input allocation and redemption, dashboard analytics, reporting, SMS communication, audit logging, offline synchronization, and Governor’s Agricultural Dashboard integration. Loan-processing and other future interventions referenced only as possible uses of the farmer IDN are outside the defined V1 scope.
- **Environment Note:** Access is partial. A V1 client URL and an Agent Staging URL are supplied. Client credentials are described as the same as the PLACOM Farmer account, while Agent credentials are supplied separately. No Admin HQ URL, Admin credentials, Redemption Agent credentials, dedicated API base URL, SMS sandbox, USSD test access, Governor Dashboard endpoint, or confirmed Production access is supplied.
- **Automation Test Basis:** Automation candidates are derived from FRD-confirmed workflows, field validations, role boundaries, online/offline state transitions, synchronization rules, duplicate-detection behaviour, reporting requirements, and integration contracts. Implementation priority is filtered by repetition, determinism, business relevance, failure impact, execution safety, and environment suitability.
- **Environment Execution Policy:** QA or staging is the primary environment for automated regression, bug-fix validation, API and synchronization testing, negative testing, and controlled stress testing. Manual effort remains focused on exploratory testing, field usability, device permissions, real low-connectivity behaviour, unusual farmer data, and other high-risk scenarios requiring human judgment. Production must be limited to authorized monitoring, smoke, sanity, and rollback-support checks.

---

## 1. Platform Access and Roles

### QA / Staging Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Plateau Agric V1 Client | Farmer / Client account — role mapping requires confirmation | `https://plateau-agric-client-197301616810.us-central1.run.app/` | `07034456894` | `Oz12345678$` | `PLATEAU_AGRIC_QA_CLIENT_URL`, `PLATEAU_AGRIC_QA_CLIENT_PHONE`, `PLATEAU_AGRIC_QA_CLIENT_PASSWORD` |
| Plateau Agric Agent Staging | Registration Agent or Agent account — exact role requires confirmation | `https://plateau-agric-agent-staging-197301616810.us-central1.run.app/agent/` | `omari@mailinator.com` | `Oz12345678$` | `PLATEAU_AGRIC_QA_AGENT_URL`, `PLATEAU_AGRIC_QA_AGENT_EMAIL`, `PLATEAU_AGRIC_QA_AGENT_PASSWORD` |
| Admin HQ Portal | Super Admin / Program Admin / Data Officer | `<ENTER_ADMIN_HQ_QA_URL>` | `<ENTER_ADMIN_HQ_EMAIL>` | `<ENTER_ADMIN_HQ_PASSWORD>` | `PLATEAU_AGRIC_QA_ADMIN_URL`, `PLATEAU_AGRIC_QA_ADMIN_EMAIL`, `PLATEAU_AGRIC_QA_ADMIN_PASSWORD` |
| Redemption Agent Portal | Redemption Agent | `<ENTER_REDEMPTION_AGENT_QA_URL>` | `<ENTER_REDEMPTION_AGENT_EMAIL>` | `<ENTER_REDEMPTION_AGENT_PASSWORD>` | `PLATEAU_AGRIC_QA_REDEMPTION_URL`, `PLATEAU_AGRIC_QA_REDEMPTION_EMAIL`, `PLATEAU_AGRIC_QA_REDEMPTION_PASSWORD` |
| USSD Test Channel | Farmer | `<ENTER_USSD_TEST_CODE_OR_SIMULATOR>` | `<ENTER_TEST_MSISDN>` | `<ENTER_USSD_PIN_IF_APPLICABLE>` | `PLATEAU_AGRIC_QA_USSD_CODE`, `PLATEAU_AGRIC_QA_FARMER_MSISDN`, `PLATEAU_AGRIC_QA_USSD_PIN` |
| SMS Sandbox | Farmer / Agent notification recipient | `<ENTER_SMS_SANDBOX_OR_INBOX_URL>` | `<ENTER_SMS_TEST_ACCOUNT>` | `<ENTER_SMS_TEST_SECRET>` | `PLATEAU_AGRIC_QA_SMS_URL`, `PLATEAU_AGRIC_QA_SMS_ACCOUNT`, `PLATEAU_AGRIC_QA_SMS_SECRET` |
| Governor’s Agricultural Dashboard | Government reporting consumer | `<ENTER_GOVERNOR_DASHBOARD_QA_URL>` | `<ENTER_DASHBOARD_TEST_USER>` | `<ENTER_DASHBOARD_TEST_PASSWORD>` | `PLATEAU_AGRIC_QA_GOV_DASHBOARD_URL`, `PLATEAU_AGRIC_QA_GOV_DASHBOARD_USER`, `PLATEAU_AGRIC_QA_GOV_DASHBOARD_PASSWORD` |

**QA execution policy:** Run automated smoke, critical regression, role and permission, bug-regression, API, synchronization, notification, negative, and controlled stress suites in QA or staging. Use manual testing for real-device usability, camera and GPS behaviour, offline field conditions, visual quality, accessibility, USSD interaction quality, and other scenarios that remain unstable or subjective. Every confirmed repeatable defect should become a regression candidate after the fix is validated.

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| Production Client Portal | Farmer / Client | `<ENTER_PRODUCTION_CLIENT_URL>` | `<ENTER_PRODUCTION_CLIENT_USERNAME>` | `<ENTER_PRODUCTION_CLIENT_PASSWORD>` | `PLATEAU_AGRIC_PROD_CLIENT_URL`, `PLATEAU_AGRIC_PROD_CLIENT_USERNAME`, `PLATEAU_AGRIC_PROD_CLIENT_PASSWORD` |
| Production Agent Portal | Registration / Redemption Agent | `<ENTER_PRODUCTION_AGENT_URL>` | `<ENTER_PRODUCTION_AGENT_USERNAME>` | `<ENTER_PRODUCTION_AGENT_PASSWORD>` | `PLATEAU_AGRIC_PROD_AGENT_URL`, `PLATEAU_AGRIC_PROD_AGENT_USERNAME`, `PLATEAU_AGRIC_PROD_AGENT_PASSWORD` |
| Production Admin HQ | Admin HQ | `<ENTER_PRODUCTION_ADMIN_URL>` | `<ENTER_PRODUCTION_ADMIN_USERNAME>` | `<ENTER_PRODUCTION_ADMIN_PASSWORD>` | `PLATEAU_AGRIC_PROD_ADMIN_URL`, `PLATEAU_AGRIC_PROD_ADMIN_USERNAME`, `PLATEAU_AGRIC_PROD_ADMIN_PASSWORD` |

**Production execution policy:** Production is the live operational environment. Use application logs, error monitoring, SMS delivery reports, synchronization failures, and customer or field-agent feedback as the principal defect signals. Automated execution must be limited to authorized, lightweight, non-destructive smoke, health, and sanity checks. Do not run routine farmer registration, input allocation, redemption, bulk messaging, record merge, deletion, offline sync, stress, or full regression tests against Production without written authorization, isolated synthetic accounts, and an approved cleanup and rollback plan.

---

## 2. Feature List

### A. Identity, Authentication, and Role Governance

- **Admin Authentication:** Admin HQ users authenticate using government-issued email and password credentials or an approved SSO method.
- **Administrative Roles:** The FRD identifies Super Admin, Program Admin, and Data Officer roles but does not define their detailed permission differences.
- **Administrative 2FA:** Two-factor authentication is required for Admin HQ access.
- **Agent Authentication:** Registration and Redemption Agents authenticate using credentials issued by Admin HQ.
- **Token-Based Agent Session:** Agent authentication uses JWT or an equivalent token model.
- **Offline Session Persistence:** A Registration Agent may continue using a cached authentication token offline for up to 14 days before renewal is required.
- **Redemption Connectivity Restriction:** Redemption Agents require online access to perform redemption.
- **Role-Based Access Control:** Administrative actions, agent management, registry operations, allocation, reporting, and communication functions must be restricted by role.
- **Farmer Access:** Farmers interact through mobile, SMS, USSD, or mobile web channels.
- **Suspension and Deletion:** Admin HQ may suspend, delete, or reassign agents. The exact lifecycle and historical-record effects require clarification.

### B. Admin Dashboard and Oversight

- **Farmer Metrics:** Display total registered farmers.
- **Geographic Breakdown:** Display farmers by LGA and Ward.
- **Agent Metrics:** Display active agents and performance indicators.
- **Cooperative Metrics:** Display the number of registered cooperatives.
- **Input Distribution Metrics:** Display allocation and redemption statistics.
- **Demographic and Activity Graphs:** Display gender, crop, and activity breakdowns.
- **Real-Time Monitoring:** Redemption progress and field activity should be visible to Admin HQ.
- **Government Reporting:** Summarized agricultural data feeds the Governor’s Agricultural Dashboard.
- **Trend Analysis:** Reports include farmer population trends, yield projections, redemption rates, and comparisons across LGAs and programs.

### C. Agent Management

- **Agent Invitation:** Admin HQ can invite Registration and Redemption Agents.
- **Agent Type Management:** The system distinguishes Registration Agents from Redemption Agents.
- **Geographic Assignment:** Agents can be assigned to specific LGAs or Wards.
- **Agent Activity Logs:** Admin HQ can view farmers registered, offline registrations, synchronization history, and other activity.
- **Agent Suspension:** Admin HQ can suspend an agent.
- **Agent Deletion:** Admin HQ can delete an agent subject to audit and historical-record rules.
- **Agent Reassignment:** Admin HQ can change an agent’s assigned LGA or Ward.
- **Performance Monitoring:** Dashboard and reports show agent activity and performance metrics.

### D. Online Farmer Registration

- **Registration Fields:**
  - Full Name
  - Gender
  - Phone Number
  - NIN — optional
  - LGA
  - Ward
  - Crop Type or Livestock Type
  - Farm Size
  - Proof of Residence image
  - GPS coordinates
- **Master-Data Dropdowns:** LGA and Ward values are selected from synchronized reference data.
- **Client-Side Validation:** Required field and format validation should occur before submission.
- **Real-Time Submission:** Online records are sent to the central database through the registration API.
- **Duplicate Detection:** The system checks phone number, NIN, and name-plus-location combinations.
- **Permanent IDN Generation:** Successful registration generates a unique Plateau Agric ID Number.
- **IDN Confirmation:** The farmer receives the permanent IDN by SMS.
- **Immediate Registry Visibility:** Successful online registration should be visible in the central registry and relevant Admin metrics.

### E. Offline Farmer Registration and Deferred Sync

- **Automatic Offline Detection:** The Agent application detects loss of connectivity and enters Offline Mode.
- **Offline Banner:** The interface displays a clear message that registrations will synchronize when connectivity returns.
- **Equivalent Form:** The same farmer registration fields are available offline.
- **Offline Validation:** Basic validation works without a server dependency.
- **Encrypted Local Storage:** Offline records are stored locally using AES-256 or an equivalent approved encryption implementation.
- **Temporary ID:** Each offline record receives a temporary identifier such as `TEMP-PLT-02345`.
- **Camera and GPS:** Image capture and GPS remain available offline where device permissions and hardware permit.
- **Temporary Registration Slip:** The agent can generate a slip containing farmer name, temporary ID, LGA/Ward, registration date, and `Pending Sync` status.
- **Deferred SMS Draft:** An optional SMS draft may be stored for later delivery after synchronization.
- **Pending Record Management:** Agents can view, edit, and complete pending offline records.
- **Automatic Sync:** Unsynced records are pushed automatically after connectivity returns.
- **Server Validation:** The backend validates synchronized records and checks duplicates.
- **Permanent IDN Assignment:** A valid synchronized record receives a permanent IDN.
- **Status Transition:** Successfully synchronized records become `Verified`.
- **Local Cleanup:** Offline data is cleared only after successful server confirmation.
- **Sync Statuses:** Agents can distinguish synced, pending, and failed records.
- **Sync Audit Logs:** HQ records the agent, farmer record, offline origin, synchronization time, and outcome.

### F. Farmer Registry and Duplicate Resolution

- **Registry Access:** Authorized Admin HQ users can view all registered farmers.
- **Verification:** Admin HQ can verify or approve farmer records.
- **Duplicate Criteria:** Duplicate candidates are identified by phone number, NIN, or name plus location.
- **Record Actions:** Authorized users can approve, flag, or merge farmer records.
- **Filters:** Filter by cooperative, Ward, and crop type.
- **Exports:** Export farmer records in CSV, XLSX, and PDF formats.
- **Auditability:** Verification, flagging, merging, and export actions should be timestamped and attributed to the acting user.
- **Offline Conflict Handling:** Records captured offline must undergo duplicate validation during synchronization before permanent IDN issuance.

### G. Cooperative Management

- **Cooperative Registry:** View cooperatives and their member farmers.
- **Cooperative Verification:** Approve or verify cooperative data.
- **Program Assignment:** Assign input programs to selected cooperatives.
- **Performance Reporting:** Generate reports by cooperative participation or performance.
- **Member Relationship:** Farmer-to-cooperative membership must be reflected in registry filters and program eligibility.

### H. Input Distribution and Redemption

- **Input Categories:** Admin HQ can define categories such as fertilizer, seed, and pesticide.
- **Farmer Allocation:** Authorized users allocate inputs to eligible farmers.
- **Cooperative Allocation:** Input programs can be assigned to cooperatives where defined.
- **Real-Time Progress:** Admin HQ tracks distribution and redemption progress.
- **Delivery and Redemption Logs:** The platform records allocation, delivery, agent, farmer, item, location, and redemption information.
- **IDN Lookup:** A Redemption Agent scans or enters a farmer IDN.
- **Farmer Retrieval:** The system retrieves the matching farmer record.
- **Allocation Display:** The agent sees the farmer’s allocated items.
- **Redemption Confirmation:** The agent confirms and records the transaction.
- **Farmer Notification:** The farmer receives an SMS confirmation containing the redeemed item, location, and date.
- **HQ Synchronization:** Redemption data updates the central database and Admin HQ dashboard in real time.
- **Online Requirement:** Redemption cannot be completed while the Redemption Agent is offline.

### I. Communications and Farmer Channels

- **Registration SMS:** Send the permanent IDN after successful registration or successful offline synchronization.
- **Redemption SMS:** Send a redemption confirmation after a successful transaction.
- **Program Updates:** Admin HQ may send SMS updates to farmers or agents.
- **Bulk Broadcast:** Recipients may be selected by gender, location, or cooperative.
- **USSD Access:** Farmers with feature phones may access basic services through USSD.
- **Mobile Web Access:** Smartphone users may access supported farmer services through the client portal.
- **IDN Usage:** Farmers use the IDN for future interventions, input collection, and other approved agricultural programs.
- **Delivery Traceability:** SMS events should retain recipient, template or message type, triggering record, dispatch time, and delivery outcome where supported.

### J. Security, Audit, Reporting, and Government Integration

- **Transport Encryption:** Data in transit must use TLS 1.3.
- **Offline Encryption:** Locally stored offline registration data must be encrypted.
- **Local Data Restrictions:** Offline records cannot be exported, copied, or shared outside the Agent application.
- **Activity Logging:** Administrative, agent, registration, synchronization, allocation, redemption, communication, and data-resolution actions are timestamped.
- **Dashboard API:** The Admin Portal sends summarized metrics to the Governor’s Agricultural Dashboard through secure APIs.
- **Government Data Set:** Summary data includes farmers by LGA, input redemption statistics, agent performance, and gender and crop distribution.
- **Export and Report Security:** Report generation and export access must respect administrative permissions and data-protection rules.
- **Traceability:** Every synchronization and redemption event should be attributable to an agent and time.

---

## 3. User Stories

> **As a Super Admin,** I want to manage administrative users and platform-wide permissions, so that sensitive agricultural records and operations remain controlled.

> **As a Program Admin,** I want to configure agents, programs, input categories, and allocations, so that agricultural interventions are delivered to the correct farmers and cooperatives.

> **As a Data Officer,** I want to review, verify, flag, merge, filter, and export farmer records, so that the central registry remains accurate and usable.

> **As an Admin HQ user,** I want to view farmer, agent, cooperative, allocation, and redemption metrics, so that the Ministry can monitor agricultural programs across Plateau State.

> **As an Admin HQ user,** I want to assign agents to LGAs and Wards and review their activity logs, so that field operations remain geographically accountable.

> **As a Registration Agent,** I want to register farmers online, so that valid records receive permanent IDNs immediately.

> **As a Registration Agent,** I want to capture farmer records offline with temporary IDs, so that registration can continue in low-connectivity communities.

> **As a Registration Agent,** I want pending offline registrations to synchronize automatically when connectivity returns, so that farmers receive verified records without duplicate manual entry.

> **As a Registration Agent,** I want to view pending, synced, and failed records, so that I can resolve incomplete or failed registrations.

> **As a Redemption Agent,** I want to retrieve a farmer by IDN and view allocated inputs, so that I distribute only approved items.

> **As a Redemption Agent,** I want completed redemptions to update Admin HQ immediately, so that program monitoring remains current.

> **As a Farmer,** I want to receive my unique IDN by SMS, so that I can participate in agricultural programs and future interventions.

> **As a Farmer,** I want to receive a redemption confirmation, so that I have evidence of the item, place, and date of collection.

> **As an Admin HQ user,** I want to send targeted and bulk SMS messages, so that farmers and agents receive relevant program information.

> **As an Admin HQ user,** I want summarized data sent securely to the Governor’s Agricultural Dashboard, so that government decision-makers can monitor state-wide outcomes.

---

## 4. Core User Flows in Gherkin Syntax

The scenarios below define the FRD-derived behavioural coverage inventory. They are intended primarily for QA or staging. Repetitive and deterministic scenarios form the automation backlog. Real-device field usability, subjective presentation quality, unusual network behaviour, and requirement-ambiguous scenarios remain manual or hybrid until they can be controlled reliably.

### Flow 1: Authentication and Role Protection

```gherkin
Feature: Plateau Agric authentication and role-based access

  Scenario Outline: A supplied QA account signs in to its assigned portal
    Given the <role> account is active
    And the portal URL and credentials are loaded from environment variables
    When the user submits valid credentials
    Then an authenticated session is established
    And the user is directed to the permitted application area
    And credentials and authentication tokens are not exposed in logs or reports

    Examples:
      | role          |
      | Farmer Client |
      | Agent         |

  Scenario: An authenticated user opens a route outside the assigned role
    Given the user is authenticated with a restricted role
    When the user navigates directly to an unauthorized route
    Then access is denied
    And protected records are not returned
    And no farmer, allocation, agent, or redemption record is changed
```

### Flow 2: Online Farmer Registration

```gherkin
Feature: Online farmer onboarding

  Scenario: A Registration Agent registers a new farmer online
    Given the Registration Agent is authenticated
    And the application has network connectivity
    And synchronized LGA and Ward reference data is available
    And the farmer does not match an existing record
    When the agent enters the required farmer information
    And uploads proof of residence
    And captures available GPS coordinates
    And submits the registration
    Then the form data is sent to the central registration service
    And the farmer record is stored in the central registry
    And a unique permanent IDN is assigned
    And the registration status reflects successful verification
    And an IDN confirmation SMS is requested for the farmer
    And the Admin HQ farmer total is updated
```

### Flow 3: Online Duplicate Detection

```gherkin
Feature: Farmer duplicate prevention

  Scenario Outline: A submitted farmer matches an existing identity signal
    Given an existing farmer record matches the submitted <duplicate_basis>
    And the Registration Agent is online
    When the agent submits the new registration
    Then the system identifies a potential duplicate
    And a second permanent IDN is not silently issued
    And the record is routed to the approved duplicate-resolution process
    And the duplicate event is audit-logged

    Examples:
      | duplicate_basis       |
      | phone number          |
      | NIN                   |
      | name plus location    |
```

### Flow 4: Offline Farmer Capture

```gherkin
Feature: Offline farmer registration

  Scenario: The Agent application captures a registration without connectivity
    Given the Registration Agent has a valid cached session
    And the device has no network connectivity
    When the Agent application detects the outage
    Then the application displays the Offline Mode banner
    When the agent completes the registration form with valid offline data
    And submits the record
    Then the record is stored in encrypted local storage
    And a unique temporary ID is assigned
    And the record status is "Pending Sync"
    And a temporary registration slip can be generated
    And no permanent IDN is issued before server validation
```

### Flow 5: Offline Record Editing and Validation

```gherkin
Feature: Pending offline record management

  Scenario: An Agent edits an incomplete pending record before synchronization
    Given an offline farmer record exists with status "Pending Sync"
    And the record has not been accepted by the central server
    When the agent opens the record
    And corrects or completes the editable fields
    Then offline field validation is applied
    And the updated record remains encrypted in local storage
    And the temporary ID remains associated with the record
    And the record remains queued for synchronization
```

### Flow 6: Connectivity Restoration and Synchronization

```gherkin
Feature: Deferred farmer-registration synchronization

  Scenario: A valid offline record synchronizes successfully
    Given an encrypted offline record exists with a temporary ID
    And network connectivity has been restored
    When the synchronization service submits the record
    Then the backend validates the farmer data
    And performs the configured duplicate checks
    And assigns one permanent IDN
    And returns successful confirmation to the Agent application
    And the local record status changes to "Verified"
    And the successful record is cleared from local pending storage
    And the temporary ID remains traceable to the permanent IDN in the audit log
    And the farmer receives an IDN confirmation SMS
    And Admin HQ records the offline origin and synchronization time

  Scenario: A synchronization attempt fails
    Given a pending offline record is queued
    And connectivity is available
    When the server or network rejects the synchronization attempt
    Then the record status becomes "Failed" or remains pending according to the approved state model
    And the local encrypted record is not deleted
    And the agent can see that the record requires retry or correction
    And no permanent IDN is displayed without confirmed server acceptance
```

### Flow 7: Agent Administration

```gherkin
Feature: Registration and Redemption Agent administration

  Scenario: Admin HQ invites and assigns an Agent
    Given an authorized Admin HQ user is authenticated
    When the admin creates an invitation for an Agent type
    And assigns an LGA or Ward
    Then the Agent record is created with the selected type and location
    And the invitation or activation process is initiated
    And the assignment is visible in agent management
    And the action is timestamped and attributed to the admin

  Scenario: Admin HQ suspends an Agent
    Given an Agent account is active
    When an authorized admin suspends the account
    Then new authenticated activity is denied according to the approved suspension policy
    And historical farmer, synchronization, allocation, and redemption records remain attributable to the Agent
    And the suspension is audit-logged
```

### Flow 8: Farmer Registry Review and Export

```gherkin
Feature: Farmer registry administration

  Scenario: A Data Officer filters and exports farmer records
    Given the Data Officer has farmer-registry and export permission
    And farmer records exist across multiple locations, crops, and cooperatives
    When the officer filters by Ward, crop type, or cooperative
    Then only matching records are displayed
    When the officer exports the filtered result as <format>
    Then the generated file contains the filtered data set
    And the export action is audit-logged

    Examples:
      | format |
      | CSV    |
      | XLSX   |
      | PDF    |

  Scenario: An authorized user resolves duplicate farmer records
    Given duplicate candidate records are visible in the registry
    When the user selects the approved merge action
    Then the records are resolved according to the configured master-record rules
    And the retained permanent IDN remains unique
    And the source records and merge actor remain traceable
```

### Flow 9: Input Allocation and Redemption

```gherkin
Feature: Agricultural input allocation and redemption

  Scenario: An Admin allocates an input to an eligible farmer
    Given an authorized Program Admin is authenticated
    And the farmer exists and is eligible for the program
    And an input category is available
    When the admin assigns the approved quantity to the farmer
    Then an active allocation is stored
    And the allocation is visible to the Redemption Agent
    And the Admin HQ allocation totals are updated
    And the action is audit-logged

  Scenario: A Redemption Agent records an authorized redemption
    Given the Redemption Agent is authenticated and online
    And the farmer has a valid permanent IDN
    And an unredeemed allocation exists
    When the Agent scans or enters the IDN
    Then the farmer and allocated items are displayed
    When the Agent confirms the permitted redemption
    Then the redemption record is stored in the central database
    And the allocation and redemption status are updated
    And an SMS confirmation is requested for the farmer
    And Admin HQ redemption metrics are updated
```

---

## 5. Expected Outcomes and Success Measures

### Functional Outcomes

- Valid online farmer registrations are stored centrally and receive one permanent IDN.
- Valid offline registrations remain available during network outages and synchronize when connectivity returns.
- Temporary IDs remain distinguishable from permanent IDNs.
- Local offline records are not deleted until the central server confirms successful synchronization.
- Duplicate checks occur for online registration and deferred offline synchronization.
- Agent actions remain attributable by identity, location, and timestamp.
- Suspended or unauthorized users cannot perform restricted operations.
- Farmer records can be filtered and exported in the FRD-defined formats.
- Input allocations are visible to authorized Redemption Agents.
- Redemption cannot be completed without online access and a valid farmer allocation.
- Successful registration and redemption events trigger the required farmer SMS.
- Central dashboard totals reflect accepted registrations, active agents, cooperatives, allocations, and redemptions.
- Government dashboard summaries reflect approved Admin HQ source data.
- Offline records cannot be exported or shared outside the Agent application.
- Audit logs preserve registration origin, synchronization outcome, allocation, redemption, and administrative actions.

### Quantitative and Time-Based Rules Defined by the FRD

- Cached Agent authentication may persist offline for up to **14 days** before renewal.
- Local offline records require **AES-256** encryption.
- Data in transit requires **TLS 1.3**.
- Online registrations synchronize in **real time**.
- Offline registrations use **deferred synchronization**.
- Redemption synchronizes in **real time**.
- Farmer exports support **CSV, XLSX, and PDF**.
- Accepted duplicate signals are **phone number, NIN, and name plus location**.
- Successful registration generates one permanent Plateau Agric IDN and an SMS request.

### Security and Access Outcomes

- Admin-only functions are inaccessible to Agent and Farmer accounts.
- Registration Agents cannot perform Redemption Agent functions unless explicitly assigned.
- Redemption Agents cannot complete a transaction offline.
- Offline data remains encrypted and inaccessible outside the application.
- Authentication tokens, NIN values, phone numbers, proof-of-residence files, GPS coordinates, and credentials are masked from automation evidence.
- Production automation remains non-destructive unless separately authorized.

### Auditability Outcomes

- Registration, verification, flagging, merge, agent assignment, suspension, allocation, redemption, synchronization, export, and communication events retain actor and timestamp details.
- Offline origin and synchronization time are visible to Admin HQ.
- Temporary-to-permanent ID mapping is traceable.
- Farmer merge history remains traceable.
- Government dashboard data can be reconciled to Admin HQ source metrics.
- SMS requests can be correlated to the registration or redemption that triggered them.

### Measures Not Yet Defined

The FRD does not define maximum response times, supported concurrent users, file-size limits, SMS-delivery service levels, dashboard refresh intervals, synchronization retry intervals, retry limits, offline queue capacity, GPS accuracy tolerance, duplicate-match thresholds, export completion time, government dashboard transmission frequency, USSD session rules, uptime targets, accessibility criteria, or supported browsers and devices. These require approval before performance or service-level automation can be considered complete.

---

## 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
|---|---|---|
| Client Authentication | Farmer or client login | Front-end authentication route and identity/session API; exact role and credential model require inspection |
| Admin Authentication | Email/password or SSO with 2FA | Identity provider, authentication API, second-factor provider, and role service |
| Agent Authentication | Credential login and cached offline token | Authentication API issuing JWT or equivalent; secure device token storage |
| Admin RBAC | Super Admin, Program Admin, and Data Officer permissions | Authorization middleware and role/permission persistence |
| Agent Assignment | Agent type and LGA/Ward assignment | Agent-management API and geographic master-data service |
| Dashboard Metrics | Farmer, agent, cooperative, allocation, redemption, gender, crop, and activity totals | Aggregation APIs or reporting queries; formulas and refresh cadence require confirmation |
| Online Registration | Validate and persist farmer record | Registration API and central farmer database |
| LGA and Ward Dropdowns | Synchronized reference data | Master-data API with local caching for offline access |
| Proof of Residence | Image capture and upload | Device camera/file input, upload API, and object storage |
| GPS Capture | Registration coordinates | Device geolocation API and farmer record fields |
| Duplicate Detection | Match phone, NIN, or name plus location | Synchronous or asynchronous matching service against central registry |
| IDN Generation | Assign permanent farmer identifier | Central unique-ID service or database-generated identity |
| Offline Storage | Store unsynced records securely | Encrypted local database; exact technology requires confirmation |
| Offline Sync | Push queued records after network restoration | Local sync queue, synchronization service, registration API, and retry/idempotency controls |
| Temporary Slip | Generate offline registration evidence | Client-side template or local document-generation component |
| SMS Confirmation | Send IDN and redemption messages | SMS gateway triggered by accepted backend transaction |
| Farmer Registry | View, verify, flag, merge, filter | Farmer-management API and central database |
| Registry Export | Produce CSV, XLSX, and PDF | Reporting/export service and temporary secure file storage |
| Cooperative Management | Verify cooperatives and manage members | Cooperative API and farmer-membership records |
| Input Categories | Define fertilizer, seed, pesticide, and other types | Program and inventory configuration API |
| Input Allocation | Assign input quantity to farmer or cooperative | Allocation API and eligibility/program data |
| Redemption | IDN lookup, allocation retrieval, transaction confirmation | Online Redemption API and central transaction database |
| Agent Activity Logs | Registration and sync history | Audit/event store and reporting API |
| Bulk Communication | Target farmers or agents by category | Recipient-query service, message queue, and SMS provider |
| USSD | Feature-phone service access | USSD gateway and session-based backend API |
| Mobile Web | Farmer smartphone access | Responsive client application and farmer-facing APIs |
| Government Dashboard | Publish summarized metrics | Secure API integration, scheduled or event-driven data transfer |
| Transport Security | TLS 1.3 | Application gateway, load balancer, or server TLS configuration |
| Audit Logging | Timestamp and attribute sensitive actions | Central audit/event logging service |

---

## 7. Integration-Based Gherkin Flows

These integration scenarios are intended for QA, staging, device test labs, or approved provider sandboxes. API stress, synchronization-load, SMS-provider failure, and resilience checks must run as separately controlled suites. They must not run as part of ordinary Production smoke testing.

### Integration Flow 1: Offline Sync Idempotency

```gherkin
Feature: Idempotent synchronization of offline registrations

  Scenario: The same offline record is retried after an uncertain response
    Given an offline registration has a stable temporary identifier
    And the first synchronization request reached the server
    But the client did not receive the final response
    When the Agent application retries the same record
    Then the server does not create a second farmer
    And no second permanent IDN is issued
    And the client receives the existing accepted result or the approved conflict response
    And the retry is visible in synchronization logs
```

### Integration Flow 2: Offline Record Duplicate Conflict

```gherkin
Feature: Duplicate validation during deferred synchronization

  Scenario Outline: An offline record conflicts with a record created before sync
    Given an offline registration is pending
    And the central registry now contains a farmer matching the same <duplicate_basis>
    When the pending record synchronizes
    Then the backend identifies the duplicate candidate
    And does not silently issue another permanent IDN
    And returns the configured review or resolution status to the Agent
    And the local record is retained until resolution is confirmed

    Examples:
      | duplicate_basis    |
      | phone number       |
      | NIN                |
      | name plus location |
```

### Integration Flow 3: Registration SMS Dispatch

```gherkin
Feature: Registration confirmation through the SMS gateway

  Scenario: A permanent IDN triggers one farmer SMS request
    Given the central registration transaction has committed successfully
    And a permanent IDN has been assigned
    When the notification event is processed
    Then one SMS request is created for the registered phone number
    And the message includes the farmer name and permanent IDN
    And a provider retry does not create duplicate farmer notifications
    And SMS failure does not create a second farmer registration
```

### Integration Flow 4: Redemption Synchronization

```gherkin
Feature: Real-time redemption integration

  Scenario: A successful redemption updates central reporting
    Given a farmer has an active input allocation
    And the Redemption Agent is online
    When the Agent confirms the redemption
    Then the Redemption API stores one transaction
    And the allocation balance or status is updated
    And the Admin HQ dashboard source data reflects the redemption
    And one redemption SMS request is created
    And repeated submission does not redeem the same allocation twice
```

### Integration Flow 5: Dashboard Metric Reconciliation

```gherkin
Feature: Admin dashboard metric accuracy

  Scenario: Accepted farmer activity is reflected in dashboard totals
    Given a known baseline exists for farmer, agent, cooperative, allocation, and redemption counts
    When controlled QA records are created or updated
    Then each relevant dashboard total changes by the expected amount
    And geographic, gender, crop, and activity breakdowns remain consistent with the source records
    And filtered details reconcile with the displayed aggregate
```

### Integration Flow 6: Governor’s Dashboard Data Feed

```gherkin
Feature: Government agricultural dashboard synchronization

  Scenario: Approved summary metrics are transmitted securely
    Given Admin HQ has current approved summary data
    When the government-dashboard integration runs
    Then the transmitted data contains farmer counts by LGA
    And input redemption statistics
    And agent performance metrics
    And gender and crop distribution summaries
    And the transfer is authenticated and encrypted
    And the received government figures can be reconciled to the source snapshot

  Scenario: The government endpoint is temporarily unavailable
    Given a summary transfer is due
    And the receiving endpoint returns a transient failure
    When the integration applies its configured recovery policy
    Then the source data remains intact
    And the failed transfer is visible for retry or operational action
    And duplicate retries do not inflate the receiving dashboard totals
```

### Integration Flow 7: Agent Offline Token Validity

```gherkin
Feature: Cached Agent session while offline

  Scenario: A valid cached token permits offline registration
    Given the Agent authenticated while online
    And the cached token is within the permitted offline period
    And the device loses connectivity
    When the Agent opens the registration workflow
    Then the Agent can access the approved offline functions
    But online-only actions remain unavailable

  Scenario: The cached token exceeds the offline validity period
    Given the device is offline
    And the cached authentication period has exceeded 14 days
    When the Agent attempts to continue protected activity
    Then access is restricted according to the approved expiry policy
    And locally stored pending records are not deleted
    And the Agent is instructed to reconnect and renew authentication
```

### Integration Flow 8: Bulk SMS Recipient Resolution

```gherkin
Feature: Targeted program communication

  Scenario Outline: Admin HQ selects a valid broadcast audience
    Given the Admin user has communication permission
    And eligible recipients exist for the selected <category>
    When the Admin confirms the broadcast
    Then the recipient list is resolved using the selected category
    And one message request is created per unique eligible recipient
    And the broadcast and recipient count are audit-logged

    Examples:
      | category    |
      | gender      |
      | location    |
      | cooperative |
```

---

## 8. System Instruction for the Plateau Agric V1 QA Automation Agent

### Role

You are the **Senior QA Automation Engineer for Plateau Agric V1**. You are responsible for designing and maintaining automated checks that remove repetitive QA effort across the client, Agent, Admin HQ, API, synchronization, SMS, redemption, and reporting layers while preserving manual capacity for exploratory field testing and high-risk scenarios requiring human judgment.

Operate differently by environment:

- **QA / Staging:** Validate new features and bug fixes, run smoke and regression suites, test APIs and integrations, exercise online/offline transitions, run controlled stress tests, document defects, and perform manual exploratory testing.
- **Production:** Monitor logs, error reports, SMS failures, synchronization failures, and field feedback. Run only authorized smoke and sanity checks, and support rollback verification. Do not use Production for normal registration, redemption, bulk communication, regression, or stress execution.

Your objective is not to automate every possible scenario. Automate the **repetitive, deterministic, business-relevant checks** that must be rerun after deployments, bug fixes, configuration changes, and integration updates.

### Platform Overview

Plateau Agric is a centralized agricultural management platform for Plateau State. It supports:

- Admin HQ oversight and analytics
- Registration and Redemption Agent administration
- Online farmer registration
- Offline farmer registration and deferred synchronization
- Duplicate detection and farmer registry resolution
- Cooperative management
- Input program allocation and redemption
- Farmer IDN generation
- SMS, mobile-web, and USSD farmer communication
- Agent activity and synchronization logs
- Government dashboard reporting
- Data encryption, role-based access, and audit logging

The FRD roles are Admin HQ, Registration Agent, Redemption Agent, and Farmer. Admin HQ contains Super Admin, Program Admin, and Data Officer sub-roles. Do not infer detailed permissions until the application and API confirm them.

### Automation Test Basis

Use the following test bases:

1. **FRD Requirements**
   - Registration fields
   - Role responsibilities
   - Online and offline workflow rules
   - Duplicate criteria
   - IDN issuance
   - Export formats
   - Notification triggers
   - Security and audit requirements

2. **State Transitions**
   - Online submission to verified farmer
   - Offline capture to pending sync
   - Pending or failed sync to verified
   - Agent active to suspended
   - Allocation available to redeemed
   - Duplicate candidate to approved, flagged, or merged

3. **Repetitive Operational Journeys**
   - Login
   - Farmer registration
   - Offline record capture and sync
   - Farmer search, filtering, and export
   - Agent assignment
   - Input allocation
   - IDN lookup
   - Redemption
   - Dashboard reconciliation
   - SMS dispatch

4. **Role and Data Boundaries**
   - Admin, Agent, Redemption Agent, and Farmer access
   - LGA and Ward assignment scope
   - Online-only redemption
   - Protected farmer personal data
   - Offline local-storage restrictions

5. **Integration Contracts**
   - Registration API
   - Sync service
   - SMS gateway
   - Government dashboard API
   - GPS and camera permissions
   - USSD gateway
   - Export service

6. **Defect History**
   - Add a permanent regression test for every confirmed repeatable bug after the fix is validated.

### Automation Selection Rule

Automate a QA check when it is:

- Repeated after deployments or fixes
- Deterministic and objectively assertable
- Business-critical or likely to regress
- Safe to execute with synthetic data
- Supported by reliable setup and cleanup
- Observable through UI, API, local state, queued event, SMS sandbox, or generated report
- Valuable enough to maintain

Keep coverage manual or hybrid when it involves:

- Real field usability
- Physical device and OS differences
- Subjective form clarity
- Camera image quality
- GPS accuracy in actual locations
- Poor or fluctuating rural connectivity that cannot be simulated reliably
- USSD experience across telecom providers
- Accessibility and language comprehension
- Unexpected farmer-data patterns
- New or unstable workflows
- Security and abuse exploration requiring human judgment

Automate high-risk deterministic rules; manually explore high-risk unpredictable behaviour.

### Environment and Secret Management

- Load URLs, credentials, tokens, test phone numbers, SMS secrets, and API settings from environment variables.
- Never hard-code secrets or personal data.
- Mask phone numbers, NINs, GPS coordinates, proof-of-residence images, authentication tokens, and IDNs in public logs and reports.
- Keep QA, staging, sandbox, and Production configuration isolated.
- Use synthetic farmers, cooperatives, agents, programs, allocations, and redemptions.
- Use reserved non-production phone numbers and SMS sandboxes.
- Do not send test messages to real farmers.
- Require a hard environment guard before any test targets Production.
- Preserve pending offline records during failed-test cleanup unless the test explicitly validates deletion after confirmed synchronization.

### Core Responsibilities

#### 1. Requirements Traceability

- Convert every FRD feature into testable behaviours.
- Assign stable identifiers such as `AUTH-001`, `REG-ONLINE-001`, `REG-OFFLINE-002`, `SYNC-003`, `DUP-002`, `REDEEM-001`, and `DASH-004`.
- Maintain:
  - Requirement
  - Role
  - Environment
  - Test type
  - Automation suitability
  - Automation status
  - Manual coverage
  - Last execution
  - Defect or clarification dependency
- Do not mark a feature covered because a screen exists.

#### 2. QA Automation Priorities

**P0 — Deployment Smoke**

- Client and Agent URL availability
- Login using supplied accounts
- Session establishment and logout
- Core route navigation
- Registration page availability
- Offline banner activation through controlled network loss
- Critical registration and lookup API availability

**P0 — Critical Regression**

- Online farmer registration
- Required field validation
- Duplicate detection
- Permanent IDN issuance
- Offline record capture
- Temporary ID creation
- Pending, failed, and verified states
- Connectivity-restored synchronization
- Idempotent sync retry
- Farmer registry persistence
- Input allocation
- Redemption and duplicate-redemption prevention
- SMS event creation

**P1 — Repetitive Functional Regression**

- Farmer filters
- CSV, XLSX, and PDF exports
- Agent invitation and geographic assignment
- Agent suspension
- Cooperative verification
- Program assignment
- Dashboard total reconciliation
- Agent activity logs
- Bulk recipient filtering
- Government dashboard feed reconciliation

**P1 — RBAC and Security Regression**

- Admin-only actions
- Registration versus Redemption Agent boundaries
- Farmer access restrictions
- Direct-route and API denial
- Offline token validity
- Local-data export restriction
- Audit-log attribution
- TLS and sensitive-data handling where testable

**P1 — Bug Regression**

- Validate fixes in QA.
- Convert deterministic defects into permanent automated tests.
- Link each test to the defect record.

**P2 — API, Integration, and Resilience**

- Registration API
- Offline sync queue
- Retry and idempotency
- Duplicate conflict after offline capture
- SMS gateway
- Government dashboard API
- GPS and camera permission handling
- USSD sandbox
- Export generation
- Provider-failure recovery

#### 3. Manual and Exploratory Allocation

Maintain exploratory charters for:

- Real-device Agent workflows
- Offline usability in actual field conditions
- Rapid connectivity switching
- Camera capture quality
- GPS accuracy and permission denial
- Large or unusual proof-of-residence images
- Form comprehension and validation clarity
- Farmer mobile-web usability
- USSD timing and telecom-provider differences
- Bulk-message content quality
- Dashboard visual interpretation
- Accessibility
- Security abuse scenarios
- Complex merge decisions
- Unspecified edge cases

#### 4. Controlled Stress and Resilience Testing

- Run stress only in QA or approved sandboxes.
- Keep stress suites separate from smoke and regression.
- Agree limits before testing:
  - Concurrent registrations
  - Offline queue size
  - Synchronization batch size
  - SMS event volume
  - Export record count
  - Redemption throughput
  - Dashboard aggregation load
- Verify graceful degradation, queue retention, retry behaviour, timeout handling, and recovery.
- Never run uncontrolled traffic floods.
- Never stress Production without a separately approved operational exercise.

#### 5. Production Policy

Permitted with authorization:

- URL and login-page availability
- Read-only health checks
- Non-destructive navigation
- Critical dependency monitoring
- Log and error review
- SMS delivery-failure monitoring
- Synchronization-failure monitoring
- Release smoke checks
- Occasional sanity checks
- Rollback verification
- Correlation of field feedback with logs and defects

Prohibited by default:

- Farmer registration
- Offline synchronization
- Agent creation or suspension
- Farmer merge
- Input allocation
- Redemption
- SMS broadcast
- Export of real farmer data
- Stress, load, soak, or volume tests
- Destructive cleanup
- Full regression

#### 6. Environment and Network Audit

- Inspect the Client and Agent portals.
- Confirm the actual role represented by each supplied account.
- Locate Admin HQ and Redemption Agent routes if reachable.
- Inventory front-end routes and network calls.
- Identify registration, lookup, synchronization, allocation, redemption, export, SMS, and dashboard requests.
- Observe local-storage and offline-cache behaviour without exposing sensitive values.
- Determine whether the Agent URL is a web application, PWA, or wrapper for a mobile application.
- Request OpenAPI or other API documentation where contracts are unavailable.
- Record observations separately from FRD requirements.

#### 7. Automation Framework Standards

- Use Cypress or Playwright for web and PWA coverage according to repository standards.
- Use Appium or a supported mobile framework only where the actual Agent application requires native-device automation.
- Use JavaScript or TypeScript according to project convention.
- Use reusable authentication, farmer-creation, network-control, sync, allocation, and redemption helpers.
- Prefer API-assisted setup for prerequisites where safe.
- Use stable `data-*` selectors.
- Avoid fixed waits.
- Synchronize against network events, visible state, queue state, or documented polling completion.
- Generate run-specific synthetic phone numbers, NIN-like test values, temporary IDs, farmer names, and allocations.
- Make tests retry-safe and parallel-safe.
- Validate UI and API state for critical flows.
- Isolate mocked provider tests from real sandbox integration tests.
- Capture offline and online transitions deterministically where supported.
- Do not rely on real SMS delivery for ordinary CI execution.

#### 8. Negative and Edge-Case Coverage

Automate stable cases including:

- Invalid credentials
- Expired Admin or Agent sessions
- Expired offline cached token
- Unauthorized route and API access
- Missing required fields
- Invalid phone number
- Invalid NIN when provided
- LGA/Ward mismatch
- Unsupported or missing proof image
- GPS unavailable
- Duplicate phone, NIN, or name plus location
- Repeat online submission
- Repeat offline synchronization
- Server failure during sync
- Partial synchronization response
- Local record retained after failure
- IDN generation failure
- SMS provider failure
- Agent suspension
- Export failure
- Invalid farmer IDN
- No active allocation
- Duplicate redemption
- Network loss during redemption
- Bulk-message duplicate recipient
- Government dashboard endpoint failure
- Dashboard/source mismatch

Keep poorly specified or provider-dependent cases manual until the expected behaviour is clarified.

#### 9. API and Integration Verification

- Validate authorization, request schema, response handling, persistence, retries, idempotency, and UI synchronization.
- Verify that repeated synchronization does not create duplicate farmers or IDNs.
- Verify that SMS failure does not roll back a committed farmer or redemption transaction.
- Verify that a failed government-dashboard transfer does not corrupt source metrics.
- Verify that repeated redemption submission does not consume one allocation twice.
- Verify that dashboard aggregates reconcile with source records.
- Verify that local records are deleted only after accepted sync.
- Use mocks for provider failures and separate sandboxes for real integration checks.
- Do not infer undocumented endpoint names or payload fields.

#### 10. Defect Evidence and Reporting

For each failure:

- Record requirement ID, test ID, role, environment, farmer or transaction test identifier, and expected state.
- Capture visible state and relevant masked network evidence.
- Capture local pending/sync state where safe.
- Classify the issue as:
  - Product defect
  - Automation defect
  - Environment defect
  - Test-data defect
  - Device or browser defect
  - Integration/provider defect
  - FRD ambiguity
  - Product drift
- Provide concise reproduction steps.
- Link fixed defects to permanent regression coverage.
- Never expose real farmer PII in defects.

#### 11. FRD Drift Reporting

Report:

- Missing modules or routes
- Different roles or permissions
- Different registration fields
- Different required/optional rules
- Undocumented statuses
- Changed duplicate logic
- Changed offline-token duration
- Different sync behaviour
- Different SMS triggers
- Different export formats
- Dashboard metrics that differ from the FRD
- Features present but undocumented
- FRD features absent in QA

Do not silently change assertions to accept undocumented drift.

#### 12. CI/CD and Suite Design

Define:

- `qa-smoke`
- `qa-critical-registration`
- `qa-offline-sync`
- `qa-farmer-registry`
- `qa-agent-management`
- `qa-input-redemption`
- `qa-rbac-security`
- `qa-bug-regression`
- `qa-api-integration`
- `qa-extended`
- `qa-stress`
- `production-smoke-readonly`
- `production-sanity-readonly`

Execution rules:

- Run `qa-smoke` after deployment.
- Run registration and offline-sync regression before promotion.
- Run affected bug-regression tests after fixes.
- Run integration suites in QA and provider sandboxes.
- Run stress only by scheduled approval.
- Prevent write and stress suites from targeting Production.
- Keep smoke suites fast and independent.
- Quarantine only with a linked defect and review date.

### Required Agent Deliverables

Maintain:

- **FRD-to-Test Coverage Matrix**
- **Automation Suitability and Priority Matrix**
- **QA Repetitive-Checks Backlog**
- **Manual Exploratory Charter Register**
- **Environment and Role Audit**
- **Route and Network/API Inventory**
- **Farmer Test Data Schema**
- **Offline Synchronization State Model**
- **RBAC Verification Matrix**
- **Automated Smoke and Regression Suites**
- **Bug Regression Register**
- **API and Integration Suite**
- **Controlled Stress Test Plan**
- **SMS and Notification Test Report**
- **Dashboard Reconciliation Report**
- **Defect and FRD Drift Register**
- **Execution Report**
- **Known Limitations Register**
- **Production Monitoring and Rollback Checklist**
- **Production-Safety Register**

### Starter Commands

1. **“Audit the supplied Plateau Agric Client and Agent URLs, identify the actual role and landing route for each account, and report inaccessible or ambiguous modules.”**
2. **“Create the initial repetitive-check automation backlog for authentication, online registration, offline capture, synchronization, duplicate detection, registry filtering, allocation, redemption, and dashboard reconciliation.”**
3. **“Build a fast QA smoke suite covering Client and Agent availability, login, registration-page access, controlled offline-mode activation, and critical API health.”**
4. **“Generate an FRD-to-test matrix that classifies each requirement as automated, manual exploratory, integration, stress, blocked, or not applicable.”**
5. **“Inspect the online farmer registration flow and document its UI, API, duplicate-check, IDN-generation, SMS-event, and dashboard-update checkpoints.”**
6. **“Implement the QA online-registration regression flow with synthetic farmer data, unique phone numbers, optional NIN coverage, proof upload, GPS handling, and safe cleanup.”**
7. **“Implement the offline-registration and deferred-sync suite using controlled network loss, temporary IDs, encrypted pending state, retry, idempotency, and permanent-ID reconciliation.”**
8. **“Test duplicate detection for phone number, NIN, and name-plus-location across online registration and offline synchronization.”**
9. **“Audit Agent authentication, offline token persistence, 14-day expiry behaviour, suspension, and geographic assignment.”**
10. **“Create the farmer-registry suite for verification, flagging, merge, filters, and CSV/XLSX/PDF exports.”**
11. **“Create the input-allocation and redemption suite, including invalid IDN, missing allocation, duplicate redemption, SMS event, and Admin dashboard reconciliation.”**
12. **“Design a separate SMS integration suite using the approved sandbox, covering registration, redemption, bulk broadcast, retries, duplicate prevention, and provider failure.”**
13. **“Audit the Governor’s Agricultural Dashboard integration and reconcile farmer, redemption, agent, gender, crop, and LGA summaries with Admin HQ source records.”**
14. **“Prepare controlled QA stress tests for registration APIs, offline synchronization queues, exports, SMS event volume, and redemption throughput.”**
15. **“Create manual exploratory charters for real-device field registration, unstable connectivity, camera and GPS behaviour, USSD, accessibility, and unusual farmer data.”**
16. **“Compare Plateau Agric QA behaviour with the FRD and produce a drift register for routes, roles, fields, statuses, validations, synchronization, messaging, and reports.”**
17. **“Build an authorized Production read-only suite limited to availability, login-page, route-health, dependency-health, and non-destructive sanity checks.”**
18. **“Create a Production monitoring and rollback checklist covering errors, failed syncs, SMS failures, redemption anomalies, customer feedback, previous-version restoration, and data-integrity verification.”**
19. **“Add hard environment guards preventing registration, redemption, bulk messaging, merge, stress, and destructive suites from targeting Production.”**

---

## 9. Product Clarifications Required Before Full Automation

1. **Product Manager:** Is the supplied V1 Client URL a QA, staging, pilot, or Production environment?
2. **Product Manager:** Does the supplied “Client” account represent a Farmer, a general client user, or another persona?
3. **Engineering Lead:** What are the Admin HQ QA URL and credentials?
4. **Engineering Lead:** What are the Redemption Agent QA URL and credentials?
5. **Engineering Lead:** Is the Agent Staging URL a mobile web application, a PWA, or a browser route corresponding to a separate native mobile app?
6. **Product Manager:** What is the authoritative permission matrix for Super Admin, Program Admin, and Data Officer?
7. **Product Manager:** Can one Agent hold both Registration and Redemption Agent roles?
8. **Engineering Lead:** What 2FA method is used for Admin HQ, and how should it be automated in QA?
9. **Engineering Lead:** What SSO provider or protocol is supported for government users?
10. **Engineering Lead:** What are the session-expiry, refresh, lockout, logout, and password-reset rules for Admin and Agent users?
11. **Product Manager:** After the 14-day offline token period expires, may the Agent view existing pending records, edit them, or only reconnect?
12. **Security Owner:** How is the cached token stored and protected on the device?
13. **Product Manager:** Which registration fields are mandatory, and what are their precise validation rules?
14. **Product Manager:** What phone-number formats are accepted and normalized?
15. **Product Manager:** When NIN is provided, what length, checksum, or verification rules apply?
16. **Product Manager:** What values are allowed for Gender, Crop Type, Livestock Type, and Farm Size?
17. **Product Manager:** Can a farmer select multiple crops or livestock types?
18. **Product Manager:** What units and numeric ranges apply to Farm Size?
19. **Engineering Lead:** How often are LGA and Ward reference lists refreshed on Agent devices?
20. **Product Manager:** What happens when a Ward is changed or removed while an Agent remains offline?
21. **Product Manager:** Is proof of residence mandatory online and offline?
22. **Engineering Lead:** What image formats, maximum sizes, compression rules, and malware checks apply to proof-of-residence uploads?
23. **Product Manager:** Is GPS mandatory, optional, or conditionally required?
24. **Product Manager:** What should happen when the user denies camera or location permission?
25. **Product Manager:** What GPS accuracy threshold is acceptable?
26. **Product Manager:** What is the exact permanent IDN format and uniqueness scope?
27. **Engineering Lead:** What happens if IDN generation succeeds but the final API response is lost?
28. **Product Manager:** What is the approved workflow when an online or offline registration is identified as a duplicate?
29. **Product Manager:** What threshold or matching logic applies to name-plus-location duplicates?
30. **Product Manager:** Can Admin HQ override a duplicate warning and create a separate farmer?
31. **Product Manager:** What fields survive when two farmer records are merged, and how is the master record selected?
32. **Product Manager:** Can a merge be reversed?
33. **Engineering Lead:** What idempotency key or unique client identifier prevents duplicate sync records?
34. **Engineering Lead:** What retry schedule and maximum retry count apply to failed synchronization?
35. **Product Manager:** What are the authoritative statuses for offline records beyond synced, pending, failed, verified, and pending sync?
36. **Engineering Lead:** Can the Agent manually trigger synchronization, or is it automatic only?
37. **Engineering Lead:** Does synchronization process records individually or in batches?
38. **Engineering Lead:** How are partial batch failures represented and retried?
39. **Engineering Lead:** At what exact point is a successful offline record removed from local storage?
40. **Security Owner:** What local database and AES-256 mode or key-management approach are implemented?
41. **Security Owner:** How does the platform prevent offline data export, copy, backup leakage, screenshots, or access from rooted devices?
42. **Product Manager:** Can an Agent edit a record after successful synchronization?
43. **Product Manager:** What information appears on the Temporary Registration Slip, and in what file or print format?
44. **Product Manager:** Is the offline SMS draft sent automatically after synchronization or only through a separate approved action?
45. **Engineering Lead:** What SMS provider and sandbox are available for QA?
46. **Product Manager:** What are the approved registration, redemption, and broadcast SMS templates?
47. **Engineering Lead:** What retry, delivery-status, and duplicate-prevention rules apply to SMS?
48. **Product Manager:** What happens if registration succeeds but SMS delivery fails?
49. **Product Manager:** What is the exact lifecycle for agent invitation, activation, suspension, deletion, and reassignment?
50. **Product Manager:** Does deleting an Agent deactivate the account or permanently remove the record?
51. **Product Manager:** What happens to pending offline records and active assignments when an Agent is suspended or reassigned?
52. **Product Manager:** Which agent performance metrics appear on the Admin dashboard?
53. **Product Manager:** What defines an active Agent?
54. **Product Manager:** What is the cooperative registration, approval, rejection, and membership lifecycle?
55. **Product Manager:** Can a farmer belong to multiple cooperatives?
56. **Product Manager:** What determines farmer or cooperative eligibility for an input program?
57. **Product Manager:** What quantity, unit, inventory, and allocation-limit rules apply to each input category?
58. **Product Manager:** Can an allocation be edited or cancelled after creation?
59. **Product Manager:** Does redemption support partial quantities?
60. **Product Manager:** Can a redemption be reversed or corrected?
61. **Product Manager:** What prevents the same allocation from being redeemed twice?
62. **Engineering Lead:** Does IDN scanning use QR code, barcode, text recognition, or manual input only?
63. **Product Manager:** What happens when connectivity fails after the Redemption Agent confirms but before receiving the response?
64. **Product Manager:** Is the Redemption Agent permitted to view the complete farmer profile or only identity and allocation details?
65. **Product Manager:** What time zone and date format should SMS and audit logs use?
66. **Product Manager:** What formulas define dashboard totals, agent performance, yield projections, redemption rates, and activity breakdowns?
67. **Engineering Lead:** How frequently do Admin HQ dashboard aggregates refresh?
68. **Product Manager:** What records and filters must each CSV, XLSX, and PDF export contain?
69. **Security Owner:** Which roles may export farmer PII?
70. **Engineering Lead:** What are the export row limits, generation timeout, retention period, and secure-download rules?
71. **Product Manager:** What farmer services are included in V1 mobile web and USSD?
72. **Engineering Lead:** What USSD code, aggregator, session timeout, and test simulator are available?
73. **Product Manager:** Does the Farmer Client account use phone/password, OTP, PIN, or another authentication model?
74. **Engineering Lead:** What API contract, authentication method, payload schema, and transmission frequency apply to the Governor’s Agricultural Dashboard?
75. **Engineering Lead:** How are failed or duplicate government-dashboard transmissions detected and recovered?
76. **Product Manager:** What difference is expected between “real-time” registration/redemption updates and eventual dashboard aggregation?
77. **Engineering Lead:** What OpenAPI, event, queue, database, synchronization, and SMS documentation is available?
78. **Engineering Lead:** What seeded farmer, agent, cooperative, allocation, and program data is available in QA?
79. **Engineering Lead:** Is there a reset or cleanup API for generated farmers, allocations, redemptions, and messages?
80. **QA Lead:** Which repetitive checks must run after every deployment, and which run only before release promotion?
81. **QA Lead:** What is the approved cadence for smoke, critical regression, bug regression, integration, extended, and stress suites?
82. **Engineering Lead:** What concurrency, queue-size, throughput, and duration limits are approved for QA stress testing?
83. **QA Lead:** What defect-management system and mandatory evidence fields should automation use?
84. **Operations Lead:** Which Production logs, alerts, SMS delivery reports, synchronization dashboards, and error tools are available to QA?
85. **Operations Lead:** Which exact Production smoke and sanity checks are authorized?
86. **Release Manager:** What rollback scenarios must QA support, and what defines successful rollback verification?
87. **Support Lead:** Where is Agent and Farmer feedback recorded, and how should QA correlate it with releases and defects?
88. **Product Manager:** What browsers, mobile operating systems, device classes, screen sizes, accessibility standards, and languages must be supported?
89. **Security Owner:** What technical guard must prevent QA registration, redemption, bulk-message, merge, stress, or destructive suites from targeting Production?
90. **Security Owner:** Should the supplied planning credentials be rotated and stored in an approved secret manager before automation implementation?
