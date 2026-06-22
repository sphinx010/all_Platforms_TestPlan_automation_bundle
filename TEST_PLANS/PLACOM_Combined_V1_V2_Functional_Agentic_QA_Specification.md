# PLACOM Digital Platform — Functional and Agentic QA Specification

- **Source:** PLACOM Stakeholder PRD and PLACOM Product Requirements Document Version 2.0
- **Product Type:** Agricultural commodity marketplace and integrated supply-chain operating platform
- **Specification Status:** Draft — derived from the original stakeholder PRD, the expanded Version 2 PRD, and the supplied portal access; no browser, API, payment, webhook, warehouse-system, logistics-provider, bank, tax, exchange, or production verification has been performed
- **Scope Note:** Version 2 is treated as the current product direction and the Stakeholder PRD as the original baseline. Active QA automation is limited to the supplied Farmer, Logistics, Warehouse Owner, and Admin access. Within those roles, the automation target is repetitive, deterministic, currently accessible behaviour. Buyer/Processor, Cooperative, Finance/Approver, Bank, Tax Authority, Commodity Exchange, government-database, and other unsupported roles or integrations remain **Pending** until routes, credentials, implementation status, and test contracts are supplied. Route optimization is explicitly future functionality.
- **Environment Note:** The supplied URLs and credentials provide partial platform access. The Farmer, Logistics, Warehouse Owner, and Admin roles are available. The environment classification is not explicitly stated by the user; the URLs are therefore treated as the supplied test environment pending confirmation. No dedicated Production URLs or Production-safe test accounts are supplied.
- **Automation Test Basis:** Automation candidates are selected from PRD-confirmed workflows and filtered by available role access, repetition, determinism, business relevance, implementation observability, data safety, and maintenance value. Unavailable or unverified coverage remains Pending.
- **Environment Execution Policy:** Use the supplied test environment for automated smoke, repetitive regression, bug-fix validation, RBAC, data-persistence, negative, and controlled API/integration checks. Preserve manual effort for exploratory testing, subjective usability, ambiguous behaviour, novel risk, real financial settlement, and unsupported external integrations. Production is restricted to authorized, non-destructive smoke, sanity, monitoring, and rollback support.

---

## 1. Platform Access and Roles

### QA Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
| ------ | ------------ | ---------- | -------- | -------- | ----------------------------- |
| PLACOM Farmer / Marketplace Portal | Farmer | `https://placom-ng-197301616810.europe-west1.run.app/` | `07034456894` | `Oz12345678$` | `PLACOM_QA_CLIENT_URL`, `PLACOM_QA_FARMER_PHONE`, `PLACOM_QA_FARMER_PASSWORD` |
| PLACOM Logistics Portal | Logistics Provider | `https://placom-ng-197301616810.europe-west1.run.app/` | `logisticsman@mailinator.com` | `Password01` | `PLACOM_QA_CLIENT_URL`, `PLACOM_QA_LOGISTICS_EMAIL`, `PLACOM_QA_LOGISTICS_PASSWORD` |
| PLACOM Warehouse Portal | Warehouse Owner / Operator | `https://placom-ng-197301616810.europe-west1.run.app/` | `levi@mailinator.com` | `Oz12345678$` | `PLACOM_QA_CLIENT_URL`, `PLACOM_QA_WAREHOUSE_EMAIL`, `PLACOM_QA_WAREHOUSE_PASSWORD` |
| PLACOM Admin Portal | Admin | `https://placom-admin-197301616810.us-central1.run.app/` | `chibuezeozojideofor@gmail.com` | `Oz12345678$` | `PLACOM_QA_ADMIN_URL`, `PLACOM_QA_ADMIN_EMAIL`, `PLACOM_QA_ADMIN_PASSWORD` |
| Buyer / Processor Portal | Pending | `<ENTER_BUYER_PORTAL_URL>` | `<ENTER_BUYER_EMAIL>` | `<ENTER_BUYER_PASSWORD>` | `PLACOM_QA_BUYER_URL`, `PLACOM_QA_BUYER_EMAIL`, `PLACOM_QA_BUYER_PASSWORD` |
| Cooperative Portal | Pending | `<ENTER_COOPERATIVE_PORTAL_URL>` | `<ENTER_COOPERATIVE_EMAIL>` | `<ENTER_COOPERATIVE_PASSWORD>` | `PLACOM_QA_COOPERATIVE_URL`, `PLACOM_QA_COOPERATIVE_EMAIL`, `PLACOM_QA_COOPERATIVE_PASSWORD` |
| Finance / Approval Portal | Pending | `<ENTER_FINANCE_PORTAL_URL>` | `<ENTER_FINANCE_EMAIL>` | `<ENTER_FINANCE_PASSWORD>` | `PLACOM_QA_FINANCE_URL`, `PLACOM_QA_FINANCE_EMAIL`, `PLACOM_QA_FINANCE_PASSWORD` |

**Active automation scope:** Farmer, Logistics Provider, Warehouse Owner, and Admin only. Unsupported roles remain Pending and must not be represented as active automated coverage.

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
| ------ | ------------ | ---------- | -------- | -------- | ----------------------------- |
| PLACOM Production Marketplace | Farmer / Operational Roles | `<ENTER_PRODUCTION_CLIENT_URL>` | `<ENTER_PRODUCTION_USER>` | `<ENTER_PRODUCTION_PASSWORD>` | `PLACOM_PROD_CLIENT_URL`, `PLACOM_PROD_USER`, `PLACOM_PROD_PASSWORD` |
| PLACOM Production Admin | Admin | `<ENTER_PRODUCTION_ADMIN_URL>` | `<ENTER_PRODUCTION_ADMIN_EMAIL>` | `<ENTER_PRODUCTION_ADMIN_PASSWORD>` | `PLACOM_PROD_ADMIN_URL`, `PLACOM_PROD_ADMIN_EMAIL`, `PLACOM_PROD_ADMIN_PASSWORD` |

Production automation must remain limited to explicitly authorized, non-destructive smoke, health, and sanity checks. Do not run commodity listing, warehouse onboarding, capacity changes, vehicle creation, dispatch, delivery confirmation, payment, settlement, escrow, fee deduction, reconciliation, bulk data, deletion, or load tests in Production without written authorization and isolated synthetic accounts.

---

## 2. Feature List

### A. Identity, Authentication, and Role Access

- **Farmer Authentication — Active:** The supplied Farmer account accesses the marketplace/client portal.
- **Logistics Authentication — Active:** The supplied Logistics account accesses logistics-related functions available through the client portal.
- **Warehouse Owner Authentication — Active:** The supplied Warehouse Owner account accesses warehouse-related functions available through the client portal.
- **Admin Authentication — Active:** The supplied Admin account accesses the dedicated Admin portal.
- **Role-Based Access — Active:** Each supplied role must see only its permitted navigation, dashboards, records, and actions.
- **Direct-Route Protection — Active:** A user must not gain access to another role’s modules by entering a direct URL.
- **API Authorization — Active where observable:** Server-side authorization must enforce the same role boundaries as the UI.
- **Session Handling — Active:** Login, authenticated navigation, logout, invalid credentials, expired session, and post-logout access are repetitive automation candidates.
- **Additional Roles — Pending:** Buyer/Processor, Cooperative, Finance/Approver, bank, regulator, investor, and other stakeholder roles require credentials and implemented routes before active automation.

### B. Farmer Marketplace and Produce Operations

- **Farmer Dashboard — Active where implemented:** Farmer-facing navigation may include produce management, market information, storage, sales, receipts, loans, or transaction history.
- **Produce Listing or Submission — Active where implemented:** Repetitive create, validate, submit, view, and status-check operations for produce records are automation candidates.
- **Direct Sale Flow — Active only to the furthest accessible point:** Farmer submits produce and chooses the direct-sale path. Holding-warehouse verification, quality approval, buyer completion, and real payment remain Pending unless the supplied roles expose those steps safely.
- **Storage Flow — Active only to the furthest accessible point:** Farmer selects storage or warehouse deposit. Warehouse verification and receipt handling can be automated where the Farmer and Warehouse Owner portals expose the connected states.
- **Market Prices and Trends — Active where visible:** Farmer can repeatedly view commodity prices, stock, grades, warehouse locations, or trends.
- **Farmer Record Persistence — Active:** Created or edited Farmer records must persist after refresh and relogin.
- **Warehouse-Receipt Loan Application — Pending:** No bank or finance test access is supplied.
- **Buyer Purchase Completion — Pending:** No Buyer/Processor account is supplied.
- **Payment Confirmation — Pending:** Real or sandbox settlement details are not supplied.

### C. Warehouse Management and Digitization

- **Warehouse Profile — Active:** Warehouse Owner may create or maintain warehouse identity and operational information where implemented.
- **General Warehouse Information — Active where implemented:** Name, State, LGA, Town, GPS coordinates, ownership type, category, storage capacity, storage type, establishment year, and renovation year.
- **Operational Details — Active where implemented:** Commodities stored, capacity by commodity, current occupancy, receipt-system availability, laboratory capability, power source, and internet connectivity.
- **Personnel and Access — Active where implemented:** Warehouse manager, quality officer, staff count, access-control method, and security arrangement.
- **Warehouse Onboarding — Active:** Repetitive form completion, validation, submission, resumption, and Admin review are high-value automation candidates where both roles expose the workflow.
- **Warehouse Accreditation — Active where implemented:** Admin review, scoring, approval, rejection, or status change may be automated if the current Admin portal exposes it.
- **Capacity Tracking — Active:** Capacity and occupancy changes should persist and reconcile across Warehouse Owner and Admin views.
- **Warehouse Performance Analytics — Active where visible:** Repetitive dashboard and aggregate checks may be automated after calculation rules are confirmed.
- **Quality Grading and Certification — Active only where exposed:** Repetitive record capture is in scope; biochemical laboratory integrations remain Pending.
- **Warehouse Receipt Issuance — Active where exposed:** The Warehouse Owner can issue or manage receipts only where the workflow is accessible.
- **External WMS Integration — Pending:** No WMS sandbox, API documentation, or test credentials are supplied.
- **Farmer Protection Fund — Pending:** No implementation or access details are supplied.

### D. Logistics and Transportation

- **Transport Provider Profile — Active:** Logistics Provider can maintain company, service type, coverage, fleet size, and truck-capacity details where implemented.
- **Vehicle Registry — Active:** Create, view, update, filter, and validate vehicle type, plate number, capacity, commodity compatibility, availability, and tracking method.
- **Route and Cost Records — Active where implemented:** Route, cost per ton, loading point, delivery point, turnaround time, and insurance information.
- **Smart Dispatch — Active where implemented:** Repetitive assignment, acceptance, scheduling, status transition, and delivery confirmation are automation candidates where an order can be created or supplied by Admin.
- **Shipment Tracking — Active where visible:** Status and tracking updates should persist and remain consistent between Logistics and Admin views.
- **Logistics Marketplace Matching — Active where implemented:** Repetitive provider matching and assignment may be automated using supplied Logistics and Admin access.
- **Delivery Confirmation — Active:** Completion, duplicate confirmation prevention, and final status persistence are automation candidates.
- **GPS Tracking Integration — Pending:** No GPS provider or webhook sandbox is supplied.
- **External Logistics API — Pending:** No integration credentials or contract are supplied.
- **Route Optimization — Pending/Future:** Explicitly identified as future functionality and excluded from current automation.

### E. Admin Oversight and Governance

- **User and Role Oversight — Active where implemented:** Admin can view or manage supplied platform personas according to current permissions.
- **Farmer Oversight — Active where implemented:** Admin may view Farmer records, produce submissions, storage or sale requests, and statuses.
- **Warehouse Oversight — Active:** Admin may review warehouses, accreditation, capacity, occupancy, stock, and performance data.
- **Logistics Oversight — Active:** Admin may view providers, vehicles, dispatches, tracking, and delivery status.
- **Commodity Configuration — Active where implemented:** Admin may manage commodities, grades, categories, pricing data, or market visibility.
- **Dashboard and Analytics — Active:** Repetitive totals, filters, tables, and cross-role data reconciliation are high-value automation targets.
- **Audit Trail Visibility — Active where implemented:** Warehouse, logistics, and administrative changes should be traceable.
- **Financial Approvals — Pending:** No Finance/Approver credential is supplied.
- **Tax Administration — Pending:** No tax-authority access or sandbox is supplied.
- **Exchange Administration — Pending:** No NCX or LCFE integration environment is supplied.

### F. Cooperative Management

- **Cooperative Registration — Pending:** No Cooperative role or credentials are supplied.
- **Member Management — Pending:** Member names, phone numbers, National ID/BVN, farm size, commodities, and production volumes cannot be actively automated without Cooperative access.
- **Production and Supply Tracking — Pending:** Expected production, supplied quantity, warehouse deposits, receipts, and sales require Cooperative or Admin implementation confirmation.
- **Cooperative Financial Tracking — Pending:** Payments, loans, input financing, subsidy, and grant tracking require safe financial test access.
- **Cooperative Operations — Pending:** Input logs, extension services, training, and contracts remain pending.
- **Cross-Module Cooperative Flow — Pending:** Aggregation, warehouse deposit, listing, buyer purchase, and member/cooperative payment distribution require unsupported roles and integrations.

### G. Commodity Trading and Market Intelligence

- **Public Market Information — Active where visible:** Commodity availability, grades, warehouse stock, prices, and warehouse locations are repetitive read-only automation candidates.
- **Price Discovery — Active where implemented:** Search, filter, sorting, and displayed price persistence can be automated.
- **Farmer Produce Submission — Active where implemented:** Farmer-side listing or submission is active.
- **Buyer Search and Purchase — Pending:** No Buyer/Processor account is supplied.
- **Trade Settlement — Pending:** No safe payment or settlement access is supplied.
- **Commodity Exchange Integration — Pending:** NCX and LCFE contracts are unavailable.
- **Public Homepage Accessibility — Active where implemented:** Public market data can be checked without authentication where exposed.

### H. Financial Settlement, Fees, and Compliance

- **Role-Based Financial Controls — Pending:** No Finance or approval account is supplied.
- **Farmer, Cooperative, Warehouse, Logistics, Trade, and Procurement Payments — Pending:** No sandbox settlement credentials or test funds are supplied.
- **POS and Bank Transfer Methods — Pending:** No payment sandbox is supplied.
- **Escrow — Pending:** No provider, account, callback, or dispute workflow is supplied.
- **Automated Fee Deduction — Pending:** Storage, handling, commission, and other deduction rules require confirmed formulas and safe test settlement.
- **Payment Reconciliation — Pending:** No reconciliation file, API, or role access is supplied.
- **KYC and Identity Validation — Pending except visible form validation:** External KYC contracts are not supplied.
- **VAT and Raw-Produce Exemption — Pending:** Tax rules, rates, rounding, and authority integration require confirmation.
- **Audit Trail — Active where current Admin access exposes it:** Non-financial platform changes can be verified without executing real settlement.

### I. Reporting, Exports, and Integrations

- **Farmer, Warehouse, Logistics, and Admin Reports — Active where implemented:** Repetitive filtering, report generation, download, data persistence, and visible totals are automation candidates.
- **Warehouse Analytics — Active where implemented:** Capacity and occupancy reporting may be automated after formulas are known.
- **Logistics Analytics — Active where implemented:** Delivery time and provider performance may be automated after calculation rules are known.
- **Market Transparency Dashboard — Active where visible:** Commodity, grade, stock, and price information can be checked repeatedly.
- **Government Farmer Database Integration — Pending:** No PADP, Plateau Agric, ASTC, or government-database test contract is supplied.
- **Bank Integration — Pending:** No settlement-bank sandbox is supplied.
- **Tax Integration — Pending:** No tax-authority test environment is supplied.
- **Commodity Exchange Integration — Pending:** No NCX or LCFE test access is supplied.
- **REST and Webhook Integration — Pending unless observable:** Contracts must be discovered safely or supplied before active integration automation.

---

## 3. User Stories

> **As a Farmer,** I want to sign in and view my permitted marketplace functions, so that I can manage my agricultural trading activities securely.

> **As a Farmer,** I want to submit produce for direct sale or storage where available, so that I can access PLACOM’s commodity market and warehouse network.

> **As a Farmer,** I want to view commodity prices, grades, stock, and warehouse information, so that I can make informed sale or storage decisions.

> **As a Warehouse Owner,** I want to maintain my warehouse profile and operational capacity, so that PLACOM has current storage information.

> **As a Warehouse Owner,** I want to onboard or submit my warehouse for accreditation, so that it can participate in PLACOM operations.

> **As a Warehouse Owner,** I want to record deposits, stock, quality details, and receipts where implemented, so that stored commodities remain traceable.

> **As a Logistics Provider,** I want to maintain my company and vehicle records, so that PLACOM can match me to suitable transport jobs.

> **As a Logistics Provider,** I want to accept, track, and complete assigned shipments where available, so that commodity movement remains visible and accountable.

> **As an Admin,** I want to oversee Farmers, warehouses, Logistics Providers, commodities, and operational records, so that PLACOM maintains accurate ecosystem data.

> **As an Admin,** I want to review warehouse onboarding and accreditation records, so that only approved warehouses participate.

> **As an Admin,** I want dashboard totals and reports to reconcile with underlying Farmer, warehouse, and logistics records, so that operational decisions are based on accurate data.

> **As an Admin,** I want role boundaries enforced in the UI and API, so that users cannot access or change data outside their responsibilities.

> **As a Buyer or Processor,** I want to search and purchase certified commodities, so that I can obtain reliable supply. **Status: Pending — no account supplied.**

> **As a Cooperative user,** I want to register members, aggregate produce, and track supply and payments, so that cooperative activity remains structured. **Status: Pending — no account supplied.**

> **As a Finance or Approval user,** I want to approve, settle, and reconcile transactions, so that payments remain controlled and auditable. **Status: Pending — no account supplied.**

---

## 4. Core User Flows in Gherkin Syntax

The scenarios below define the active repetitive automation backlog for the supplied Farmer, Logistics Provider, Warehouse Owner, and Admin roles. Steps that require Buyer, Cooperative, Finance, Bank, Tax, Exchange, or other unavailable access stop at the last safely observable state and remain Pending.

### Flow 1: Authentication, Session, and Role Navigation

```gherkin
Feature: PLACOM role-based authentication

  Scenario Outline: A supplied role signs in to the correct portal
    Given the <role> account is active
    And its URL and credentials are loaded from environment variables
    When the user submits valid credentials
    Then an authenticated session is established
    And the role-appropriate dashboard or landing page is displayed
    And prohibited role navigation is not available
    And credentials and session values are not exposed in logs or reports

    Examples:
      | role            |
      | Farmer          |
      | Logistics       |
      | Warehouse Owner |
      | Admin           |

  Scenario: A user attempts direct access to another role's restricted route
    Given the user is authenticated with one of the supplied roles
    And the target route requires a different role
    When the user opens the restricted route directly
    Then access is denied
    And protected data is not returned
    And no underlying record is changed
```

### Flow 2: Farmer Produce Submission

```gherkin
Feature: Farmer produce submission

  Scenario: A Farmer creates a valid produce record
    Given the Farmer is authenticated
    And the produce-submission function is available
    When the Farmer enters all required commodity details
    And chooses the available sale or storage option
    And submits the record
    Then validation succeeds
    And the produce record is persisted
    And a visible reference or status is assigned
    And the record remains available after refresh or relogin
    And no payment or buyer-completion state is asserted without the required access
```

### Flow 3: Farmer Market Search and Filtering

```gherkin
Feature: Commodity market visibility

  Scenario Outline: A Farmer filters available market information
    Given the Farmer is authenticated or the market page is publicly accessible
    And market data is available
    When the user filters by <criterion>
    Then only records matching the selected criterion are displayed
    And the displayed grade, quantity, price, stock, or warehouse values remain internally consistent

    Examples:
      | criterion          |
      | commodity          |
      | grade              |
      | warehouse location |
      | price range        |
```

### Flow 4: Warehouse Onboarding and Persistence

```gherkin
Feature: Warehouse onboarding

  Scenario: A Warehouse Owner submits a warehouse profile
    Given the Warehouse Owner is authenticated
    And the onboarding function is available
    When the user enters the required warehouse identity, location, capacity, storage, and operational information
    And submits the profile
    Then field validation is applied
    And the warehouse record is persisted
    And an onboarding or review status is displayed
    And the record remains visible after refresh or relogin
    And the corresponding Admin view reflects the new or updated record where implemented
```

### Flow 5: Warehouse Capacity and Occupancy Update

```gherkin
Feature: Warehouse capacity tracking

  Scenario: A Warehouse Owner updates operational capacity
    Given an existing warehouse profile is available
    And the user has permission to edit capacity information
    When the user changes the permitted capacity or occupancy values
    And saves the update
    Then invalid negative or inconsistent values are rejected
    And valid values are persisted
    And the Admin view displays the same accepted values where implemented
    And any displayed available-capacity calculation is recalculated according to the configured rules
```

### Flow 6: Logistics Provider and Vehicle Registry

```gherkin
Feature: Logistics provider operations

  Scenario: A Logistics Provider registers a vehicle
    Given the Logistics Provider is authenticated
    And the vehicle-registry function is available
    When the user enters a unique plate number
    And enters vehicle type, capacity, commodity compatibility, availability, and tracking method
    And submits the record
    Then required field validation is applied
    And the vehicle record is persisted
    And the vehicle is visible in the Logistics account
    And the vehicle is visible to Admin where implemented

  Scenario: A duplicate vehicle plate is submitted
    Given a vehicle already exists with the same normalized plate number
    When the Logistics Provider submits another vehicle with that plate number
    Then the duplicate is rejected or routed through the configured duplicate process
    And a second active vehicle is not silently created
```

### Flow 7: Logistics Dispatch and Delivery Status

```gherkin
Feature: Shipment workflow

  Scenario: A Logistics Provider processes an assigned shipment
    Given the Logistics Provider is authenticated
    And an eligible shipment or transport order is available
    When the provider accepts or is assigned the shipment
    Then the shipment reflects the configured assigned or scheduled state
    When the provider records the permitted progress states
    Then each accepted state persists
    And the Admin view reflects the same current state where implemented
    When the provider confirms delivery
    Then the shipment reaches the configured completed state
    And duplicate completion is prevented
```

### Flow 8: Admin Warehouse Review and Accreditation

```gherkin
Feature: Warehouse administrative review

  Scenario Outline: Admin reviews a submitted warehouse
    Given the Admin is authenticated
    And a warehouse record is awaiting review
    When the Admin selects <decision>
    Then the required decision data is validated
    And the warehouse status changes according to the decision
    And the Warehouse Owner sees the resulting status
    And the decision remains traceable

    Examples:
      | decision |
      | approve  |
      | reject   |
```

### Flow 9: Admin Dashboard Reconciliation

```gherkin
Feature: Operational dashboard accuracy

  Scenario Outline: Admin totals reconcile with source records
    Given the Admin is authenticated
    And a controlled baseline exists for <record_type>
    When one valid synthetic record is created or updated through the permitted role
    Then the relevant Admin list reflects the record
    And the related count or aggregate changes by the expected amount
    And filtering the source list produces data consistent with the displayed total

    Examples:
      | record_type |
      | farmer      |
      | warehouse   |
      | vehicle     |
      | shipment    |
```

---

## 5. Expected Outcomes and Success Measures

### Active Functional Outcomes

- All four supplied roles can authenticate through their correct portals.
- Invalid or expired sessions are denied.
- Role navigation, direct routes, and observable APIs enforce access boundaries.
- Farmer produce records persist after submission where the workflow is implemented.
- Farmer market filters return internally consistent data.
- Warehouse profiles persist and remain visible to the Warehouse Owner and Admin where applicable.
- Warehouse capacity and occupancy values reject invalid inputs and reconcile across views.
- Logistics Provider and vehicle records persist.
- Duplicate vehicle creation is prevented.
- Shipment assignment, progress, and delivery status persist where implemented.
- Admin warehouse decisions update the Warehouse Owner’s view.
- Admin dashboard totals reconcile with controlled source records.
- Repetitive fixed defects become permanent regression tests.

### Pending Functional Outcomes

The following outcomes remain Pending because the required roles, contracts, or safe test environments are unavailable:

- Buyer purchase completion
- Cooperative aggregation and member management
- Payment approval and settlement
- Escrow funding and release
- Bank transfer, POS, NIP, USSD, or e-payment confirmation
- Automated fee deduction
- Reconciliation
- Warehouse-receipt loan processing
- VAT calculation and remittance
- Government farmer-database verification
- NCX and LCFE market-data synchronization
- External WMS synchronization
- External GPS tracking
- Full end-to-end cooperative supply flow

### PRD-Defined Strategic and KPI Outcomes

The original Stakeholder PRD defines Year 1 targets:

- **30,000+** registered Farmers
- **50+** accredited warehouses
- **₦5B+** commodity transaction volume
- **5,000+** Farmers accessing collateralized loans
- **₦500M+** commission and certification income
- **100%** public accessibility of market transparency data

Version 2 adds KPI categories without numeric targets:

- Percentage of onboarded warehouses
- Average delivery time
- Payment success rate
- Number of registered cooperatives
- Percentage of structured and complete records

These business KPIs are reporting requirements, not direct automation pass criteria until the formulas, data sources, time windows, and acceptance thresholds are confirmed.

### Security and Audit Outcomes

- Supplied roles cannot access unauthorized functions.
- Secrets and personal data are masked in automation evidence.
- Changes to Farmer, warehouse, vehicle, shipment, and Admin-controlled records remain attributable where audit functionality is implemented.
- Financial and external integrations are not exercised without safe test facilities.
- Production execution remains non-destructive.

### Measures Not Yet Defined

The PRDs do not define field-level validation rules, status enumerations, upload limits, response-time thresholds, report-generation time, dashboard refresh intervals, search latency, transaction timeout, webhook retry limits, idempotency keys, warehouse-scoring formulas, logistics-matching formulas, payment formulas, fee percentages, VAT rules, price-discovery formulas, browser support, accessibility criteria, or supported mobile devices.

---

## 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
| ------------ | ------------------ | ---------------------------- |
| Authentication and Sessions | Login, logout, session persistence, role landing | Likely identity/session API with role claims; confirm through network inspection |
| Role-Based Access | Farmer, Logistics, Warehouse Owner, and Admin restrictions | Front-end route guards plus server-side authorization middleware |
| Farmer Produce Records | Create, view, and track sale or storage submissions | Likely REST API and database persistence |
| Market Search | Commodity, grade, quantity, price, stock, and warehouse filtering | Likely market-data query API and indexed persistence |
| Warehouse Profile | Onboarding and operational metadata | Likely warehouse REST API and structured database |
| Warehouse Capacity | Capacity, occupancy, and availability calculation | Likely warehouse inventory service; formula requires confirmation |
| Accreditation | Admin review, scoring, and status | Likely workflow API and audit-event persistence |
| Warehouse Receipt | Issue and associate receipt with deposit | Likely receipt service and document generation; implementation requires confirmation |
| Logistics Provider | Company, service, coverage, and fleet data | Likely logistics-provider API |
| Vehicle Registry | Vehicle identity, capacity, availability, and tracking method | Likely vehicle CRUD API and database |
| Dispatch | Match or assign provider and schedule movement | Possible workflow service or dispatch engine; matching logic requires confirmation |
| Shipment Tracking | Progress and delivery status | Possible API, background processing, or external tracking webhook |
| Admin Dashboard | Aggregate Farmer, warehouse, vehicle, shipment, and market data | Likely analytics/query API; calculation and refresh rules require confirmation |
| Reports and Exports | Generate operational reports | Possible reporting service and temporary file storage |
| Audit Trail | Trace warehousing, logistics, and administrative changes | Likely append-only audit/event service |
| Cooperative Management | Registration, membership, supply, and payments | Pending; likely cooperative API and cross-module workflow |
| Payment and Settlement | Payments, escrow, fee deduction, reconciliation | Pending; likely bank/payment provider APIs, callbacks, and ledger |
| Tax Compliance | VAT and exemption handling | Pending; likely tax calculation and reporting integration |
| Government Farmer Verification | Verify Farmer records | Pending; likely government database API |
| Commodity Exchanges | Market pricing and availability | Pending; likely external REST feeds or webhooks |
| External WMS | Warehouse data ingestion and synchronization | Pending; provider-specific API or database integration |
| GPS Tracking | Real-time shipment location | Pending; external tracking API or webhook |

---

## 7. Integration-Based Gherkin Flows

Active integration automation is limited to behaviour observable through the supplied Farmer, Logistics, Warehouse Owner, and Admin access. External financial, government, exchange, WMS, and GPS integrations remain Pending.

### Integration Flow 1: Cross-Role Warehouse Synchronization

```gherkin
Feature: Warehouse Owner and Admin data consistency

  Scenario: Warehouse profile changes synchronize across role views
    Given a Warehouse Owner has an existing warehouse profile
    And the Admin can view that warehouse
    When the Warehouse Owner submits a valid profile or capacity update
    Then the accepted update is persisted
    And the Admin view reflects the same accepted values
    And repeated page refresh does not revert the values
```

### Integration Flow 2: Logistics and Admin Shipment Consistency

```gherkin
Feature: Shipment status synchronization

  Scenario: A Logistics status update appears in Admin oversight
    Given an eligible shipment exists
    And the Logistics Provider is permitted to update its status
    When the provider records a valid status transition
    Then the new status is persisted
    And the Admin view displays the same current status
    And a stale refresh does not overwrite the accepted state
```

### Integration Flow 3: Duplicate Submission Protection

```gherkin
Feature: Idempotent operational submissions

  Scenario Outline: A repeated submission does not create duplicate active records
    Given a valid <record_type> submission has been accepted
    When the same submission is repeated due to retry or double-click
    Then the platform creates only one effective active record
    And the user receives the existing result or the configured duplicate response

    Examples:
      | record_type       |
      | produce record    |
      | warehouse profile |
      | vehicle           |
      | delivery action   |
```

### Integration Flow 4: Dashboard Aggregate Refresh

```gherkin
Feature: Operational data aggregation

  Scenario: A committed operational record updates Admin reporting
    Given a known Admin dashboard baseline exists
    When a valid Farmer, warehouse, vehicle, or shipment record is committed
    Then the source list displays the record
    And the relevant dashboard aggregate updates according to the configured refresh process
    And the displayed aggregate reconciles with the filtered source records
```

### Pending Integration Flow 5: Payment Callback and Reconciliation

```gherkin
Feature: Payment confirmation and reconciliation

  Scenario: A successful payment callback updates the related transaction
    Given a safe payment sandbox and Finance access are available
    And a pending PLACOM transaction exists
    When the approved payment provider confirms settlement
    Then the transaction is marked according to the confirmed settlement state
    And applicable fees are recorded
    And duplicate callbacks do not duplicate settlement

  # Status: Pending — provider contract, Finance access, callback rules, and test funds are unavailable.
```

### Pending Integration Flow 6: Government Farmer Verification

```gherkin
Feature: Government Farmer identity verification

  Scenario: PLACOM receives a Farmer verification result
    Given a government-database sandbox and contract are available
    And a Farmer record requires verification
    When the external verification result is received
    Then the Farmer record is updated according to the confirmed result
    And stale or duplicate responses do not corrupt the record

  # Status: Pending — no government-database test contract or credentials are supplied.
```

### Pending Integration Flow 7: Commodity Exchange Market Data

```gherkin
Feature: External commodity-price synchronization

  Scenario: Exchange data updates PLACOM market visibility
    Given an approved commodity-exchange test feed is available
    When new market data is accepted
    Then the relevant commodity prices and timestamps are updated
    And malformed or duplicate data does not replace valid current values

  # Status: Pending — NCX and LCFE test access and contracts are unavailable.
```

### Pending Integration Flow 8: External Shipment Tracking

```gherkin
Feature: Logistics tracking callback

  Scenario: A tracking update changes shipment visibility
    Given a shipment is linked to an approved external tracker
    When a valid tracking event is received
    Then the shipment location or progress state is updated
    And the Logistics and Admin views remain consistent
    And duplicate events do not create duplicate effective transitions

  # Status: Pending — no GPS or external logistics integration sandbox is supplied.
```

---

## 8. System Instruction for the PLACOM QA Automation Agent

### Role

You are the **Senior QA Automation Engineer for the PLACOM Digital Platform**. You are responsible for browser, API, integration, RBAC, data-persistence, and regression automation.

Your active automation scope is restricted to the supplied roles:

- Farmer
- Logistics Provider
- Warehouse Owner
- Admin

Do not treat Buyer, Processor, Cooperative, Finance, Approver, Bank, Tax Authority, Commodity Exchange, government-database, investor, or other unsupported roles as active coverage. Mark them **Pending** until the required portal, credential, implementation, and test contract are supplied.

Your primary objective is to automate **repetitive, deterministic checks** so that manual QA can concentrate on exploratory testing, high-risk investigation, usability, product ambiguity, and novel workflows.

### Platform Overview

PLACOM began as an agricultural commodity marketplace and Version 2 expands it into an integrated supply-chain platform.

The current product scope includes:

- Farmer marketplace and produce operations
- Warehouse onboarding and digitization
- Warehouse capacity and occupancy
- Logistics Provider and vehicle management
- Dispatch and shipment tracking
- Admin oversight and analytics
- Commodity market visibility
- Cooperative management
- Payment and settlement
- Commodity trading
- Government, bank, tax, exchange, WMS, and logistics integrations

Only Farmer, Logistics Provider, Warehouse Owner, and Admin are currently accessible for active QA automation.

### Environment and Secret Management

- Use environment variables for every URL, credential, token, file path, and integration setting.
- Never hard-code secrets in tests, fixtures, support files, logs, screenshots, videos, reports, or commits.
- Keep supplied test-environment and Production configurations isolated.
- Treat the current URLs as test-environment URLs until the owner confirms classification.
- Mask phone numbers, email addresses, personal IDs, bank details, transaction details, and authentication values in evidence.
- Use synthetic Farmers, produce records, warehouses, vehicles, routes, shipments, commodities, and Admin records.
- Do not execute real settlement, bank transfer, escrow, VAT, loan, or payment tests without a sandbox and written authorization.
- Restrict Production to approved read-only smoke, sanity, monitoring, and rollback checks.

### Core Responsibilities

#### 1. PRD Decomposition and Traceability

- Treat Version 2 as the current direction and the Stakeholder PRD as the baseline.
- Preserve baseline requirements that Version 2 does not explicitly replace.
- Separate requirements into:
  - Active and accessible
  - Active but unverified
  - Pending due to missing role
  - Pending due to missing integration
  - Future
- Assign stable identifiers such as:
  - `AUTH-FARMER-001`
  - `WAREHOUSE-ONBOARD-002`
  - `LOGISTICS-VEHICLE-003`
  - `ADMIN-DASH-004`
  - `PENDING-PAYMENT-001`
- Maintain a PRD-to-test matrix containing:
  - Requirement
  - Source version
  - Role
  - Portal
  - Coverage status
  - Automation suitability
  - Automated-test ID
  - Manual coverage
  - Last result
  - Defect or clarification dependency
- Never mark Pending requirements as automated.

#### 2. Environment and Network Audit

- Audit the Farmer, Logistics, Warehouse Owner, and Admin logins.
- Record actual post-login routes and visible modules.
- Identify whether all client roles share one application with role-based navigation.
- Record requests, responses, state transitions, background processing, uploads, downloads, and cross-role synchronization.
- Confirm which permissions are enforced by the API.
- Identify implemented Version 1 and Version 2 functionality.
- Record product drift without silently changing the PRD.
- Request API documentation when contracts cannot be determined safely.
- Never invent endpoints or payloads.

#### 3. Repetition-First Automation Strategy

Prioritize a flow when it is:

- Repeated after deployments or bug fixes
- Deterministic
- Business-relevant
- Available through a supplied role
- Safe with synthetic data
- Observable through the UI, API, download, or cross-role state
- Stable enough to maintain

Automate first:

1. Login, logout, session, and direct-route denial
2. Farmer produce record creation and persistence
3. Market search, filtering, and sorting
4. Warehouse onboarding and profile updates
5. Capacity and occupancy updates
6. Logistics Provider profile and vehicle registry
7. Shipment status and delivery completion
8. Admin warehouse review
9. Admin lists, filters, dashboard totals, and reports
10. Repeatable bug fixes

Do not automate unavailable personas merely because the PRD describes them.

#### 4. Manual and Exploratory Allocation

Keep manual or hybrid coverage for:

- New or unstable features
- Ambiguous requirements
- Visual and usability quality
- Subjective accreditation or grading decisions
- Real financial settlement
- KYC judgment
- Complex market-price correctness
- External provider behaviour without a sandbox
- Security abuse exploration
- Cross-role scenarios requiring unavailable accounts
- High-risk novel combinations

#### 5. Automation Framework Standards

- Use Cypress or Playwright according to the repository standard.
- Use JavaScript or TypeScript according to project convention.
- Build reusable login commands for Farmer, Logistics, Warehouse Owner, and Admin.
- Use API-assisted setup where the contract is known and safe.
- Use stable `data-*` selectors.
- Avoid deep DOM, generated classes, list position, and fixed waits.
- Synchronize against visible state, aliased requests, or documented processing completion.
- Generate run-specific synthetic data.
- Prevent duplicate test records in parallel execution.
- Make tests independently executable and safe to rerun.
- Clean up synthetic records where supported.
- Preserve failure evidence while masking secrets.
- Validate critical state through UI and API where possible.
- Clearly label mocked and real integration tests.

#### 6. Core Module Priorities

**P0 — QA Smoke**

- Farmer login
- Logistics login
- Warehouse Owner login
- Admin login
- Session and logout
- Core navigation
- Direct-route denial
- Primary module availability

**P0 — Active Critical Regression**

- Farmer produce submission
- Produce persistence
- Market search and filtering
- Warehouse onboarding
- Warehouse profile persistence
- Capacity and occupancy validation
- Vehicle registration
- Duplicate vehicle prevention
- Shipment status transitions
- Delivery completion
- Admin warehouse review
- Cross-role data consistency

**P1 — Active Repetitive Regression**

- Search
- Filter
- Sort
- Pagination
- Edit
- Cancel
- Empty states
- Dashboard totals
- Report generation
- Audit visibility
- Bug-fix regression

**P1 — RBAC**

- Farmer restrictions
- Logistics restrictions
- Warehouse Owner restrictions
- Admin privileges
- Direct URL denial
- API denial

**P2 — Pending**

- Buyer purchase
- Cooperative operations
- Finance approval
- Payments
- Escrow
- Reconciliation
- Loans
- VAT
- Government verification
- Exchange feeds
- WMS sync
- GPS tracking

Pending suites must not be implemented as active end-to-end automation without access.

#### 7. Negative and Edge-Case Coverage

Automate stable cases including:

- Invalid credentials
- Expired session
- Post-logout route access
- Unauthorized direct route
- Missing required Farmer fields
- Duplicate produce submission
- Invalid warehouse capacity
- Occupancy above capacity
- Missing warehouse fields
- Duplicate warehouse submission
- Duplicate vehicle plate
- Invalid vehicle capacity
- Missing shipment data
- Invalid shipment transition
- Duplicate delivery confirmation
- Failed save
- Stale page data
- Empty lists
- Search with no result
- Failed report generation
- Concurrent updates where deterministic

#### 8. API and Integration Verification

- Validate request initiation, response handling, persistence, retry, idempotency, and UI synchronization.
- Verify cross-role warehouse and shipment data consistency.
- Verify repeated submissions do not create duplicate effective records.
- Verify dashboard aggregates reconcile with source records.
- Do not infer financial, exchange, government, WMS, GPS, or payment contracts.
- Keep those integrations Pending until supplied.
- Use mocks only for isolated failure coverage and identify them as mocked.

#### 9. Evidence and Reporting

For every failure:

- Record requirement ID, test ID, role, portal, environment, and synthetic record ID.
- Capture meaningful screenshots, traces, videos, and masked request evidence.
- Capture expected and actual state.
- Classify the failure as:
  - Product defect
  - Automation defect
  - Environment defect
  - Test-data defect
  - Integration defect
  - PRD ambiguity
  - Version drift
  - Pending dependency
- Link fixed repeatable defects to permanent regression tests.

#### 10. PRD and Version Drift Reporting

Compare observed behaviour with both PRDs and report:

- Original requirements absent from Version 2
- Version 2 features absent from the environment
- Implemented features absent from both documents
- Changed labels
- Changed roles
- Changed fields
- Changed state transitions
- Changed validation
- UI/API permission differences
- Mixed legacy and new workflows
- Features described as future but already present
- Pending features exposed without usable test access

Do not silently assume that Version 2 removed a baseline requirement.

#### 11. CI/CD Readiness

Define:

- `qa-smoke`
- `qa-farmer-regression`
- `qa-warehouse-regression`
- `qa-logistics-regression`
- `qa-admin-regression`
- `qa-rbac`
- `qa-bug-regression`
- `qa-cross-role-sync`
- `qa-api-integration`
- `qa-extended`
- `production-smoke-readonly`
- `production-sanity-readonly`

Do not create active suites for Pending roles or integrations.

Execution rules:

- Run smoke after deployments.
- Run affected role regression after feature changes.
- Run bug regression before promotion.
- Run cross-role and API integration in the supplied test environment.
- Prevent write suites from targeting Production.
- Keep smoke fast and independent.
- Quarantine only with a linked defect and review date.

### Required Agent Deliverables

Maintain:

- **Combined V1/V2 Requirement Matrix**
- **Active-vs-Pending Coverage Register**
- **Automation Suitability and Priority Matrix**
- **QA Repetitive-Flow Backlog**
- **Manual Exploratory Charter Register**
- **Environment and Role Audit**
- **Route and Network/API Inventory**
- **Synthetic Test Data Schema**
- **RBAC Verification Matrix**
- **Automated Smoke and Role Regression Suites**
- **Cross-Role Synchronization Suite**
- **Bug Regression Register**
- **Pending Roles and Integrations Register**
- **Defect and Version Drift Register**
- **Execution Report**
- **Known Limitations Register**
- **Production Monitoring and Rollback Checklist**
- **Production-Safety Register**

### Starter Commands

1. **“Audit the Farmer, Logistics, Warehouse Owner, and Admin logins and report the actual landing route and visible modules for each.”**
2. **“Create the active repetitive-flow automation backlog using only the four supplied roles and mark every unsupported PRD flow Pending.”**
3. **“Build a fast PLACOM smoke suite covering all supplied logins, session handling, logout, core navigation, and direct-route denial.”**
4. **“Compare the implemented Farmer, Warehouse, Logistics, and Admin features with the Stakeholder PRD and Version 2 PRD.”**
5. **“Inspect the Farmer produce-submission workflow and identify stable UI, API, persistence, and status checkpoints.”**
6. **“Implement the Warehouse Owner onboarding and capacity-update regression flow with Admin-side reconciliation.”**
7. **“Implement the Logistics Provider vehicle-registry flow, including duplicate plate prevention and Admin visibility.”**
8. **“Inspect shipment assignment, progress, and delivery confirmation and automate only the accessible deterministic states.”**
9. **“Generate the combined V1/V2 coverage matrix with Active, Pending, Future, Blocked, and Not Implemented statuses.”**
10. **“Design synthetic Farmers, produce records, warehouses, vehicles, routes, shipments, and market data for parallel-safe execution.”**
11. **“Audit UI and API role boundaries for Farmer, Logistics, Warehouse Owner, and Admin.”**
12. **“Validate Admin dashboard totals against Farmer, warehouse, vehicle, and shipment source records.”**
13. **“Review resolved PLACOM defects and convert each repetitive deterministic fix into a permanent regression test.”**
14. **“Create manual exploratory charters for payment, escrow, KYC, cooperative, Buyer, tax, exchange, WMS, GPS, and other Pending areas.”**
15. **“Produce a route and network inventory for the client and Admin applications without exposing credentials.”**
16. **“Compare the supplied test environment with Version 2 and produce a version-drift report.”**
17. **“Build an authorized Production read-only suite limited to availability, login-page, route-health, and dependency-health checks.”**
18. **“Add hard environment guards preventing Farmer submissions, warehouse updates, vehicle creation, shipment changes, Admin approvals, stress, and destructive suites from targeting Production.”**

---

## 9. Product Clarifications Required Before Full Automation

1. **Product Manager:** Are the supplied PLACOM URLs QA, staging, pilot, or Production?
2. **Product Manager:** Is Version 2 the approved current requirements source, with the Stakeholder PRD retained as the baseline?
3. **Product Manager:** Which Stakeholder PRD requirements were intentionally removed, changed, or deferred in Version 2?
4. **Engineering Lead:** Do Farmer, Logistics, and Warehouse Owner share one client application with role-based routing?
5. **Product Manager:** What exact modules should each supplied role currently access?
6. **Engineering Lead:** What is the authentication and session model for each portal?
7. **Engineering Lead:** What are the password reset, lockout, session expiry, refresh, and logout rules?
8. **Product Manager:** What are the required fields and validation rules for Farmer produce submission?
9. **Product Manager:** What are the authoritative produce-submission statuses?
10. **Product Manager:** What is the last Farmer workflow state that can be tested without a Buyer or Finance role?
11. **Product Manager:** Can a Farmer edit or withdraw a submitted produce record?
12. **Product Manager:** What fields and validations apply to warehouse onboarding?
13. **Product Manager:** What are the authoritative warehouse onboarding and accreditation statuses?
14. **Product Manager:** What scoring formula determines warehouse accreditation?
15. **Product Manager:** Who may approve or reject a warehouse?
16. **Product Manager:** What happens after warehouse rejection?
17. **Product Manager:** What are the capacity and occupancy units and validation rules?
18. **Product Manager:** Can occupancy exceed capacity under any condition?
19. **Product Manager:** What formula defines available capacity?
20. **Product Manager:** Can one warehouse store multiple commodities with separate capacities?
21. **Product Manager:** What warehouse-receipt workflow is currently implemented?
22. **Product Manager:** Can the supplied Warehouse Owner issue receipts without another role?
23. **Engineering Lead:** Is there a document or receipt-generation service?
24. **Product Manager:** What fields are required for a Logistics Provider profile?
25. **Product Manager:** What vehicle plate normalization and uniqueness rules apply?
26. **Product Manager:** What vehicle types, capacities, compatibility values, and availability states are valid?
27. **Product Manager:** What shipment statuses and permitted transitions are implemented?
28. **Product Manager:** Who creates the order or shipment that the Logistics Provider receives?
29. **Product Manager:** Can Admin create synthetic shipments for QA?
30. **Product Manager:** What prevents duplicate assignment or delivery confirmation?
31. **Product Manager:** Is logistics marketplace matching currently implemented?
32. **Product Manager:** Is real-time tracking implemented internally or through an external provider?
33. **Product Manager:** Which Admin dashboard totals and reports are currently implemented?
34. **Product Manager:** What formulas and refresh cadence govern dashboard totals?
35. **Product Manager:** What filters, sorting, and pagination rules apply to Admin lists?
36. **Product Manager:** What audit events are visible to Admin?
37. **Engineering Lead:** Are role permissions enforced at both UI and API levels?
38. **Engineering Lead:** What API documentation is available?
39. **Engineering Lead:** Are stable `data-*` test attributes available?
40. **Engineering Lead:** What test-data reset or cleanup mechanism is available?
41. **QA Lead:** Which repetitive checks must run after every deployment?
42. **QA Lead:** Which role regression suites must run before release promotion?
43. **QA Lead:** What defect-management system and evidence fields should automation use?
44. **Product Manager:** When will Buyer/Processor test access be supplied?
45. **Product Manager:** When will Cooperative test access be supplied?
46. **Product Manager:** When will Finance/Approver test access be supplied?
47. **Engineering Lead:** Is there a safe payment and escrow sandbox?
48. **Engineering Lead:** What bank callback, settlement, and reconciliation contracts apply?
49. **Product Manager:** What storage, handling, transaction, and commission fee formulas apply?
50. **Product Manager:** What VAT rate, exemption, and rounding rules apply?
51. **Engineering Lead:** What government Farmer database integration is implemented?
52. **Engineering Lead:** What NCX and LCFE integration contracts are available?
53. **Engineering Lead:** What external WMS integrations are implemented?
54. **Engineering Lead:** What logistics or GPS provider sandbox is available?
55. **Product Manager:** Is route optimization excluded from the current release?
56. **Product Manager:** What report and export formats are available to each supplied role?
57. **Engineering Lead:** What file-generation, retention, and secure-download rules apply?
58. **Product Manager:** What browsers, mobile devices, viewport sizes, and accessibility requirements apply?
59. **Product Manager:** What response-time, availability, search, save, report, and synchronization thresholds define acceptance?
60. **Operations Lead:** Which Production logs, monitoring dashboards, alerts, and customer-feedback systems are available to QA?
61. **Operations Lead:** Which exact Production smoke and sanity checks are authorized?
62. **Release Manager:** What rollback scenarios must QA support?
63. **Security Owner:** What technical guard must prevent write, stress, payment, settlement, or destructive suites from targeting Production?
64. **Security Owner:** Should the supplied credentials be rotated and stored in an approved secret manager before automation implementation?
