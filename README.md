# Agentic QA and Platform Automation Scheme

This repository serves as a centralized hub housing the Product Requirements Documents (PRDs), test plans, and automation specifications for all platforms under the Opex suite (including RegTech, GovTech, AgriTech, and corporate web platforms).

The core objective of this scheme is to employ **AI agents to perform QA tasks**, automating Behavior-Driven Development (BDD) workflows and End-to-End (E2E) testing using Cypress.

---

## Strategic Pillars of the Scheme

The automation scheme is built on three key operational principles:

### 1. Platform-Specific PRD & Test Plan Aggregation
Each platform under Opex has a dedicated PRD and corresponding QA test specification defined within this scheme. This includes:
*   **RegTech Platforms:** RegTech365, RegLearn (LMS), RegWatch (AI compliance monitoring), and REGPORT (AML/CFT transaction screening).
*   **AgriTech Platforms:** PlateauAgric (Agent/HQ control portals) and PLACOM (agricultural supply chain logistics).
*   **GovTech Platforms:** IMO OHURU (public services).
*   **Corporate Portals:** Opex Landing Page.

### 2. AI Agent-Driven QA Execution
Rather than relying solely on manual engineering, AI testing agents are integrated into the QA process. These agents ingest the platform-specific PRDs and specifications to:
*   Perform E2E browser verification.
*   Intercept API traffic to define robust network checkpoints.
*   Autonomously construct and maintain the Cypress test suite.

### 3. Gherkin as the Business Translation Layer
Gherkin syntax (`Feature`, `Scenario`, `Given`, `When`, `Then`) is used as the standard translation layer. It acts as the bridge between business requirements and technical execution:
*   Provides stakeholders with a clear, non-technical translation of the automation scheme.
*   Enables AI testing agents to parse business scenarios programmatically and map them directly to automated Cypress test scripts.

---

## Unified Testing Philosophy & Strategy

Across all platforms in this repository, a consistent, modern testing philosophy is enforced. This ensures automation is stable, high-value, and respects environment boundaries:

### 1. Risk-Based Environment Execution Policy
- **Staging / QA (Primary Sandbox):** Used for running all regression suites, boundary and validation testing, API interceptions, sync tests, data mutation actions, and stress testing.
- **Production (Sanity Check Only):** Restricts automated runs to non-destructive, lightweight smoke and health checks. Data modifications (e.g., registration, redemption, or invoicing) are strictly prohibited on live databases without explicit, isolated synthetic test profiles.

### 2. Automation Candidates & Selection Rules
We do not automate for the sake of high coverage numbers alone. The criteria for automation are:
- **Automation Priority:** Repetitive, deterministic, high-impact workflows (such as authentication, multi-step CRUD actions, file parses, and permission boundaries).
- **Manual Priority:** Exploratory testing, low-connectivity offline scenarios, physical hardware features (camera/GPS permissions), visual quality checks, and SMS delivery validation.

### 3. Agentic & Visual Verification Capabilities
AI-driven testing agents are equipped to handle advanced test boundaries beyond simple assertion paths:
- **Visual Regression:** Pixel comparison to monitor dashboard dynamic charts, layout alignment, and report representations.
- **Behavioral Simulation:** Emulating realistic human interaction for media controllers (such as video player playback speed and checkpoints) and spatial components (maps and forms).
- **Data Integrity & Reconciliation:** Checking backend integrity, webhook callbacks, transaction statuses, and API response schemas after bulk batch procedures.

### 4. Zero Hardcoded Secrets & Token Isolation
- Under no circumstances are secrets (passwords, JWTs, API tokens, personal identifying information like NINs or emails) hardcoded in tests, committed to repositories, or logged in execution outputs.
- Configuration properties must be loaded via secure environment variable mappings (e.g., `.env` file template configurations).

---

## Directory Structure

```text
all_platforms_automation_scheme/
├── README.md                              # This document (Overview of the automation strategy)
├── PLATFORMS SCHEDULED FOR AUTOMATION.md  # Unified registry of URLs, environments, and access credentials
├── PRDS/                                  # Source Product Requirements Documents (PRDs) per platform
│   └── RegTech365/
│       └── (PRD)RegTech365_Learning.md    # Detailed requirements for the LMS compliance platform
└── TEST_PLANS/                            # QA automation specifications and system instructions for AI agents
    ├── IMO_Ohuru_Combined_V1_V2_Functional_Agentic_QA_Specification.md
    ├── PLACOM_Combined_V1_V2_Functional_Agentic_QA_Specification.md
    ├── Plateau_Agric_HQ_Functional_Agentic_QA_Specification.md
    ├── Plateau_Agric_V1_Functional_Agentic_QA_Specification.md
    ├── REG PORT - AUTO.md
    ├── RegComply_Document_Task_Management_QA_Specification_Revised.md
    └── RegTech365_Learning_QA_Specification.md
```

### Detailed Test Plans Directory

Below are the active QA automation specifications and system instructions for testing agents, mapped to their respective files:

*   **[IMO_Ohuru_Combined_V1_V2_Functional_Agentic_QA_Specification.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/IMO_Ohuru_Combined_V1_V2_Functional_Agentic_QA_Specification.md):** Combined QA specification for IMO OHURU V1 and V2 e-governance client portals.
*   **[PLACOM_Combined_V1_V2_Functional_Agentic_QA_Specification.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/PLACOM_Combined_V1_V2_Functional_Agentic_QA_Specification.md):** Combined QA specification for PLACOM V1 and V2 agricultural supply chain logistics.
*   **[Plateau_Agric_HQ_Functional_Agentic_QA_Specification.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/Plateau_Agric_HQ_Functional_Agentic_QA_Specification.md):** QA specification for PlateauAgric HQ Control portal.
*   **[Plateau_Agric_V1_Functional_Agentic_QA_Specification.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/Plateau_Agric_V1_Functional_Agentic_QA_Specification.md):** QA specification for PlateauAgric V1 client and agent portals.
*   **[REG PORT - AUTO.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/REG%20PORT%20-%20AUTO.md):** E2E test plan and execution strategy for REGPORT AML/CFT transaction screening.
*   **[RegComply_Document_Task_Management_QA_Specification_Revised.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/RegComply_Document_Task_Management_QA_Specification_Revised.md):** QA specification for the RegComply Document & Task Management platform.
*   **[RegTech365_Learning_QA_Specification.md](file:///c:/Users/Ayooluwa/Documents/Opex/all_platforms_automation_scheme/TEST_PLANS/RegTech365_Learning_QA_Specification.md):** QA specification for the RegLearn Learning LMS Module.

---

## Standard Automation Workflow for AI Agents

When executing QA tasks, AI agents follow this lifecycle:

1.  **Ingestion & Audit:** Read the platform's PRD and authenticate through the designated environment URLs using dynamically loaded secrets.
2.  **Scenario Translation:** Parse the Gherkin-syntax scenarios to identify the business flow and expected outcomes.
3.  **Network Mapping:** Run browser-level interaction to intercept API calls, capturing HTTP methods, status codes, and request/response payload schemas.
4.  **Cypress Implementation:** Implement the tests using traditional Cypress JS patterns, leveraging mock fixtures and environment variables to ensure test isolation.
5.  **Drift Reporting:** Compare the live platform execution against the PRD requirements and output a Drift Report (reconciling implemented vs. specified features).

