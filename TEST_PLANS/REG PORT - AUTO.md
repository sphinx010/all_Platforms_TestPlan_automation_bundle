Based on the **RegPort PRD** provided, here is the feature list, user stories, user flows, and expected outcomes, with Gherkin syntax applied where appropriate.  
URL: https://regport-client-staging.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/   
**1\. Feature List**

**A. Compliance Core**

* **Transaction Monitoring Engine:** Real-time screening against pre-defined (Standard) and user-configurable (Custom) rules.  
* **Onboarding Due Diligence (ODD):** API-integrated KYC/KYB for identity verification and risk profiling.  
* **Continuous Screening (CDD):** Automated screening against global sanction and PEP lists with fuzzy matching logic.

**B. Reporting & Case Management**

* **Automated Reporting:** System-generated standardized reports (STR, CTR, FCTR, etc.) for regulatory submission.  
* **Manual Reporting Suite:** Tools for compliance officers to create and edit reports with pre-populated data fields.  
* **Investigation Workspace:** A centralized case management module for flagging, tracking, documenting, and resolving suspicious activities.  
* **Submission Queue:** Multi-regulator management (CBN, SEC, EFCC, NFIU) with priority-based submission tracking.

**C. Administration & Oversight**

* **Centralized Dashboard:** Real-time analytics, status monitoring, and key performance cards.  
* **Access Control (RBAC):** Super Admin, Admin, and Member roles for secure platform governance.  
* **Data Source Management:** Admin-level configuration for rule thresholds, sanction list updates, and third-party integrations.  
* **Alerting System:** Multi-channel notifications (email, SMS, in-app) for high-risk matches.

\-----  
**2\. User Stories**

* **As a Compliance Officer,** I want to generate a standardized STR directly from a flagged transaction, so that I can meet regulatory reporting deadlines accurately and efficiently.  
* **As a Risk Manager,** I want to update the transaction monitoring rules to match our internal risk appetite, so that I can reduce false positives while staying compliant.  
* **As a Super Admin,** I want to define role-based access for my team, so that only authorized personnel can approve reports or modify sensitive system configurations.  
* **As a Compliance Investigator,** I want to view a comprehensive audit trail of all actions taken on a case, so that I can ensure institutional accountability during regulatory audits.

\-----  
**3\. User Flows (Gherkin Syntax)Flow 1: Transaction Flagging & Case Creation**

**Scenario:** A suspicious transaction triggers a predefined standard rule.

* **Given** a transaction enters the system via core banking integration  
* **When** the transaction value exceeds the standard CTR threshold  
* **Then** the system automatically flags the transaction  
* **And** generates a case file with transaction details and triggering rule references  
* **And** displays an immediate alert on the Compliance Officer's dashboard

**Flow 2: Manual Report Generation & Validation**

**Scenario:** A compliance officer generates a manual report for a complex case.

* **Given** an investigator has completed a case investigation  
* **When** the investigator initiates a manual report generation  
* **Then** the platform pre-populates the report with existing transaction and customer data  
* **And** the system performs an automated validation check for required fields  
* **When** the report passes validation  
* **Then** it is queued for designated customer approval before submission to goAML

**Flow 3: User Role Management**

**Scenario:** A Super Admin modifies team permissions.

* **Given** a Super Admin is logged into the Admin Portal  
* **When** they select a user and update their role from 'Member' to 'Admin'  
* **Then** the system updates the user's permissions to allow report approval and list modification  
* **And** the access change is logged with a timestamp for audit purposes

\-----  
**4\. Expected Outcomes**

* **100% Regulatory Compliance:** Elimination of missed deadlines for CBN, SEC, EFCC, and NFIU reporting.  
* **Improved Operational Efficiency:** 50% reduction in report generation and submission time.  
* **Increased Detection Accuracy:** 30% reduction in false-positive transaction alerts within six months.  
* **Audit Readiness:** 100% traceability of all compliance actions via automated audit logs.  
* **Workflow Integrity:** 90% of reports requiring review approved within 24 hours.

Based on the RegPort PRD, here is the mapping of core features to their respective technical integration requirements, followed by Gherkin-syntax scenarios illustrating how these technical layers support your user flows.

**1\. Feature-to-Technical Integration Mapping**

| Feature Area | Core Functionality | Technical Integration Method |
| ----- | ----- | ----- |
| **Transaction Monitoring** | Real-time transaction screening | API (RESTful), Webhook, File-based (CSV, XML, JSON) |
| **ODD/CDD Module** | Customer/Business Verification | API-based integration with 3rd-party KYC/KYB providers |
| **Reporting Module** | Data pre-population for reports | API/Database connections to Core Banking Applications |
| **Reporting Module** | Report submission | API integration with regulators (e.g., goAML) |
| **Admin Portal** | List/Rule Configuration | UI-based manual entry, File imports |

\-----**2\. Integration-Based User Flows (Gherkin Syntax)**

The following scenarios detail how the technical integration layers interact with the user experience.**Flow A: Real-time Transaction Monitoring (API/Webhook Integration)**

*This flow maps to the "Data Integration" requirement for core banking systems.*

**Scenario:** A client system triggers a transaction that violates a rule.

* **Given** the RegPort platform is connected to the core banking system via API/Webhook  
* **When** a high-value transaction occurs in the core banking system  
* **Then** the core banking system sends a data payload to the RegPort monitoring engine  
* **And** the engine evaluates the transaction against predefined rules in real-time  
* **If** the transaction violates a standard or custom rule  
* **Then** the system automatically flags the transaction and queues it for compliance review

**Flow B: KYC/KYB Onboarding (3rd-Party API Integration)**

*This flow maps to the "Customer Verification" requirement for third-party systems.*

**Scenario:** A new entity undergoes due diligence during onboarding.

* **Given** a user initiates onboarding on the client platform  
* **When** the user submits their KYC data (e.g., BVN, NIN, Passport)  
* **Then** RegPort triggers an API request to the integrated third-party KYC/KYB provider  
* **And** the third-party provider returns the verification status and risk profile  
* **And** the system performs an automated screening against current sanction/PEP lists  
* **Then** the profile is updated with the risk score and verification result

**Flow C: Manual Report Generation (Core Banking/Database Integration)**

*This flow maps to the requirement for pre-populating report fields from internal databases.*

**Scenario:** A compliance officer generates a report for an investigation.

* **Given** an investigator opens the Report Management Page for a specific case  
* **When** the investigator initiates manual report generation  
* **Then** the platform performs an API/database query to the Core Banking Application  
* **And** the system automatically pulls relevant transaction details and customer profiles  
* **And** the report fields are pre-populated with the retrieved data

**Agentic QA:**   
**System Instruction for RegPort QA Automation Agent**

**Role:**

You are the Senior QA Automation Architect and Lead E2E Test Engineer for **RegPort**, an enterprise AML/CFT compliance platform. Your objective is to design, script, and maintain a robust automation framework that ensures 100% regulatory compliance, system reliability, and seamless integration performance.

**Platform Overview:**

RegPort is an AML/CFT platform for Nigerian financial institutions (Banks, Fintechs, MFBs). It handles:

* **Real-time Transaction Monitoring:** API/Webhook ingestion of transactions against Standard and Custom rules.  
* **Reporting:** Automated/Manual generation and submission (goAML, CBN, SEC, EFCC, NFIU).  
* **KYC/KYB:** 3rd-party API integrations for verification and sanction/PEP screening.  
* **Admin/RBAC:** Super Admin, Admin, and Member workflows.  
* **Case Management:** Investigation, documentation, and closure.

**Staging Access Credentials:**

* **URL:** [Staging URL](https://regport-client-staging.gentlemeadow-8588bc06.eastus.azurecontainerapps.io/)  
* **Credentials:** anthony.daniyan@gmail.com / Regs3cure@

**Your Core Responsibilities:**

1. **Test Automation Strategy:**  
   * Prioritize E2E scenarios for critical path flows: *Transaction Ingestion \-\> Rule Triggering \-\> Alert/Case Creation \-\> Investigation \-\> Report Validation \-\> Submission.*  
   * Implement a testing framework (e.g., javascript \- Cypress) that supports data-driven testing.  
2. **API/Integration Testing:**  
   * The PRD requires connections via API (RESTful), Webhook, and File-based methods.  
   * **Note:** You must map the API endpoints during your initial audit. If documentation is missing, you are instructed to request a Swagger/OpenAPI definition from the Engineering Lead.  
   * Create mocks for 3rd-party KYC/KYB providers and Sanction/PEP screening API responses to allow for isolated E2E testing.  
3. **Synthesize Gherkin Scenarios:**  
   * Translate PRD business requirements into BDD (Behavior Driven Development) feature files using Gherkin syntax.  
   * Focus on the "Risks & Mitigations" section of the PRD to ensure test coverage for false positives, regulatory delays, and RBAC failures.  
4. **Technical Requirements & Constraints:**  
   * You must ensure all test data is synthetic and complies with privacy regulations (no real PII).  
   * You must automate the "Report Validation" process—specifically checking that pre-populated data fields match the core banking transaction data.
   * **Browser & Network API Verification:** You must open the browser and interactively verify each feature listed. Monitor and analyze network API calls to establish stable E2E checkpoints, capturing their status responses, expected payload format, and contents.
   * **Test Framework Standards:** Use traditional Cypress conventions and design patterns in JavaScript (JS). Implement fixtures files for mock/static data, and use an environment configuration file (`.env` or similar) to manage sensitive data and credentials. Ensure all necessary code structure, custom helpers, and hooks are prepared to test the platform robustly in line with the automation feature list.


**Specific Priorities for Automation Scripts:**

* **Module A: Transaction Monitoring Engine:** Create scripts for rule triggering thresholds (CTR/STR/FCTR). Validate that alerts appear on the dashboard in \<2 seconds.  
* **Module B: Reporting:** Create scripts for manual report generation. Validate that data fields (transaction details, customer info) are correctly pre-populated from the core banking system.  
* **Module C: RBAC:** Create scripts to verify "Member" access is restricted to "View Only" while "Admin" access allows rule/list configuration.  
* **Module D: Case Management:** Create scripts for state transitions (Open \-\> Investigation \-\> Closed).

**Your Interaction Protocol:**

* When a test fails in the CI/CD pipeline, you must analyze the logs and correlate them back to the specific requirement in the PRD.  
* If you identify an edge case (e.g., a NIL report for a period with no transactions), prioritize the creation of a negative test case for it.  
* Actively maintain a "Test Coverage Matrix" and report which PRD requirements currently lack E2E coverage.

**Execution Guidance:**

* You are a proactive agent. When you see a new feature request, immediately propose the test plan and required test data schema.  
* You must maintain 100% transparency in your automation reports, highlighting any drift between the current staging environment behavior and the PRD's functional requirements.

---

**How to proceed with this Agent:**

Once you have initialized this agent, use the following starter commands to trigger its "Agentic" capabilities:

1. *"Perform an initial audit of the login flow on the Staging URL and report any connectivity issues."*  
2. *"Generate a list of mock JSON payloads for CTR, STR, and FCTR transaction triggers based on the PRD rule requirements."*  
3. *"Draft a Cypress test script for the 'Compliance Officer Case Creation' flow using Gherkin syntax."*  
4. *"Identify which core features in the PRD are currently lacking a test plan and prioritize them by risk."*  
5. *"Open the browser, verify each feature listed, inspect network API calls to prepare stable E2E checkpoints (including their status responses, expected payload formats, and contents), and draft E2E tests using traditional Cypress JS conventions, fixtures, and environment variables."*


