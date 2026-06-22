# IMO Ohuru Diopalm Digital Platform — Functional and Agentic QA Specification

- **Source:** Ohuru Diopalm Digital Platform PRD Version 1 and PRD Version 2.0
- **Product Type:** Digital cooperative management, farmer participation, tree-planting, revenue-monitoring, and agricultural impact platform
- **Specification Status:** Draft — derived from the two supplied PRDs and the supplied QA portal access. No authenticated role, API, email, SMS, USSD, payment, QR, geolocation, administrative, or production verification has been performed.
- **Version Treatment:** Version 2.0 is treated as the current product direction. Version 1 remains the baseline for requirements not expressly replaced or contradicted by Version 2.0.
- **Active Access Scope:** The supplied access method is **self-registration of new accounts** through the QA client portal. No reusable credentials or confirmed Super Admin, Cooperative Administrator, Farmer, Field Agent, Stakeholder, Buyer/Processor, or payment-role accounts are supplied.
- **Active Automation Scope:** Automation is limited to repetitive, deterministic behaviour that can be exercised through public access and newly registered synthetic accounts: portal availability, account registration, registration validation, duplicate-account prevention, successful authentication where registration creates a login-capable account, session handling, logout, and visible account-state persistence.
- **Pending Scope:** Cooperative approval, cooperative administration, Farmer management, Field Agent activity, QR scanning, tree allocation and verification, USSD onboarding, SMS notifications, revenue monitoring, profit calculation, payment distribution, Admin management, settings, analytics, audit reports, role/permission administration, and all external integrations remain **Pending** until the relevant routes, roles, credentials, contracts, and test facilities are available.
- **Environment Execution Policy:** QA is the primary environment for automated repetitive checks, bug-fix regression, negative validation, and controlled API inspection. Manual effort should focus on exploratory behaviour, usability, rural-connectivity conditions, device-specific functions, subjective impact reporting, and high-risk workflows that are not yet accessible or deterministic. Production must be restricted to authorized, non-destructive smoke, sanity, monitoring, and rollback-support checks.

---

## 1. Platform Access and Roles

### QA Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| IMO Ohuru Client QA Portal | Public visitor / self-registering user | `https://imo-platform-client-qa-197301616810.us-central1.run.app/` | Register a new synthetic account | Create a synthetic password during registration | `OHURU_QA_CLIENT_URL`, `OHURU_QA_TEST_EMAIL`, `OHURU_QA_TEST_PHONE`, `OHURU_QA_TEST_PASSWORD` |
| Super Admin Portal | Pending | `<ENTER_SUPER_ADMIN_QA_URL>` | `<ENTER_SUPER_ADMIN_EMAIL>` | `<ENTER_SUPER_ADMIN_PASSWORD>` | `OHURU_QA_SUPER_ADMIN_URL`, `OHURU_QA_SUPER_ADMIN_EMAIL`, `OHURU_QA_SUPER_ADMIN_PASSWORD` |
| Cooperative Administrator Portal | Pending | `<ENTER_COOPERATIVE_ADMIN_QA_URL>` | `<ENTER_COOPERATIVE_ADMIN_EMAIL>` | `<ENTER_COOPERATIVE_ADMIN_PASSWORD>` | `OHURU_QA_COOP_ADMIN_URL`, `OHURU_QA_COOP_ADMIN_EMAIL`, `OHURU_QA_COOP_ADMIN_PASSWORD` |
| Farmer Account | Pending unless public registration produces this role | `<ENTER_FARMER_PORTAL_URL>` | `<GENERATE_FARMER_EMAIL_OR_PHONE>` | `<GENERATE_FARMER_PASSWORD>` | `OHURU_QA_FARMER_URL`, `OHURU_QA_FARMER_EMAIL`, `OHURU_QA_FARMER_PHONE`, `OHURU_QA_FARMER_PASSWORD` |
| Field Agent Portal / App | Pending | `<ENTER_FIELD_AGENT_QA_URL>` | `<ENTER_FIELD_AGENT_EMAIL>` | `<ENTER_FIELD_AGENT_PASSWORD>` | `OHURU_QA_FIELD_AGENT_URL`, `OHURU_QA_FIELD_AGENT_EMAIL`, `OHURU_QA_FIELD_AGENT_PASSWORD` |
| Stakeholder / Project Owner Portal | Pending | `<ENTER_STAKEHOLDER_QA_URL>` | `<ENTER_STAKEHOLDER_EMAIL>` | `<ENTER_STAKEHOLDER_PASSWORD>` | `OHURU_QA_STAKEHOLDER_URL`, `OHURU_QA_STAKEHOLDER_EMAIL`, `OHURU_QA_STAKEHOLDER_PASSWORD` |
| Buyer / Processor Portal | Pending | `<ENTER_BUYER_QA_URL>` | `<ENTER_BUYER_EMAIL>` | `<ENTER_BUYER_PASSWORD>` | `OHURU_QA_BUYER_URL`, `OHURU_QA_BUYER_EMAIL`, `OHURU_QA_BUYER_PASSWORD` |
| USSD Sandbox | Pending | `<ENTER_USSD_CODE_OR_SIMULATOR>` | `<ENTER_TEST_MSISDN>` | `<ENTER_USSD_PIN_IF_APPLICABLE>` | `OHURU_QA_USSD_CODE`, `OHURU_QA_USSD_TEST_MSISDN`, `OHURU_QA_USSD_PIN` |
| SMS Sandbox | Pending | `<ENTER_SMS_SANDBOX_URL>` | `<ENTER_SMS_TEST_ACCOUNT>` | `<ENTER_SMS_TEST_SECRET>` | `OHURU_QA_SMS_URL`, `OHURU_QA_SMS_ACCOUNT`, `OHURU_QA_SMS_SECRET` |
| Payment Sandbox | Pending | `<ENTER_PAYMENT_SANDBOX_URL>` | `<ENTER_PAYMENT_TEST_ACCOUNT>` | `<ENTER_PAYMENT_TEST_SECRET>` | `OHURU_QA_PAYMENT_URL`, `OHURU_QA_PAYMENT_ACCOUNT`, `OHURU_QA_PAYMENT_SECRET` |

**QA execution policy:** Generate synthetic accounts with unique run-specific identifiers. Automate only registration and post-registration behaviour that is actually exposed. Do not assume that registration creates a Farmer, Cooperative Administrator, Stakeholder, or other PRD role until the resulting account type and permissions are verified.

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Environment Variable Mappings |
|---|---|---|---|---|---|
| IMO Ohuru Production Client | Public / registered user | `<ENTER_PRODUCTION_CLIENT_URL>` | `<ENTER_PRODUCTION_TEST_USER>` | `<ENTER_PRODUCTION_TEST_PASSWORD>` | `OHURU_PROD_CLIENT_URL`, `OHURU_PROD_TEST_USER`, `OHURU_PROD_TEST_PASSWORD` |
| IMO Ohuru Production Admin | Authorized Admin | `<ENTER_PRODUCTION_ADMIN_URL>` | `<ENTER_PRODUCTION_ADMIN_EMAIL>` | `<ENTER_PRODUCTION_ADMIN_PASSWORD>` | `OHURU_PROD_ADMIN_URL`, `OHURU_PROD_ADMIN_EMAIL`, `OHURU_PROD_ADMIN_PASSWORD` |

Production automation must remain limited to explicitly authorized, non-destructive availability, login-page, route-health, and sanity checks. Do not create accounts, cooperatives, Farmers, planting records, revenue records, payments, messages, administrators, reports, or other live data in Production without written authorization and an isolated synthetic tenant.

---

## 2. Feature List

### A. Public Portal and Account Registration — Active

- **Portal Availability:** The QA client URL should load successfully.
- **Registration Entry Point:** A public user can access the account-registration flow.
- **Synthetic Account Creation:** QA may create new synthetic accounts because no reusable test credentials are supplied.
- **Required Field Validation:** Registration should prevent submission when mandatory fields are absent.
- **Format Validation:** Email, phone, password, and other exposed identity fields should enforce the implemented formats.
- **Password Rules:** The registration form should enforce the implemented minimum length, complexity, confirmation, and prohibited-value rules.
- **Duplicate Identity Prevention:** The same normalized email, phone number, username, NIN, or other unique identifier must not silently create multiple active accounts where such fields are exposed.
- **Whitespace and Case Normalization:** Leading/trailing whitespace and email case should be handled consistently.
- **Successful Registration:** A valid unique account should be created once.
- **Confirmation State:** The user should receive the implemented success, verification, approval-pending, or next-step state.
- **Post-Registration Authentication:** Where registration creates a login-capable account, the new account should be able to authenticate.
- **Session Persistence:** A successfully authenticated synthetic account should remain signed in according to the implemented session policy.
- **Logout:** Logout should end the authenticated session.
- **Post-Logout Protection:** Protected routes should not remain accessible after logout.
- **Account Persistence:** The account should remain recognized after refresh and a later login.
- **Registration Defect Regression:** Every repeatable registration or authentication defect should become a permanent automated regression check after the fix is validated.

### B. Role Assignment and Approval — Pending

- **Registration Role:** It is not yet confirmed which role public registration creates.
- **Role Selection:** It is not confirmed whether users can select Farmer, Cooperative Administrator, Field Agent, Buyer, or another persona during registration.
- **Administrative Approval:** Version 2 defines cooperative approval, but it is not confirmed whether individual accounts also require approval.
- **Role Permissions:** No authenticated role accounts are supplied for verification.
- **Role Management:** Super Admin creation, assignment, permission management, and deactivation remain Pending.
- **Cooperative Administrator Scope:** Cooperative-specific data isolation remains Pending.
- **Direct-Route and API RBAC:** Active only after at least two distinct roles or an administrative account become available.

### C. Cooperative Management — Pending

Version 2 defines:

- Add Cooperative
- View Cooperative Details
- Edit Cooperative Information
- Monitor Cooperative Performance
- Track Cooperative Membership
- Review Applications
- Approve Cooperatives
- Reject Applications
- View Approval Status
- Maintain Approval History

Version 1 additionally defines:

- Central cooperative registry
- Cooperative location, leader, and membership records
- Member linkage
- Cooperative-specific access control
- Minor Farmer-record corrections
- Contribution and earnings views
- Cooperative autonomy with system-wide integrity

All cooperative-management automation remains Pending because no cooperative or Super Admin access is supplied.

### D. Farmer Management — Pending Except Account Registration

Version 2 defines:

- Farmer registration
- Farmer profile management
- Cooperative assignment
- Search and filtering
- Farmer reporting

Version 1 additionally defines:

- USSD onboarding
- NIN and VIN capture
- Auto-captured phone number
- Cooperative selection
- Central-database synchronization
- SMS confirmation
- QR-linked Farmer identity
- Contribution records
- Farmer earnings and profit-share visibility

Active automation is limited to public account registration. It must not be represented as full Farmer onboarding unless the resulting account and fields match the Farmer workflow.

### E. Tree Planting and Field Operations — Pending

- Tree allocation tracking
- Planting progress
- Cooperative contribution to planting
- Community-level performance
- Tree-planting analytics
- QR-coded seedling or Farmer records
- Geotagged photo capture
- Field Agent scanning
- Real-time or deferred synchronization
- Timestamped field entries
- Agent activity logs
- Planting verification

No Field Agent access, mobile app, QR test data, camera workflow, GPS fixture, or offline-sync contract is supplied.

### F. Revenue, Profit, and Payment — Pending

Version 2 defines:

- Internal mill revenue
- External mill revenue
- Revenue analytics
- Revenue trends

Version 1 additionally defines:

- Monthly palm-oil sales
- Farmer contribution-based profit calculations
- Configurable percentage splits
- Example 70% Farmer, 20% cooperative, and 10% project-overhead distribution
- Planting bonus
- Malaria-wallet credit
- Paystack or Paga disbursement
- Pending payouts
- Payment history
- SMS payment breakdown
- Cooperative balance

No payment sandbox, wallet, revenue account, Finance role, contribution data, formulas, or approval workflow is supplied. All financial automation remains Pending.

### G. SMS and USSD — Pending

Version 1 defines:

- USSD registration
- Registration confirmation SMS
- Contribution update SMS
- Payment alert SMS
- Bulk training and meeting messages
- Optional localized-language support

No USSD code, aggregator simulator, SMS sandbox, test MSISDN, message templates, delivery reports, or credentials are supplied.

### H. Dashboard and Project Oversight — Pending

Version 2 defines dashboard metrics:

- Total Farmers
- Total Trees Planted
- Total Cooperatives
- Internal Mill Revenue
- External Mill Revenue

Version 1 additionally defines:

- Total contributions
- Total payouts
- Participation rates
- Produce collected
- Profit-distribution ratios
- Farmer activity
- Cooperative performance
- Seasonal and monthly trends

No Super Admin or Stakeholder account is supplied.

### I. Admin and Settings — Pending

Version 2 defines:

- Create Administrators
- Assign Roles
- Manage Permissions
- Deactivate Users
- User Preferences
- Platform Configuration
- Security Controls
- Notification Preferences

No Super Admin account or Admin route is supplied.

### J. Analytics, Reports, and Audit — Pending

Version 2 defines:

- Cooperative analytics
- Farmer analytics
- Tree-planting analytics
- Revenue analytics
- Exportable reports

Version 1 additionally defines:

- Immutable activity logs
- User activity history
- Data-origin traceability
- Tamper prevention
- Audit reports
- Basic impact summaries

No authenticated oversight account or audit-report access is supplied.

---

## 3. User Stories

### Active User Stories

> **As a public visitor,** I want to open the Ohuru QA portal, so that I can begin account registration.

> **As a new user,** I want the registration form to identify missing or invalid values, so that I can correct my information before account creation.

> **As a new user,** I want to register once with a unique identity, so that duplicate accounts are not created.

> **As a newly registered user,** I want to receive a clear success, verification, or approval-pending state, so that I understand the next step.

> **As a newly registered user,** I want to authenticate with my created credentials where login is enabled, so that I can access my permitted account area.

> **As an authenticated user,** I want logout to terminate my session, so that my account remains protected on shared devices.

### Pending User Stories

> **As a Super Admin,** I want to approve cooperatives and manage administrators. **Status: Pending.**

> **As a Cooperative Administrator,** I want to manage Farmers and cooperative performance. **Status: Pending.**

> **As a Farmer,** I want to receive seedlings, participate in planting programs, and view opportunities. **Status: Pending beyond public registration.**

> **As a Field Agent,** I want to scan QR codes, capture geotagged evidence, and synchronize planting or contribution records. **Status: Pending.**

> **As a Stakeholder,** I want to monitor Farmers, cooperatives, trees, revenue, and project impact. **Status: Pending.**

> **As a Cooperative Administrator,** I want the platform to calculate and distribute Farmer profit shares. **Status: Pending.**

> **As a Farmer,** I want to register through USSD and receive SMS confirmations. **Status: Pending.**

---

## 4. Core User Flows in Gherkin Syntax

The following scenarios define the active automation backlog. They are intentionally restricted to repetitive registration and account-access behaviour available through the supplied QA portal. PRD business modules remain Pending until access is provided.

### Flow 1: QA Portal Availability and Registration Access

```gherkin
Feature: Public access to IMO Ohuru QA

  Scenario: A public visitor opens the QA portal
    Given the IMO Ohuru QA client URL is configured
    When the visitor opens the portal
    Then the application loads without a fatal error
    And the account-registration entry point is available
    And no authentication secret is required to open the registration flow
```

### Flow 2: Required Registration Fields

```gherkin
Feature: Registration form validation

  Scenario: A user submits an empty registration form
    Given the user is on the registration page
    When the user submits without entering required information
    Then the account is not created
    And each missing required field displays an actionable validation message
    And no success or authenticated state is displayed
```

### Flow 3: Invalid Registration Data

```gherkin
Feature: Registration input validation

  Scenario Outline: A user enters an invalid identity value
    Given the user is on the registration page
    And all other required fields contain valid synthetic data
    When the user enters <invalid_value> in the <field>
    And submits the form
    Then the account is not created
    And the <field> displays the implemented validation response

    Examples:
      | field            | invalid_value                       |
      | email            | invalid-email                       |
      | phone number     | alphabetic-phone                    |
      | password         | value below the implemented policy  |
      | password confirm | value different from the password   |
```

### Flow 4: Successful Synthetic Account Registration

```gherkin
Feature: New account creation

  Scenario: A user registers with unique valid synthetic details
    Given the user is on the registration page
    And a run-specific email or phone number has been generated
    And all required fields contain valid values
    When the user submits the registration
    Then one account-creation request is accepted
    And the user receives the implemented success, verification, or approval-pending state
    And the submitted identity is not exposed unnecessarily in logs or reports
```

### Flow 5: Duplicate Account Prevention

```gherkin
Feature: Duplicate registration control

  Scenario: A previously registered identity is submitted again
    Given a synthetic account already exists
    When a user attempts to register again using the same normalized unique identity
    Then a second active account is not silently created
    And the user receives the configured duplicate-account response
    And the original account remains valid
```

### Flow 6: Login with a Newly Registered Account

```gherkin
Feature: Post-registration authentication

  Scenario: A verified or immediately active account signs in
    Given a synthetic account was registered successfully
    And the account has reached the login-eligible state
    When the user submits the registered credentials
    Then an authenticated session is established
    And the user is directed to the implemented account landing page
    And the authenticated identity remains recognized after page refresh

  Scenario: A pending or unverified account attempts to sign in
    Given a synthetic account exists but is not yet login-eligible
    When the user submits its credentials
    Then the application applies the configured pending or verification rule
    And no unauthorized account area is displayed
```

### Flow 7: Logout and Protected Route Handling

```gherkin
Feature: Session termination

  Scenario: An authenticated synthetic user logs out
    Given the user has an active session
    When the user selects logout
    Then the session is terminated
    And the user is returned to a public or login page
    When the user revisits a previously protected route
    Then authentication is required
    And cached protected data is not displayed
```

### Flow 8: Registration Retry and Double Submission

```gherkin
Feature: Idempotent account creation

  Scenario: The registration button is submitted repeatedly
    Given valid unique synthetic registration data is entered
    When the user submits the same form more than once before the response completes
    Then only one effective account is created
    And the UI does not display contradictory success and error states
    And the user can proceed using the single resulting account
```

---

## 5. Expected Outcomes and Success Measures

### Active Functional Outcomes

- The QA client portal is available.
- A public user can access registration.
- Required-field validation prevents incomplete account creation.
- Invalid formats are rejected according to implemented rules.
- Valid unique synthetic details create one account.
- Duplicate identities do not silently create multiple active accounts.
- Repeated submission does not create duplicate effective accounts.
- The user receives a clear post-registration state.
- Login succeeds for registration-created accounts that are active or verified.
- Pending or unverified accounts follow the implemented restriction.
- Logout terminates the session.
- Protected routes require authentication after logout.
- Registered-account state persists across refresh and later sign-in.
- Repeatable registration defects become permanent regression tests.

### Pending Functional Outcomes

The following remain Pending:

- Cooperative creation and approval
- Cooperative membership management
- Farmer operational profiles and reporting
- Tree allocation, planting, and verification
- QR and geolocation workflows
- Field Agent activity
- Internal and external mill revenue
- Profit calculations and payout distribution
- Payment-gateway processing
- USSD onboarding
- SMS delivery
- Super Admin oversight
- Admin and permission management
- Settings
- Analytics
- Exports
- Audit reports
- FarmsAgora market linkage
- Buyer/Processor access

### PRD-Defined Programme Outcomes

The PRDs describe strategic outcomes such as:

- Greater project transparency
- Stronger cooperative governance
- Improved Farmer visibility
- Measurable tree-planting impact
- Revenue visibility
- Data-driven decisions
- Farmer empowerment
- Community economic growth
- Support for up to 1,000 Farmers in the original initiative vision

These are programme and reporting outcomes, not direct automated pass criteria until their calculation formulas, source data, time windows, and acceptance thresholds are supplied.

### Security Outcomes

- Credentials and personal identity values are not written to public logs.
- Synthetic data is used for account registration.
- Registration does not expose another user’s account details.
- Post-logout access is denied.
- Pending roles and modules are not tested through unauthorized route guessing.
- Production account creation is prohibited without authorization.

### Measures Not Yet Defined

The PRDs do not define:

- Exact registration fields
- Required versus optional account fields
- Account types available at registration
- Email or phone verification rules
- Password policy
- Duplicate-key rules
- Approval requirements
- Session duration
- Lockout
- Password reset
- Account deletion
- Response-time thresholds
- Accessibility standard
- Browser and device support
- Rate limits
- CAPTCHA
- API contracts
- Audit retention
- Production monitoring thresholds

---

## 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
|---|---|---|
| Public Portal | Load registration and public content | Client web application and hosting platform |
| Account Registration | Validate and create a user | Identity/user-management API and user database |
| Duplicate Prevention | Enforce unique user identity | Database uniqueness, identity-service lookup, or registration API |
| Email Verification | Confirm ownership where implemented | Pending confirmation; email provider and token service |
| Phone Verification | Confirm ownership where implemented | Pending confirmation; OTP/SMS provider |
| Authentication | Login and create session | Identity/session API |
| Session Persistence | Retain authenticated state | Secure cookie or token storage; exact mechanism requires inspection |
| Logout | Revoke or clear session | Client session action and authentication API |
| Password Reset | Recover account access | Pending confirmation; email/SMS token flow |
| Role Assignment | Apply account role | Pending; identity/authorization service |
| Cooperative Approval | Review and activate cooperative | Pending; workflow and Admin API |
| Farmer Management | Store Farmer and cooperative data | Pending; Farmer-management API |
| Field Activity | QR, GPS, photo, timestamp, sync | Pending; mobile app, device APIs, storage, and sync service |
| Tree Planting | Allocate, record, and verify trees | Pending; planting-management API |
| Revenue | Record mill income and trends | Pending; revenue/financial data service |
| Profit Distribution | Calculate shares and disburse | Pending; calculation engine, ledger, and payment gateway |
| SMS | Send confirmations and alerts | Pending; SMS provider |
| USSD | Register Farmers without internet | Pending; USSD gateway and session API |
| Dashboard | Aggregate cooperative, Farmer, tree, and revenue metrics | Pending; reporting or analytics API |
| Audit Trail | Record sensitive operations | Pending; append-only event/audit service |
| Reports | Generate project reports and exports | Pending; reporting and file-generation service |

---

## 7. Integration-Based Gherkin Flows

Only registration-related integration behaviour is active. Provider-dependent flows remain Pending.

### Integration Flow 1: Registration API Persistence

```gherkin
Feature: Account registration persistence

  Scenario: A valid registration is accepted and remains retrievable
    Given unique valid synthetic registration data is available
    When the registration request is accepted
    Then one user record is created
    And a later login or account lookup recognizes the same identity
    And page refresh does not revert the registration outcome
```

### Integration Flow 2: Registration Idempotency

```gherkin
Feature: Registration retry protection

  Scenario: The same registration request is retried
    Given a registration request has already been accepted
    When the same effective request is submitted again
    Then the service does not create a second active account
    And it returns the configured duplicate or existing-account response
```

### Integration Flow 3: Email or Phone Verification

```gherkin
Feature: Account verification

  Scenario: A verification challenge activates a registered account
    Given a synthetic account requires email or phone verification
    And an approved QA inbox or OTP mechanism is available
    When the correct verification value is submitted
    Then the account becomes login-eligible
    And the verification value cannot be reused contrary to the implemented policy

  # Status: Active only if the current registration flow exposes verification and QA can retrieve the challenge safely.
```

### Integration Flow 4: Session Revocation

```gherkin
Feature: Authentication-session control

  Scenario: Logout invalidates protected access
    Given an authenticated session exists
    When logout completes
    Then subsequent protected requests using the ended session are rejected
    And a new authentication action is required
```

### Pending Integration Flow 5: USSD Registration

```gherkin
Feature: USSD Farmer onboarding

  Scenario: A Farmer completes registration through USSD
    Given an approved USSD simulator and test phone number are available
    When the Farmer submits the required registration values
    Then the central Farmer record is created
    And it is linked to the selected cooperative
    And an SMS confirmation is requested

  # Status: Pending — no USSD sandbox, code, or test MSISDN is supplied.
```

### Pending Integration Flow 6: Field Agent QR and Geolocation

```gherkin
Feature: Field activity synchronization

  Scenario: A Field Agent records a verified planting event
    Given an approved Field Agent account and device test environment are available
    When the Agent scans the Farmer or seedling QR code
    And captures the required geotagged evidence
    Then the planting record is timestamped
    And synchronized to cooperative and oversight dashboards

  # Status: Pending — no Field Agent access, QR fixture, device application, GPS, or sync contract is supplied.
```

### Pending Integration Flow 7: Profit Distribution

```gherkin
Feature: Automated Farmer profit distribution

  Scenario: Cooperative revenue is divided and disbursed
    Given an approved revenue record and contribution data exist
    And a payment sandbox is available
    When the distribution process applies the configured percentages and bonuses
    Then each Farmer share is calculated
    And approved payments are sent once
    And the cooperative balance and payout history reconcile

  # Status: Pending — no formulas, Finance role, contribution data, or payment sandbox is supplied.
```

### Pending Integration Flow 8: SMS Notifications

```gherkin
Feature: Farmer communication

  Scenario Outline: A platform event triggers a Farmer SMS
    Given an approved SMS sandbox and template are available
    When a <platform_event> is committed
    Then one SMS request is created for the correct Farmer
    And duplicate retries do not send duplicate effective notifications

    Examples:
      | platform_event           |
      | registration completed   |
      | contribution recorded    |
      | payment completed        |

  # Status: Pending — no SMS sandbox, templates, delivery contract, or test phone number is supplied.
```

---

## 8. System Instruction for the IMO Ohuru QA Automation Agent

### Role

You are the **Senior QA Automation Engineer for the IMO Ohuru Diopalm Digital Platform**.

Your active automation scope is limited to behaviour available through:

- The public QA client portal
- Self-registration of new synthetic accounts
- Post-registration authentication and session behaviour where exposed

Do not represent cooperative, Farmer operational, Field Agent, Super Admin, stakeholder, revenue, payment, USSD, SMS, tree-planting, analytics, reporting, settings, or audit functionality as active automated coverage without the required access and contracts.

Your primary objective is to automate **repetitive, deterministic account-registration and access checks** so that manual QA can concentrate on exploratory testing and high-risk workflows.

### Product and Version Position

- Treat Version 2.0 as the current product direction.
- Retain Version 1 as the baseline for features not removed or contradicted.
- Record a requirement as Pending when access, implementation, expected behaviour, or integration support is unavailable.
- Do not invent a role or workflow because it appears in a PRD.

### Environment and Secret Management

- Load the QA URL and generated credentials from environment variables or secure runtime fixtures.
- Generate unique synthetic email addresses, phone numbers, names, and passwords per run.
- Never use real Farmer, NIN, VIN, cooperative, payment, or personal information.
- Never hard-code secrets.
- Mask generated credentials in reports where they are no longer required for diagnosis.
- Keep QA and Production configuration isolated.
- Require hard environment guards before any registration suite can target Production.
- Clean up synthetic accounts where an approved cleanup mechanism exists.
- Do not guess administrative endpoints to delete accounts.

### Automation Test Basis

Use:

1. Public portal behaviour
2. Registration form rules observed in QA
3. PRD identity requirements where the field is actually exposed
4. Account-state transitions
5. Duplicate and retry behaviour
6. Authentication and session behaviour
7. Confirmed registration defects
8. API contracts discovered safely through the active flow

### Repetition-First Automation Rule

Automate a check when it is:

- Repeated after deployment or bug fixes
- Deterministic
- Accessible through public registration or a newly registered synthetic account
- Safe to execute repeatedly
- Objectively observable
- Valuable enough to maintain

Automate first:

1. Portal availability
2. Registration-page access
3. Required-field validation
4. Invalid-format validation
5. Password and confirmation validation
6. Successful unique registration
7. Duplicate registration prevention
8. Double-submit protection
9. Verification where accessible
10. Login after registration
11. Session persistence
12. Logout
13. Post-logout access denial
14. Repeatable bug regression

### Pending Rule

Mark a requirement Pending when:

- No role credential is supplied
- No route is supplied or safely discoverable
- The feature is not implemented
- The expected behaviour is unclear
- An external provider or sandbox is unavailable
- The workflow would affect real finances or personal data
- The workflow requires device hardware or field conditions not available to the automation environment

Pending is a valid coverage status. Do not fabricate a test to make the matrix appear complete.

### Manual and Exploratory Allocation

Use manual QA for:

- Registration usability and comprehension
- Mobile layout and accessibility
- Rural-user comprehension
- Unusual names and identity values
- Network interruption during registration
- Security abuse exploration
- CAPTCHA behaviour
- Email/SMS deliverability without a sandbox
- Role ambiguity
- New post-registration modules
- Any high-risk workflow that is not yet deterministic

### Automation Framework Standards

- Use Cypress or Playwright according to the project standard.
- Use JavaScript or TypeScript according to the repository convention.
- Create reusable commands for:
  - Opening registration
  - Generating synthetic identity data
  - Completing the form
  - Capturing account state
  - Logging in
  - Logging out
- Use stable `data-*` selectors.
- Avoid fixed waits.
- Synchronize against visible states and aliased requests.
- Make tests parallel-safe through unique identities.
- Make registration tests retry-safe without creating uncontrolled duplicates.
- Validate UI outcomes and API responses where possible.
- Capture meaningful evidence with identity values masked.
- Do not rely on provider messages unless a QA inbox or sandbox is available.

### Core Module Priorities

**P0 — QA Smoke**

- Portal availability
- Registration entry point
- Registration form renders
- Login route renders where available

**P0 — Registration Regression**

- Required fields
- Invalid formats
- Password policy
- Password confirmation
- Unique registration
- Duplicate prevention
- Double submission
- Post-registration state

**P1 — Authentication Regression**

- New-account login
- Pending-account restriction
- Invalid password
- Session persistence
- Logout
- Protected route after logout

**P1 — Bug Regression**

- Every resolved, repeatable registration or authentication defect

**Pending**

- Cooperative
- Farmer operational data
- Field Agent
- QR
- GPS
- Offline sync
- Tree planting
- Revenue
- Profit distribution
- Payment
- SMS
- USSD
- Admin
- Settings
- Analytics
- Reports
- Audit

### Negative and Edge-Case Coverage

Automate stable cases such as:

- Empty submission
- Invalid email
- Invalid phone
- Weak password
- Mismatched password confirmation
- Leading and trailing whitespace
- Email case normalization
- Duplicate identity
- Repeated click
- Back navigation after success
- Refresh during registration
- Network error with deterministic interception
- Server validation error
- Invalid login
- Logout
- Protected-route revisit

Keep CAPTCHA, real inbox delivery, unusual provider failure, and subjective error-message quality manual unless the environment makes them deterministic.

### API Verification

- Record the registration and authentication request sequence.
- Validate status handling without exposing tokens.
- Verify duplicate and retry protection.
- Verify one effective account per unique identity.
- Verify logout or token clearing.
- Do not infer Cooperative, Admin, payment, SMS, USSD, planting, or revenue endpoints.
- Record undocumented fields as observed implementation, not PRD fact.

### Evidence and Reporting

For each failure:

- Record requirement ID
- Automated-test ID
- Environment
- Generated account reference
- Expected state
- Actual state
- Masked request information
- Screenshot or trace where useful

Classify as:

- Product defect
- Automation defect
- Environment defect
- Test-data defect
- Registration-rule ambiguity
- Version drift
- Pending dependency

Link fixed repeatable defects to permanent regression tests.

### Version Drift Reporting

Compare observed registration behaviour against both PRDs and report:

- Fields in QA but absent from both PRDs
- V1 identity fields absent from QA
- Account roles that differ from the PRDs
- Verification or approval behaviour not documented
- Post-registration modules not described
- V2 features absent from the environment
- V1 features apparently removed or deferred
- Different labels, validations, or states

Do not assume that Version 2 removed a V1 feature unless confirmed.

### CI/CD and Suite Design

Define:

- `qa-public-smoke`
- `qa-registration-regression`
- `qa-authentication-regression`
- `qa-bug-regression`
- `qa-registration-api`
- `qa-registration-extended`
- `production-smoke-readonly`
- `production-sanity-readonly`

Execution rules:

- Run public smoke after deployment.
- Run registration regression before promotion.
- Run affected bug regression after fixes.
- Use unique account data for every execution.
- Prevent registration suites from targeting Production.
- Quarantine only with a linked defect and review date.
- Do not create active CI suites for Pending modules.

### Required Agent Deliverables

Maintain:

- **Combined V1/V2 Requirement Matrix**
- **Active-vs-Pending Coverage Register**
- **Registration Field and Validation Inventory**
- **Automation Suitability and Priority Matrix**
- **QA Repetitive-Flow Backlog**
- **Synthetic Account Data Strategy**
- **Route and Network/API Inventory**
- **Automated Registration and Authentication Suites**
- **Bug Regression Register**
- **Manual Exploratory Charter Register**
- **Pending Roles and Integrations Register**
- **Defect and Version Drift Register**
- **Execution Report**
- **Known Limitations Register**
- **Production Monitoring and Rollback Checklist**
- **Production-Safety Register**

### Starter Commands

1. **“Audit the IMO Ohuru QA client portal and document its public routes, registration fields, validation rules, and post-registration states.”**
2. **“Determine which role or account type is created by public registration without assuming a PRD persona.”**
3. **“Create the repetitive registration automation backlog and mark every inaccessible business module Pending.”**
4. **“Build a fast public smoke suite covering portal availability, registration-page access, form rendering, and login-page availability.”**
5. **“Generate unique parallel-safe synthetic account data for email, phone, name, and password fields actually exposed by the form.”**
6. **“Implement required-field, invalid-format, password-policy, and confirmation validation tests.”**
7. **“Implement successful registration, duplicate-account prevention, and double-submission protection.”**
8. **“Test post-registration login, pending-account restrictions, session persistence, logout, and protected-route handling where exposed.”**
9. **“Inspect the registration and authentication network sequence and produce a masked request and response inventory.”**
10. **“Compare the observed registration fields and states with the Version 1 and Version 2 PRDs.”**
11. **“Review resolved registration and authentication defects and convert each deterministic fix into a permanent regression test.”**
12. **“Create manual exploratory charters for mobile usability, accessibility, interrupted networks, unusual identity data, CAPTCHA, and unclear role assignment.”**
13. **“Create a Pending register for Super Admin, Cooperative Administrator, Farmer operations, Field Agent, tree planting, revenue, payments, SMS, USSD, analytics, reports, settings, and audit.”**
14. **“Add hard environment guards preventing account creation and state-changing tests from targeting Production.”**
15. **“Build an authorized Production read-only suite limited to availability, public-route health, login-page health, and non-destructive sanity checks.”**

---

## 9. Product Clarifications Required Before Full Automation

1. **Product Manager:** Is Version 2.0 the approved current requirements source?
2. **Product Manager:** Which Version 1 requirements were removed, replaced, or deferred in Version 2.0?
3. **Engineering Lead:** Is the supplied URL a QA environment only?
4. **Product Manager:** Which user role does public registration create?
5. **Product Manager:** Can a user select a role during registration?
6. **Product Manager:** Is public registration intended for Farmers, Cooperative Administrators, other users, or multiple personas?
7. **Engineering Lead:** What are the exact required and optional registration fields?
8. **Engineering Lead:** What uniqueness rules apply to email, phone, NIN, VIN, username, or other identity fields?
9. **Product Manager:** Are NIN and VIN still required as described in Version 1?
10. **Engineering Lead:** What email and phone normalization rules apply?
11. **Engineering Lead:** What password policy applies?
12. **Engineering Lead:** Is CAPTCHA or bot protection enabled?
13. **Product Manager:** Does registration create an active account immediately?
14. **Product Manager:** Does registration require email verification, phone OTP, Admin approval, or cooperative approval?
15. **Engineering Lead:** Is a QA email inbox or OTP retrieval mechanism available?
16. **Product Manager:** What are the authoritative account states?
17. **Engineering Lead:** What are the login, logout, session expiry, refresh, and lockout rules?
18. **Engineering Lead:** Is password reset implemented?
19. **Product Manager:** Can a user edit registration details after submission?
20. **Product Manager:** Can a user delete or withdraw a pending account?
21. **Engineering Lead:** Is there an approved cleanup or reset API for synthetic accounts?
22. **Engineering Lead:** Are stable `data-*` selectors available?
23. **Engineering Lead:** What API documentation is available?
24. **QA Lead:** How many new synthetic accounts may automation create per run or per day?
25. **QA Lead:** Which registration and authentication checks must run after every deployment?
26. **QA Lead:** What defect-management system and evidence fields should automation use?
27. **Product Manager:** What post-registration modules should the newly created account access?
28. **Product Manager:** What are the Super Admin portal URL and credentials?
29. **Product Manager:** What are the Cooperative Administrator portal URL and credentials?
30. **Product Manager:** Is there a separate Farmer portal or dashboard?
31. **Product Manager:** What are the Field Agent application URL and credentials?
32. **Product Manager:** What are the Stakeholder or Project Owner access details?
33. **Product Manager:** Is Buyer/Processor functionality implemented?
34. **Product Manager:** What is the cooperative application and approval workflow?
35. **Product Manager:** What Farmer fields and cooperative-assignment rules apply?
36. **Product Manager:** What tree-allocation and planting status model applies?
37. **Product Manager:** How are tree planting and verification records corrected or rejected?
38. **Engineering Lead:** What QR format and scanning application are used?
39. **Engineering Lead:** Is planting synchronization real-time, offline-capable, or both?
40. **Product Manager:** What revenue fields distinguish internal and external mills?
41. **Product Manager:** What formulas define revenue metrics and trends?
42. **Product Manager:** Are the Version 1 profit-split examples actual requirements or illustrative examples?
43. **Product Manager:** What percentages, bonuses, credits, and rounding rules apply?
44. **Engineering Lead:** What payment provider and sandbox are available?
45. **Product Manager:** What payment approval, retry, reversal, and reconciliation rules apply?
46. **Engineering Lead:** What SMS provider and sandbox are available?
47. **Product Manager:** What approved SMS templates and trigger rules apply?
48. **Engineering Lead:** What USSD code, aggregator, simulator, and test MSISDN are available?
49. **Product Manager:** Which local languages must SMS or USSD support?
50. **Product Manager:** What Admin roles and permission levels exist?
51. **Product Manager:** What settings are configurable by each role?
52. **Product Manager:** What dashboard metrics and formulas are authoritative?
53. **Product Manager:** What report and export formats are required?
54. **Engineering Lead:** How are audit logs made immutable?
55. **Product Manager:** Which activities must be audit-logged?
56. **Product Manager:** What browsers, devices, viewport sizes, accessibility standards, and languages are supported?
57. **Product Manager:** What response-time, availability, registration, login, report, SMS, and payment thresholds define acceptance?
58. **Operations Lead:** Which Production logs, monitoring dashboards, and alerts are available to QA?
59. **Operations Lead:** Which exact Production smoke and sanity checks are authorized?
60. **Release Manager:** What rollback scenarios must QA support?
61. **Security Owner:** What technical guard must prevent registration and state-changing suites from targeting Production?
62. **Security Owner:** What retention and deletion policy applies to synthetic QA accounts?
