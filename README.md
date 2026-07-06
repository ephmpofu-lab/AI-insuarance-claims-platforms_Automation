# Governed AI Claims Platform

A modular AI-assisted insurance claims processing system built around controlled workflow orchestration, multimodal evidence analysis, human oversight and auditable decision paths.

The platform receives claim submissions, validates incoming data, processes documents and images, applies policy context and rules, supports claim decisions, generates reports and routes outcomes through controlled notification paths.

The system is built as a set of specialised workflows coordinated by a central claims orchestrator.

---

## The Problem

Insurance claims processing combines different forms of evidence, business rules and operational decisions.

A claim may include:

- structured claim data;
- policy information;
- PDF documents;
- photographs of damage;
- validation requirements;
- fraud indicators;
- human review decisions.

Placing all of these responsibilities inside one large workflow makes failures difficult to isolate and decisions difficult to trace.

This project separates the claims lifecycle into specialised workflows with defined responsibilities.

---

## Architecture

The platform is organised into four functional layers.

### 1. Ingestion and Validation

The system receives the claim submission, creates the initial claim record, uploads supporting files and validates required information.

Invalid or incomplete submissions follow a dedicated error path and create an event record.

### 2. Evidence Intelligence

Documents and images are processed through separate AI workflows.

**Document Intelligence**

- extracts text from claim documents;
- analyses document content;
- returns structured findings;
- stores the analysis against the claim record.

**Image Intelligence**

- performs an initial image safety and suitability check;
- routes accepted images for damage analysis;
- returns structured damage findings;
- records rejected or non-compliant inputs.

The outputs remain separate until they are combined at claim level.

### 3. Decision Orchestration

The decision layer brings together:

- claim information;
- document findings;
- image findings;
- policy information;
- policy rules.

The AI decision-support workflow uses this context to produce a structured recommendation and claim report.

Claims can then be routed to:

- approval;
- human review;
- fraud investigation.

AI output therefore supports the decision process without removing escalation and review paths.

### 4. Output and Notification

The final stage generates the claim report, records the decision path, updates claim status and sends the appropriate notification.

Different outcome routes are handled separately so that communication follows the actual claim decision.

---

## System Flow

```text
Claim Submission
        │
        ▼
Ingestion & Validation
        │
        ├── Validation Failure
        │           │
        │           ▼
        │    Error Handling
        │           │
        │           ▼
        │      Audit Event
        │
        ▼
Evidence Processing
        │
        ├── Document Intelligence
        │
        └── Image Safety Review
                    │
                    ▼
             Damage Analysis
        │
        ▼
Evidence Consolidation
        │
        ▼
Policy & Rules Context
        │
        ▼
AI Decision Support
        │
        ▼
Structured Claim Report
        │
        ├── Approved
        │
        ├── Human Review
        │
        └── Fraud Investigation
        │
        ▼
Notification
        │
        ▼
Status Update + Audit Event
```

---

## Governance by Design

Governance controls are integrated into the workflow architecture rather than added as a separate final step.

### Traceability

Key processing events, validation outcomes and status changes are recorded throughout the claim lifecycle.

### Human Oversight

Claims requiring additional judgement can be routed to human review rather than being automatically resolved.

### Structured AI Outputs

AI analysis workflows use defined output structures so that downstream workflows receive predictable fields rather than uncontrolled free-text responses.

### Separation of Responsibilities

Validation, document analysis, image analysis, decision support, reporting and notification are handled by separate workflows.

This allows individual components to be tested, changed and monitored independently.

### Error Visibility

Validation failures and image-policy violations follow explicit processing paths and create event records.

Failures are handled as system events rather than silently disappearing from the workflow.

### Evidence-Based Decision Support

Decision support is based on claim information, analysed evidence, policy information and defined rules.

---

## Workflow Structure

```text
Main Claims Orchestrator
│
├── Claim Ingestion
│
├── Binary File Upload
│
├── Input Validation
│   │
│   └── Validation Error Handler
│
├── Evidence Intelligence
│   │
│   ├── Document Intelligence
│   │
│   └── Image Processing
│       ├── Image Safety Review
│       └── Image Damage Analysis
│
├── Decision Support
│   ├── Policy Context
│   ├── Policy Rules
│   ├── Evidence Consolidation
│   └── AI Decision Support
│
├── Report Generation
│
└── Outcome Notification
    ├── Approval
    ├── Human Review
    └── Fraud Investigation
```

---

## Workflow Files

The workflow exports are organised in execution order:

```text
workflows/
│
├── 01_main_claims_orchestrator.json
├── 02_upload_binary_files.json
├── 03_claim_validation.json
├── 04_validation_error_handler.json
├── 05_document_intelligence.json
├── 06_image_safety_review.json
├── 07_image_damage_analysis.json
├── 08_claim_decision_support.json
├── 09_claim_report_generation.json
└── 10_claim_outcome_notification.json
```

---

## Technology

- **n8n** — workflow orchestration
- **OpenAI models** — document, image and decision-support tasks
- **Supabase** — operational data, evidence records and event logging
- **Structured Output Parsers** — controlled AI response schemas
- **Webhooks and HTTP services** — system integration
- **PDF generation** — claim reporting
- **Gmail integration** — outcome notification

---

## Design Principles

The platform follows five core design principles.

### 1. Modularity

Each workflow owns a defined responsibility.

### 2. Traceability

Important processing events produce persistent records.

### 3. Human Oversight

AI supports the claims process while maintaining escalation and review paths.

### 4. Fail Visibility

Invalid inputs and policy violations follow explicit error routes.

### 5. Controlled Orchestration

Specialised workflows communicate through defined processing stages rather than one large workflow handling the complete claims lifecycle.

---

## Repository Structure

```text
governed-ai-claims-platform/
│
├── workflows/
│   ├── 01_main_claims_orchestrator.json
│   ├── 02_upload_binary_files.json
│   ├── 03_claim_validation.json
│   ├── 04_validation_error_handler.json
│   ├── 05_document_intelligence.json
│   ├── 06_image_safety_review.json
│   ├── 07_image_damage_analysis.json
│   ├── 08_claim_decision_support.json
│   ├── 09_claim_report_generation.json
│   └── 10_claim_outcome_notification.json
│
├── images/
│   ├── system-architecture.png
│   └── workflow-previews/
│
└── README.md
```

---

## Project Status

Portfolio architecture project demonstrating the design and implementation of a modular AI-assisted insurance claims processing system with integrated validation, multimodal evidence analysis, decision support, human review paths and audit logging.

Workflow exports are provided without credentials, API keys or production data.
