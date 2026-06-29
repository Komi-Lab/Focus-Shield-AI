# Focus-Shield-AI
From document chaos to clarity.

## Overview
AI-powered invoice processing system built with UiPath Maestro.  
Orchestrates AI agents, robots, and humans to automate end-to-end invoice workflows.

## Files
| File | Description |
|------|-------------|
| AI Invoice Processor.uis | Maestro BPMN process for invoice automation |
| Invoice Monitor.uiapp | Action Center screen for monitoring |
| InvoiceDispatcher.zip | Dispatcher workflow |
| InvoiceReviewApp.uiapp | UiPath Apps review screen |
| invoiceprocessing_ixp-...json | IXP taxonomy for document extraction |
| Submission deck_FocusShieldAI.pdf | Project presentation deck |

## Project Overview

Focus-Shield-AI solves the problem of **manual, error-prone invoice processing** that burdens finance teams across organizations. Invoices arrive via email in various formats (PDF, scanned images, etc.), are manually keyed into systems, reviewed by humans, and tracked through spreadsheets — a process that is slow, inconsistent, and costly.

This solution automates the entire invoice lifecycle end-to-end:
- **Ingestion** — Monitors email inboxes and captures incoming invoices automatically
- **Extraction** — Uses AI (IXP + LLM) to extract structured data from unstructured documents
- **Validation** — AI validates extracted fields against business rules
- **Human Review** — Routes exceptions to human reviewers via Action Center
- **Record Management** — Stores processed results in DataFabric for downstream use

---

## UiPath Components

| Component | Role |
|---|---|
| **UiPath Maestro** | BPMN-based orchestration of the entire invoice workflow |
| **UiPath IXP (Intelligent Document Processing)** | AI-powered extraction of invoice fields using pre-built taxonomy |
| **UiPath Apps** | Low-code UI for invoice monitoring (`Invoice Monitor`) and human review (`InvoiceReviewApp`) |
| **Action Center** | Human-in-the-loop task routing for exception handling and approvals |
| **UiPath DataFabric** | Centralized record storage and PDF document management |
| **UiPath Studio** | Development of Dispatcher and Performer automation workflows |
| **UiPath Orchestrator** | Queue management, scheduling, and robot coordination |
| **LLM Extraction API** | Large language model-based field extraction and internal AI validation |
| **UiPath Integration Service** | Email monitoring trigger via Microsoft 365 Mail connector; detects subject keywords and moves emails to the Invoice folder |
| **Microsoft 365 Mail (Exchange)** | Email service connected through Integration Service for invoice ingestion |
| **UiPath AutoPilot** | AI-assisted development used to accelerate workflow, expression, and activity creation across Studio, Maestro, and Apps during build |


---

## Agent Type

This solution uses **Low-Code Agents**.

- The core orchestration is built in **UiPath Maestro** (BPMN visual designer) — no custom coding required for the main workflow
- Document extraction leverages **UiPath IXP** with a configured taxonomy (JSON-based, no-code setup)
- The human review and monitoring UIs are built using **UiPath Apps** (drag-and-drop low-code builder)
- The Dispatcher workflow is developed in **UiPath Studio** using low-code activity sequences

> No coded agents (e.g., Python scripts, custom API integrations coded from scratch) are used in this solution.

---

## Setup Instructions

### Prerequisites
- UiPath Orchestrator (cloud or on-prem) with active license
- UiPath Studio installed (v2023.10 or later recommended)
- UiPath Apps enabled in your Orchestrator tenant
- Action Center enabled
- DataFabric enabled in your tenant
- Email account accessible via IMAP or Exchange

### Step 1 — Import the Maestro Process
1. Open UiPath Orchestrator
2. Navigate to **Automations > Processes**
3. Upload `AI Invoice Processor.uis` as a new Maestro process
4. Publish and activate the process

### Step 2 — Deploy the Dispatcher Workflow
1. Extract `InvoiceDispatcher.zip`
2. Open the project in UiPath Studio
3. Configure the email connection (IMAP/Exchange credentials) in the settings
4. Publish to Orchestrator and create a Trigger (e.g., schedule or email trigger)

### Step 3 — Import IXP Taxonomy
1. Go to **AI Center > Document Understanding** in Orchestrator
2. Import `invoiceprocessing_ixp-1b7d2e76-taxonomy.json` as a new taxonomy
3. Link the taxonomy to the Maestro process

### Step 4 — Deploy UiPath Apps
1. In Orchestrator, navigate to **Apps**
2. Import `Invoice Monitor.uiapp` → Publish
3. Import `InvoiceReviewApp.uiapp` → Publish
4. Share the apps with relevant users/roles

### Step 5 — Configure Action Center
1. Ensure Action Center is enabled for your tenant
2. Assign the reviewer role to appropriate users
3. Confirm the Maestro process is configured to route exceptions to Action Center

### Step 6 — Test the Solution

1. Extract `TestPDFs.zip` to get sample invoice PDFs.
2. Send a test email with one of the following subject lines to trigger the automation.

   - **To:** `The email address connected to your Integration Service`
   - **Subject (choose one):**
     - `【請求書送付】`
     - `Invoice Attached`
     - `Rechnung beigefügt`
   - Attach one or more invoice PDFs from `TestPDFs.zip` to the email.

3. The Integration Service (Microsoft 365 Mail trigger) detects the keyword in the subject line and moves the email to the **Invoice** folder automatically.
4. Verify the Dispatcher picks up the email from the Invoice folder and queues the item in Orchestrator.
5. Monitor the Maestro process execution in Orchestrator.
6. Check `Invoice Monitor` app for real-time status.
7. For exception cases, confirm tasks appear in Action Center for human review.
8. Verify final records are stored in DataFabric.


  
