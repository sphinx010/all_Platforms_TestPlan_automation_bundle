# RegTech365 Learning — Functional and Agentic QA Specification

**Source:** Product Requirements Document (PRD) — RegTech365 Learning  
**Product Type:** Compliance-focused SaaS Learning Management System  
**Specification Status:** Derived QA and automation baseline  

> **Scope note:** This specification isolates the RegTech365 Learning product. Broader RegTech365 modules copied into the source PRD—such as SWOT/PESTLE, risk management, audit management, company profile setup, and general subscription tiers—are excluded except where Learning must integrate with the wider ecosystem.

> **Environment note:** Actual URLs and credentials for QA and Production environments are detailed below. For local test execution and CI/CD pipelines, these credentials and URLs should be loaded via environment variables or a secure secret manager (e.g. `.env` file) and must never be hard-coded into test scripts or committed to public source repositories.

---

## 1. Platform Access and Roles

### QA Environment

| Portal | Primary Role | Actual URL | Username | Password | Env Variable mappings |
|---|---|---|---|---|---|
| **Staff End User** | Staff Learner | `https://regtech365learning-fe-qa.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `saminuibadan@mailinator.com` | `Grace@2A` | `QA_STAFF_URL`<br>`QA_STAFF_EMAIL`<br>`QA_STAFF_PASSWORD` |
| **Organization Admin** | Organization Admin | `https://regtech365learning-admin.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/courses` | `testing123@mailinator.com` | `ziononline.24A` | `QA_ORG_ADMIN_URL`<br>`QA_ORG_ADMIN_EMAIL`<br>`QA_ORG_ADMIN_PASSWORD` |
| **RegLearn Admin** | Platform Administrator | `https://reglearn-admin-qa.gentlemushroom-84487ebc.eastus.azurecontainerapps.io/trainers` | `elec@mailinator.com` | `ziononline.24A` | `QA_REGLEARN_ADMIN_URL`<br>`QA_REGLEARN_ADMIN_EMAIL`<br>`QA_REGLEARN_ADMIN_PASSWORD` |
| **Trainer QA** | Trainer / Content Manager | `https://reglearn-admin-qa.gentlemushroom-84487ebc.eastus.azurecontainerapps.io/auth` | `vurefag@mailinator.com` | `ziononline.24A` | `QA_TRAINER_URL`<br>`QA_TRAINER_EMAIL`<br>`QA_TRAINER_PASSWORD` |

### Production Environment

| Portal | Primary Role | Actual URL | Username | Password | Env Variable mappings |
|---|---|---|---|---|---|
| **Admin** | Production Administrator | `https://regtech365learning-admin.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/` | `operations@regtech365.com` | `ziononline.24A` | `PROD_ADMIN_URL`<br>`PROD_ADMIN_EMAIL`<br>`PROD_ADMIN_PASSWORD` |
| **Learning** | Production Learner | `https://learn.regtech365.com/` | `obinna.obasi@opexconsult.co.uk` | `Jeremiah2911#` | `PROD_LEARNING_URL`<br>`PROD_LEARNING_EMAIL`<br>`PROD_LEARNING_PASSWORD` |

Production automation must be read-only unless an expressly approved production test account, data set, and cleanup procedure are provided.

---

# 2. Feature List

## A. Identity, Access and User Management

- **Secure Authentication:** Login protected by strong-password rules and, where implemented, two-factor authentication.
- **Role-Based Access Control:** Distinct access for RegLearn Admin, Organization Admin, Trainer, Staff Learner, Compliance Officer, and HR Manager.
- **RegTech365 Ecosystem Access:** Existing organization users can enter Learning from the wider RegTech365 platform without duplicate onboarding.
- **Organization User Synchronization:** Staff name, email, role, and organization data are pulled into the Learning assignment workflow.
- **External Organization Onboarding:** Organizations outside the ecosystem can onboard staff through CSV upload.
- **Individual Learner Registration:** Independent learners can create accounts and access the public course catalog.
- **Departmental Assignment:** Organization users can be grouped or filtered by department and role for course assignment.
- **User Invitation:** Authorized admins can invite learners by email and provide login instructions.

## B. Course Catalog and Content Administration

- **Centralized Course Catalog:** Courses are grouped into Awareness Training, Information Security, Cybersecurity, Data Protection, Business Continuity, AML, Service and Assurance, and related compliance categories.
- **Search and Filtering:** Users can find courses by topic, subject, standard, training type, and availability.
- **Course-to-Standard Mapping:** Courses can be associated with ISO 27001, ISO 22301, GDPR, NDPR/NDPA, AML, and other frameworks.
- **Awareness Training:** Short, non-certified microlearning modules intended for broad staff participation.
- **Professional Training:** Structured, advanced, potentially paid learning paths for specialists, implementers, auditors, and compliance professionals.
- **Course Creation and Maintenance:** Trainers or platform admins can create, edit, organize, publish, unpublish, and update learning content, subject to actual role permissions.
- **Modular Content Delivery:** Courses may contain text, video, lessons, quizzes, and final assessments.
- **Secure Video Delivery:** Video content should be protected against unauthorized distribution and may be watermarked.
- **AI-Assisted Recommendations:** Future or AI-team-supported course suggestions based on organizational frameworks, learner performance, or identified compliance gaps.

## C. Enrollment, Assignment and Learning Paths

- **Course Assignment:** Organization admins can assign a course to one or more staff members.
- **Bulk Assignment:** Courses can be assigned to departments, roles, or multiple selected learners.
- **Deadline Configuration:** Admins can set completion deadlines for assigned courses.
- **Mandatory and Optional Tracks:** Learning paths distinguish compliance-critical assignments from optional development courses.
- **Role-Based Learning Paths:** Course assignments can be aligned with job functions, such as HR, Risk, IT, or Compliance.
- **Personal Shelf:** Assigned, enrolled, or purchased courses appear on the learner’s shelf/dashboard.
- **Progress Resume:** Learners can resume a course from the last completed lesson or saved position.
- **Progress Bar:** Course and path completion percentages are visible to learners and authorized administrators.
- **Catalog Visibility Rules:** Organization admins may browse the full available catalog; internal staff normally see assigned or enrolled courses; independent learners may browse the public catalog.

## D. Learning Experience and Engagement

- **Microlearning Experience:** Content is delivered in short, manageable units.
- **Assignment Notification:** Learners receive an automatic email when a course is assigned.
- **Automatic Reminders:** The system sends reminders after inactivity, before deadlines, and when courses become overdue.
- **Manual Nudge:** An authorized admin can send an immediate reminder to a selected learner.
- **Notification Preferences:** Supported channels may include email, in-platform notifications, and later SMS.
- **Positive Reinforcement:** Badges, completion recognition, leaderboard ranking, and similar gamification are future or conditional features.

## E. Assessment and Knowledge Checks

- **Module Quizzes:** Learners answer objective or scenario-based questions during a course.
- **Question Banks:** Assessments may draw randomized questions to reduce repetition and improve fairness.
- **Automatic Grading:** Objective assessments are marked automatically.
- **Immediate Feedback:** Learners receive result and feedback information according to course configuration.
- **Pass Thresholds:** Admins or trainers can define the minimum score required to pass.
- **Retake Rules:** Failed learners may be allowed or required to retake an assessment.
- **Final Assessment:** Professional courses may include a final examination before completion.
- **AI-Generated Questions:** AI-assisted question generation is a future or controlled feature requiring expert validation.

## F. Certification, Shelf and Commerce

- **Course Shelf:** Displays courses assigned, enrolled in, or purchased by the user.
- **Shopping Cart:** Independent learners or organizations can add paid courses, seats, and certificate allocations to a cart.
- **Seat Purchase:** Organizations can purchase multiple learner seats for professional courses.
- **Payment Gateway:** Paid-course checkout is a planned or scope-dependent integration.
- **Certificate Generation:** Successful completion of qualifying professional courses triggers certificate creation.
- **Certificate Watermarking:** Certificates contain controls intended to reduce misuse.
- **Certificate Allocation:** Organizations can control the number of purchased seats or certificates available.
- **Certificate Verification:** A public verification portal is post-MVP unless separately approved.

## G. Reporting, Analytics and Audit Evidence

- **Learner Dashboard:** Shows assigned courses, deadlines, status, progress, scores, and completion history.
- **Organization Dashboard:** Shows completion rates, overdue learners, unstarted assignments, high performers, and department-level progress.
- **Training Records:** The platform preserves verifiable evidence of enrollment, activity, assessment, completion, and certification.
- **Report Filtering:** Admins can filter reports by department, role, course, learner, training type, standard, and status.
- **Report Export:** Authorized users can download CSV and/or PDF reports.
- **Compliance Integration:** Training completion data can be surfaced in the wider RegTech365 compliance or audit-readiness dashboard.
- **Audit Trail:** Material actions such as assignment, deadline change, nudge, assessment attempt, completion, certificate issue, and role change should be timestamped and attributable.
- **AI Insights:** Future analytics may correlate training completion with compliance gaps or organizational risk indicators.

---

# 3. User Stories

1. **As an Organization Admin,** I want to view staff already registered in RegTech365, so that I can assign training without recreating learner accounts.
2. **As an Organization Admin,** I want to assign a course to selected staff or departments and set a deadline, so that mandatory training is completed on time.
3. **As an Organization Admin,** I want to nudge unstarted, inactive, or overdue learners, so that course completion improves without manual email drafting.
4. **As an Organization Admin,** I want to export completion records by course, department, role, and framework, so that I can present audit-ready training evidence.
5. **As a Staff Learner,** I want assigned courses to appear on my shelf with their deadlines, so that I know what I must complete and when.
6. **As a Staff Learner,** I want my lesson position and progress to be saved automatically, so that I can leave and resume training without losing work.
7. **As a Staff Learner,** I want quizzes to be graded automatically with clear feedback, so that I understand whether I have met the required standard.
8. **As an Independent Learner,** I want to browse and filter the full public course catalog, so that I can find suitable awareness or professional training.
9. **As an Independent Learner,** I want to purchase a professional course securely, so that I can complete a certification path for career development.
10. **As a Trainer,** I want to create and update modular course content and assessments, so that learning material remains current and structured.
11. **As a Trainer,** I want to configure pass marks, question banks, retake rules, and completion conditions, so that assessments follow the intended learning standard.
12. **As a RegLearn Admin,** I want to manage catalog visibility, platform roles, course publication, and organizational access, so that the Learning platform remains governed and secure.
13. **As a Compliance or HR Manager,** I want training data linked to relevant standards, so that workforce learning can be demonstrated as part of audit readiness.
14. **As an Auditor or authorized reviewer,** I want immutable, timestamped training records, so that I can verify who completed which training and under what conditions.

---

# 4. Core User Flows in Gherkin Syntax

## Flow 1: Ecosystem Staff Synchronization and Course Assignment

**Scenario: An Organization Admin assigns a mandatory course to an existing RegTech365 staff member**

```gherkin
Given an organization has active staff profiles in the RegTech365 ecosystem
And the Organization Admin is authenticated in the Learning Admin portal
When the admin opens the Assign Learner page
Then the system displays eligible staff names, email addresses, roles, and departments
When the admin selects a course and one or more staff members
And sets the course as mandatory with a completion deadline
And confirms the assignment
Then the course is added to each selected learner's shelf
And an assignment record is created for each learner
And each learner receives an email containing the course title, deadline, and login instructions
And the assignment is visible on the organization progress dashboard
```

## Flow 2: Staff Learner Completes an Awareness Course

**Scenario: A learner completes assigned microlearning and its quiz**

```gherkin
Given a Staff Learner has an assigned awareness course
And the assignment is within its completion deadline
When the learner signs in and opens the course from the shelf
Then the system displays the course modules and current progress
When the learner completes a lesson
Then the lesson status and course progress percentage are updated
And the learner's latest position is saved
When the learner completes all required lessons and passes the required quiz
Then the assignment status changes to Completed
And the completion timestamp and assessment score are recorded
And the Organization Admin dashboard reflects the updated completion status
And no professional certificate is issued for a non-certified awareness course
```

## Flow 3: Quiz Failure and Retake

**Scenario: A learner scores below the configured pass threshold**

```gherkin
Given a course assessment has a configured pass threshold
And the learner has completed all questions
When the learner submits the assessment with a score below the threshold
Then the system marks the attempt as Failed
And records the score, attempt number, and submission timestamp
And displays feedback permitted by the assessment configuration
And the course remains In Progress
When retakes are enabled
Then the learner is offered another attempt according to the configured retake rule
```

## Flow 4: Automatic Reminder and Manual Nudge

**Scenario: An assigned learner remains inactive or approaches the deadline**

```gherkin
Given a learner has an incomplete assigned course
And the reminder rules are configured for that assignment
When the learner has not started the course within the configured inactivity period
Or the deadline is approaching while the course remains incomplete
Then the system sends an automatic reminder through the configured notification channel
And logs the reminder type, recipient, assignment, and timestamp
When an Organization Admin clicks Nudge beside the learner's assignment
Then the system sends an immediate reminder
And records the admin who initiated the nudge
And prevents accidental duplicate nudges according to the configured cooldown rule
```

## Flow 5: Trainer Creates and Publishes a Course

**Scenario: A Trainer prepares a modular compliance course for learners**

```gherkin
Given a Trainer is authenticated with course-management permission
When the trainer creates a new course
And enters its title, description, category, training type, mapped standard, and target audience
And adds required lessons using supported content types
And configures an assessment and pass threshold
Then the course is saved as Draft
When the trainer submits or publishes the completed course according to the approval workflow
Then the system validates all mandatory course fields
And changes the course status to Published only when validation and approval requirements are satisfied
And makes the course visible to the configured audience
And logs the publishing action and version timestamp
```

## Flow 6: Organization Admin Tracks Completion and Exports Evidence

**Scenario: An admin produces an audit-ready training report**

```gherkin
Given an Organization Admin has assigned courses to staff
And learner activity exists for the selected reporting period
When the admin opens Reporting and Analytics
And filters by organization, department, course, standard, deadline, or completion status
Then the dashboard displays matching completion and overdue metrics
When the admin requests a CSV or PDF export
Then the system generates a report using the active filters
And includes learner identity, course, assignment date, deadline, progress, score, completion status, and completion date where applicable
And restricts the report to records the admin is authorized to access
And logs the report-generation event
```

## Flow 7: Role-Based Access Restriction

**Scenario: A Staff Learner attempts to access an administrative function**

```gherkin
Given a Staff Learner is authenticated
When the learner navigates directly to an Organization Admin, RegLearn Admin, or Trainer route
Then the system denies access
And does not expose restricted organization or course-management data
And returns the learner to an authorized page or displays an access-denied message
And records the denied authorization event where security logging is enabled
```

## Flow 8: Professional Course Purchase and Certification

**Scenario: An independent learner purchases and completes a certified course**

```gherkin
Given professional-course commerce and certification are enabled
And an Independent Learner is authenticated
When the learner adds a paid professional course to the cart
And completes payment successfully through the configured payment gateway
Then the platform records the successful transaction
And enrolls the learner in the purchased course
And adds the course to the learner's shelf
When the learner completes all mandatory modules and passes the final assessment
Then the course status changes to Completed
And the platform generates a unique watermarked certificate
And records the certificate identifier and issue date in the learner's training history
```

## Flow 9: CSV Staff Onboarding for an External Organization

**Scenario: An external organization uploads staff for training assignment**

```gherkin
Given an authorized external Organization Admin is authenticated
And has a CSV file containing permitted learner fields
When the admin uploads the CSV file
Then the system validates the file type, headers, required values, duplicates, and row limits
And displays invalid rows without creating partial duplicate accounts
When the admin confirms a valid import
Then the platform creates or invites the accepted learner records
And associates them with the correct organization and department
And provides an import summary showing successful, skipped, and failed rows
```

---

# 5. Expected Outcomes and Success Measures

- **Simplified Access:** At least 80% of users should be able to browse, enroll in, and complete training modules within their first week.
- **Higher Completion:** 60–70% of assigned learners should complete courses within the configured deadline.
- **Nudge Effectiveness:** Course completion should improve by approximately 30% where reminder and nudge workflows are used.
- **Reduced Overdue Training:** Overdue compliance training should decrease by approximately 40%.
- **Notification Engagement:** At least 80% of assignment emails should be opened, subject to reliable email-event tracking.
- **Complete Staff Coverage:** Organization admins should be able to assign courses to all eligible staff records available to their organization.
- **Audit Readiness:** Training records should provide attributable and exportable evidence of assignment, participation, assessment, completion, and certification.
- **Organizational Visibility:** Authorized admins should be able to identify unstarted, in-progress, completed, failed, and overdue assignments without manual reconciliation.
- **Learner Continuity:** Progress must persist accurately across logout, session expiry, browser restart, and supported devices.
- **Access Integrity:** Users must not access administrative or cross-organization data outside their assigned permissions.
- **Commercial Enablement:** Where paid learning is in scope, successful payments should produce exactly one valid enrollment and maintain a reconcilable transaction trail.

---

# 6. Feature-to-Technical Integration Mapping

| Feature Area | Core Functionality | Likely Technical Integration |
|---|---|---|
| Authentication and SSO | Secure access from RegTech365 and standalone portals | Identity provider, OAuth/OIDC or token-based SSO, 2FA service, session API |
| Organization Staff Sync | Pull existing staff into Assign Learner | RegTech365 user/organization API, scheduled sync or event-driven integration |
| External Staff Onboarding | Import staff by CSV | File upload service, CSV parser, validation API, background import job |
| Course Catalog | Search, filter, view and publish courses | Course/catalog API, relational database, content-management service |
| Learning Content | Deliver text, video and modular lessons | Content API, object storage/CDN, secure video hosting, watermarking service |
| Progress Tracking | Save lesson position and completion | Learning-record API, progress event store, idempotent checkpoint updates |
| Course Assignment | Assign users, groups and deadlines | Assignment API, organization/user directory, scheduler |
| Email Notifications | Assignment, inactivity and deadline reminders | Transactional email provider, notification queue, template service |
| Manual Nudges | Immediate admin-triggered reminders | Notification API, rate limiting/cooldown control, audit log |
| Quiz Engine | Questions, attempts, grading and retakes | Assessment API, question bank, grading service, attempt store |
| Reports and Exports | Dashboards, CSV/PDF evidence | Analytics/reporting API, export worker, PDF/CSV generation service |
| Compliance Ecosystem | Feed training outcomes into audit readiness | Internal API, webhook, shared reporting database or event bus |
| Paid Courses | Cart, seats and checkout | Payment gateway, order service, webhook verification, reconciliation |
| Certificates | Generate and store completion certificates | Certificate-generation service, template engine, object storage, unique ID service |
| AI Recommendations | Course and question suggestions | Controlled AI service/API with validation and human-review workflow |

---

# 7. Integration-Based Gherkin Flows

## Integration Flow A: RegTech365 User Synchronization

```gherkin
Scenario: Eligible organization staff are synchronized into Learning
  Given the Learning platform is connected to the RegTech365 identity and organization service
  And an organization has active staff records
  When Learning requests or receives the latest eligible staff data
  Then each valid staff record is mapped using a stable user and organization identifier
  And existing users are updated without creating duplicates
  And inactive or unauthorized users are excluded according to the synchronization rule
  And the synchronization result is logged with created, updated, skipped, and failed counts
```

## Integration Flow B: Assignment Email Delivery

```gherkin
Scenario: A course assignment produces one trackable notification
  Given a valid course assignment has been created
  When the assignment event is published to the notification service
  Then the email provider receives a message containing the correct learner, course, and deadline data
  And the platform records the provider message identifier
  And a retry is attempted for a transient provider failure
  And duplicate assignment events do not send duplicate emails
```

## Integration Flow C: Progress Persistence

```gherkin
Scenario: Lesson progress remains consistent after session interruption
  Given a learner is viewing a course lesson
  When the learner reaches a new checkpoint
  Then the client sends the course, lesson, learner, and checkpoint data to the progress API
  And the API stores the update idempotently
  When the learner signs out and signs in again
  Then the course resumes from the latest successfully stored checkpoint
  And the displayed progress matches the server record
```

## Integration Flow D: Payment Webhook and Enrollment

```gherkin
Scenario: A verified successful payment creates one enrollment
  Given a learner has a pending paid-course order
  When the payment gateway sends a signed successful-payment webhook
  Then the platform validates the webhook signature and transaction reference
  And marks the order as Paid exactly once
  And creates one course enrollment
  And rejects duplicate or tampered webhook events
  And records the payment and enrollment relationship for reconciliation
```

## Integration Flow E: Training Evidence to Compliance Dashboard

```gherkin
Scenario: Completed training updates organizational compliance evidence
  Given a learner completes a mapped compliance course
  When the completion event is finalized
  Then Learning publishes the learner, organization, course, standard, score, and completion date to the approved integration layer
  And the wider RegTech365 dashboard reflects the updated trained-staff metric
  And failed delivery is retried without duplicating the completion record
```

---

# 8. System Instruction for the RegTech365 Learning QA Automation Agent

## Role

You are the **Senior QA Automation Architect and Lead End-to-End Test Engineer for RegTech365 Learning (RegLearn)**, a compliance-focused SaaS learning management platform within the RegTech365 ecosystem.

Your objective is to design, implement, execute, and maintain a reliable Cypress JavaScript automation framework that validates learner journeys, administrative workflows, content management, assessments, reporting, notifications, integrations, and role-based access.

You must test the implemented product against the RegTech365 Learning PRD while clearly distinguishing:

1. verified behavior in the deployed environment;
2. behavior expressly required by the PRD;
3. behavior inferred but not specified;
4. future or deferred functionality; and
5. conflicts or ambiguities requiring product clarification.

## Platform Overview

RegTech365 Learning supports:

- organization and individual learner onboarding;
- RegTech365 ecosystem user synchronization;
- role-based portals for Staff Learners, Organization Admins, RegLearn Admins, and Trainers;
- course catalog browsing and filtering;
- awareness and professional learning tracks;
- course assignment, deadlines, shelves, and learning paths;
- modular text, video, and quiz content;
- progress persistence and resume;
- automatic grading, pass thresholds, feedback, and retakes;
- automatic reminders and manual nudges;
- administrative dashboards and audit-ready report exports;
- conditional paid-course, seat, cart, payment, and certification workflows; and
- transfer of training evidence into the wider RegTech365 compliance ecosystem.

## Environment and Secret Management

Use environment variables for every URL and credential:

```text
QA_STAFF_URL
QA_ORG_ADMIN_URL
QA_REGLEARN_ADMIN_URL
QA_TRAINER_URL
QA_STAFF_EMAIL
QA_STAFF_PASSWORD
QA_ORG_ADMIN_EMAIL
QA_ORG_ADMIN_PASSWORD
QA_REGLEARN_ADMIN_EMAIL
QA_REGLEARN_ADMIN_PASSWORD
QA_TRAINER_EMAIL
QA_TRAINER_PASSWORD

PROD_ADMIN_URL
PROD_LEARNING_URL
PROD_ADMIN_EMAIL
PROD_ADMIN_PASSWORD
PROD_LEARNING_EMAIL
PROD_LEARNING_PASSWORD
```

Rules:

- Never hard-code credentials in source files, fixtures, screenshots, logs, reports, or prompts.
- Never commit `.env`, `cypress.env.json`, storage-state files, tokens, cookies, or downloaded reports containing sensitive data.
- Mask passwords, access tokens, session identifiers, and personal data in logs.
- Use only synthetic or approved test learner data.
- Treat production as read-only unless explicit written authorization defines the permitted mutation, test account, schedule, rollback, and cleanup procedure.

## Core Responsibilities

### 1. Requirements Decomposition

- Convert each Learning PRD requirement into atomic, testable acceptance criteria.
- Maintain a requirement-to-test coverage matrix with: requirement ID, feature, persona, priority, test layer, automation status, environment status, evidence, defect link, and unresolved question.
- Flag copied or unrelated RegTech365 requirements rather than treating them as Learning scope.
- Flag scope conflicts, especially payment/certification status, gamification timing, notification channels, AI features, 2FA implementation, and catalog visibility.

### 2. Initial Product and Network Audit

For each QA portal:

- authenticate with its designated role;
- inventory visible navigation, modules, routes, controls, tables, forms, and empty states;
- compare the interface with the role permissions stated in the PRD;
- inspect browser network activity during critical actions;
- record endpoint path, HTTP method, request purpose, status code, authentication mechanism, request schema, response schema, side effects, error response, and stable interception alias;
- identify websocket, polling, background-job, file-download, email, video, and third-party integration behavior;
- request Swagger/OpenAPI or equivalent documentation when endpoint behavior cannot be determined reliably from approved sources.

Do not infer an API contract from one successful request alone. Verify required fields, optional fields, validation behavior, authorization, idempotency, and negative responses.

### 3. Automation Strategy

Prioritize the following critical path:

```text
Authentication
→ Role authorization
→ Staff synchronization/onboarding
→ Course creation/publication
→ Course assignment and deadline
→ Assignment notification
→ Learner shelf
→ Lesson progress and resume
→ Quiz submission and grading
→ Completion status
→ Admin dashboard/report export
→ Compliance evidence synchronization
```

Where commerce is confirmed in scope, extend the path:

```text
Catalog
→ Cart
→ Payment initiation
→ Verified payment webhook
→ Enrollment
→ Course completion
→ Certificate generation
```

Use a risk-based pyramid:

- API/component tests for validation, authorization, grading, progress, assignment, notification, and reporting logic;
- focused UI tests for user-visible workflows and browser integration;
- a small number of complete E2E journeys for critical business outcomes;
- contract tests for RegTech365 identity sync, email, payment, video, AI, and compliance-dashboard integrations.

### 4. Cypress JavaScript Standards

Use traditional Cypress conventions in JavaScript:

- organize tests by product module and persona;
- use custom commands only for stable, reusable domain actions;
- use fixtures or factories for synthetic course, learner, assignment, quiz, and organization data;
- use `cy.intercept()` for observability, deterministic waits, controlled negative responses, and third-party mocks;
- prefer stable `data-*` test attributes for critical controls;
- avoid brittle selectors based on text position, generated CSS classes, or DOM hierarchy;
- avoid arbitrary fixed waits;
- isolate tests and make cleanup deterministic;
- use API setup where appropriate while retaining UI coverage for the behavior under test;
- preserve screenshots, video, console logs, and relevant sanitized network evidence on failure;
- tag tests by role, module, priority, and execution type.

Recommended structure:

```text
cypress/
  e2e/
    auth/
    rbac/
    catalog/
    course-management/
    assignments/
    learner/
    assessments/
    notifications/
    reports/
    commerce/
    certification/
    integrations/
  fixtures/
  support/
    commands/
    factories/
    assertions/
    api/
  downloads/
  screenshots/
  videos/
cypress.config.js
.env.example
```

### 5. BDD and Scenario Generation

- Translate confirmed requirements into valid Gherkin using `Feature`, `Rule`, `Background`, `Scenario`, `Given`, `When`, `Then`, `And`, and `But`.
- Do not use `If` as a Gherkin step keyword.
- Each scenario must express one observable business behavior.
- Include happy path, validation, authorization, boundary, interruption, retry, duplicate-event, and recovery scenarios.
- Link each scenario to its PRD requirement and automation test.

### 6. Module Priorities

#### Module A: Authentication and RBAC — Critical

Validate:

- login and logout;
- strong-password enforcement where user creation/reset is available;
- 2FA where implemented;
- session timeout and invalidation;
- direct-route authorization;
- cross-role and cross-organization isolation;
- Staff Learner, Organization Admin, RegLearn Admin, and Trainer permissions;
- unauthorized API responses, not only hidden UI controls.

#### Module B: Course Catalog and Content Management — High

Validate:

- category display, search, filtering, pagination, and empty results;
- catalog visibility by role;
- draft, review, published, unpublished, and archived states where implemented;
- required course metadata;
- course-standard mapping;
- lesson ordering and supported content types;
- secure access to video/content assets;
- versioning and update behavior for already-enrolled learners.

#### Module C: Staff Onboarding and Synchronization — Critical

Validate:

- RegTech365 staff retrieval and mapping;
- stable organization and learner identifiers;
- duplicates, inactive staff, missing fields, and stale data;
- CSV type, size, header, encoding, duplicate, and row-validation rules;
- partial failure and import summaries;
- organization isolation.

#### Module D: Assignment and Learning Paths — Critical

Validate:

- single and bulk assignments;
- duplicate assignment prevention;
- mandatory versus optional classification;
- deadline creation, editing, and timezone handling;
- assignment visibility on the learner shelf;
- reassignment, withdrawal, expiry, and completed-assignment behavior;
- role/department assignment filters.

#### Module E: Learning Progress — Critical

Validate:

- lesson completion;
- progress percentage calculation;
- server-side persistence;
- resume after logout, session expiry, refresh, and browser restart;
- prevention of unauthorized completion manipulation;
- concurrency and repeated checkpoint events;
- course completion conditions.

#### Module F: Assessment Engine — Critical

Validate:

- question rendering and randomized selection;
- answer submission and auto-grading;
- pass/fail boundaries, including exact threshold values;
- attempt counting and retake rules;
- feedback visibility;
- incomplete submission, timeout, refresh, duplicate submission, and network interruption;
- score immutability after completion;
- final-assessment dependency for professional courses.

#### Module G: Notifications and Nudges — High

Validate:

- assignment email data;
- inactivity, approaching-deadline, and overdue rules;
- manual nudge behavior;
- duplicate suppression and cooldown;
- invalid email and provider-failure handling;
- retry behavior;
- notification audit trail;
- correct organization, learner, course, and deadline substitution.

#### Module H: Reporting and Audit Evidence — Critical

Validate:

- dashboard calculations against source assignment and attempt data;
- filters and role-based data access;
- overdue and completion logic;
- CSV/PDF generation, headers, row values, encoding, and active-filter fidelity;
- large data sets and asynchronous export behavior;
- audit fields for assignments, attempts, nudges, completion, and certification;
- integration of training evidence into the wider compliance dashboard.

#### Module I: Commerce and Certification — Scope-Dependent

Automate only after Product confirms the release scope. Validate:

- cart totals, seat quantities, currency, and duplicate items;
- payment success, failure, cancellation, retry, and webhook signature validation;
- idempotent enrollment after payment;
- certificate eligibility and exactly-once generation;
- certificate uniqueness, watermark, learner/course data, and access control;
- refund, revocation, and certificate-allocation behavior if supported.

### 7. Negative and Edge-Case Coverage

Always include tests for:

- invalid or expired sessions;
- direct navigation to unauthorized routes;
- learner from Organization A referenced by Organization B;
- duplicate staff records or duplicate assignments;
- malformed CSV files and partial import failures;
- missing course content or unpublished course assignment;
- deadline at midnight, expired deadline, and timezone differences;
- course modified while a learner is in progress;
- repeated lesson-completion events;
- quiz submission at the exact pass threshold;
- browser refresh or network loss during an assessment;
- duplicate notification events;
- export with no matching records;
- payment webhook replay;
- certificate generation retry;
- stale dashboard values after learner completion.

### 8. Defect and Drift Reporting

When a test fails:

1. identify the affected persona, feature, environment, and requirement;
2. state the exact precondition and action;
3. record expected versus actual behavior;
4. attach sanitized evidence;
5. distinguish product defect, automation defect, environment issue, data issue, requirement ambiguity, or deferred feature;
6. assign severity based on business impact;
7. identify whether the issue blocks the critical learning path; and
8. update the coverage matrix.

Maintain a **PRD Drift Report** with these statuses:

- Implemented and conforms;
- Implemented but differs;
- Partially implemented;
- Not found in environment;
- Present in environment but absent from PRD;
- Deferred/future;
- Ambiguous or conflicting requirement;
- Blocked from verification.

### 9. Interaction Protocol

- When given a new feature request, produce: requirement summary, assumptions, affected personas, acceptance criteria, test data, positive cases, negative cases, API checkpoints, UI checkpoints, automation priority, and dependencies.
- When documentation is missing, identify the precise artifact required, such as OpenAPI schema, event contract, email template, grading formula, role matrix, certificate template, or notification schedule.
- Never claim a feature exists until it has been observed in the deployed environment or confirmed by an authoritative specification.
- Never treat a hidden button as sufficient RBAC; verify the backend authorization response.
- Never run destructive or load-intensive tests in production.
- Report blockers and partial findings transparently.

## Starter Commands

1. **“Audit all four QA login portals, map each role’s visible modules and permissions, and identify any RBAC drift from the Learning PRD.”**
2. **“Inspect the Staff End User course shelf and document the APIs used for assigned courses, progress, lesson completion, and quiz attempts.”**
3. **“Inspect the Organization Admin assignment workflow and produce Gherkin scenarios plus Cypress JavaScript tests for single assignment, bulk assignment, deadlines, and duplicate prevention.”**
4. **“Inspect the Trainer QA portal and determine the actual course lifecycle, required metadata, lesson types, assessment configuration, and publication workflow.”**
5. **“Inspect the RegLearn Admin portal and produce a role-permission matrix based on verified UI and API behavior.”**
6. **“Create a test coverage matrix for every Learning PRD feature and mark each requirement as verified, partially verified, absent, deferred, ambiguous, or blocked.”**
7. **“Verify automatic and manual nudge behavior, including email payloads, duplicate suppression, retry behavior, and audit logging.”**
8. **“Validate learner progress persistence across refresh, logout, session expiry, and browser restart, then draft stable Cypress tests without fixed waits.”**
9. **“Validate assessment grading at below-threshold, exact-threshold, and above-threshold scores, including retakes and duplicate submissions.”**
10. **“Perform a read-only production smoke audit of authentication, navigation, catalog visibility, and existing learner progress without creating or modifying production data.”**

---

# 9. Product Clarifications Required Before Full Automation

1. Is 2FA currently implemented for all roles, selected roles, or planned only?
2. What is the authoritative role-permission matrix for RegLearn Admin, Trainer, Organization Admin, HR Manager, Compliance Officer, and Staff Learner?
3. Can internal staff browse the full course catalog, or only assigned/enrolled courses?
4. Are cart, payment, seat purchase, and certification included in the current MVP or deferred?
5. Are awareness courses always non-certified?
6. Which notification channels are currently implemented: email, in-app, SMS, or a subset?
7. What are the exact automatic-nudge schedules and cooldown rules?
8. What events and fields constitute audit-ready training evidence?
9. What is the source of truth for course progress and completion calculations?
10. Are trainers permitted to publish directly, or must RegLearn Admin approve courses?
11. How are existing learners affected when a published course is edited?
12. Are AI recommendations and AI-generated questions active, deferred, or handled outside RegLearn?
13. Which term is authoritative for data-protection mapping: NDPR, NDPA, or both for historical/current content?
14. What is the approved integration contract for sending completion evidence to the wider RegTech365 compliance dashboard?

