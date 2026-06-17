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

## Directory Structure

```text
all_platforms_automation_scheme/
├── README.md                              # This document (Overview of the automation strategy)
├── PLATFORMS SCHEDULED FOR AUTOMATION.md  # Unified registry of URLs, environments, and access credentials
├── PRDS/                                  # Source Product Requirements Documents (PRDs) per platform
│   └── RegTech365/
│       └── (PRD)RegTech365_Learning.md    # Detailed requirements for the LMS compliance platform
├── TEST_PLANS/                            # QA automation specifications and system instructions for AI agents
│   ├── REG PORT - AUTO.md                 # Test plan and execution strategy for REGPORT
│   └── RegTech365_Learning_QA_Spec.md     # Detailed E2E test plan for RegTech365 Learning
```

---

## Standard Automation Workflow for AI Agents

When executing QA tasks, AI agents follow this lifecycle:

1.  **Ingestion & Audit:** Read the platform's PRD and authenticate through the designated environment URLs using dynamically loaded secrets.
2.  **Scenario Translation:** Parse the Gherkin-syntax scenarios to identify the business flow and expected outcomes.
3.  **Network Mapping:** Run browser-level interaction to intercept API calls, capturing HTTP methods, status codes, and request/response payload schemas.
4.  **Cypress Implementation:** Implement the tests using traditional Cypress JS patterns, leveraging mock fixtures and environment variables to ensure test isolation.
5.  **Drift Reporting:** Compare the live platform execution against the PRD requirements and output a Drift Report (reconciling implemented vs. specified features).

