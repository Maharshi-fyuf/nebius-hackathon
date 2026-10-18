# SignalDesk AI

> **AI-powered Security Alert Triage & Investigation Assistant**\
> Evidence-first security investigation with NVIDIA Nemotron via Nebius
> Token Factory.

![Status](https://img.shields.io/badge/status-MVP%20in%20development-orange)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Frontend](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB)
![Backend](https://img.shields.io/badge/Backend-FastAPI-009688)
![AI](https://img.shields.io/badge/AI-NVIDIA%20Nemotron-76B900)

SignalDesk AI is a prototype designed to help security analysts review
suspicious events more efficiently. It combines deterministic checks
with a hosted NVIDIA Nemotron model to produce structured explanations,
highlight supporting evidence, and suggest investigation steps for a
human analyst to review.

**Important:** SignalDesk AI is a decision-support prototype, not a
replacement for a Security Operations Centre (SOC), a SIEM, or a
qualified security analyst. Findings are indicative and must be
verified.

## Contents

-   [Problem](#problem)
-   [Solution](#solution)
-   [Core workflow](#core-workflow)
-   [Key features](#key-features)
-   [Architecture](#architecture)
-   [Technology stack](#technology-stack)
-   [Hackathon requirement: Nebius +
    NVIDIA](#hackathon-requirement-nebius--nvidia)
-   [Getting started](#getting-started)
-   [Environment configuration](#environment-configuration)
-   [Example investigation output](#example-investigation-output)
-   [Testing and evaluation](#testing-and-evaluation)
-   [Security and privacy](#security-and-privacy)
-   [Limitations](#limitations)
-   [Roadmap](#roadmap)
-   [Team](#team)
-   [License](#license)

## Problem

Security teams may receive many alerts and logs. Analysts must determine
which events deserve attention, what evidence supports a potential
incident, what information is missing, and what to investigate next.
Reviewing alerts manually can be time-consuming, while an unexplained AI
verdict is difficult to trust.

## Solution

SignalDesk AI aims to turn an alert into an **evidence-backed
investigation brief**. The application combines:

1.  **Deterministic checks** for known patterns and transparent risk
    factors.
2.  **NVIDIA Nemotron inference** through Nebius Token Factory to
    explain the event and suggest follow-up checks.
3.  **Output validation** to ensure the model response follows an
    expected structure.
4.  **Human review** so an analyst can accept, dismiss, or modify a
    finding.
5.  **Case history and reporting** to preserve the investigation record.

The model does not independently prove that an attack occurred. The app
should distinguish observed evidence from model-generated
interpretation.

## Core workflow

1.  Select a built-in synthetic alert or provide a supported sample
    event.
2.  Validate and normalize the input.
3.  Extract relevant fields such as timestamp, account, source IP, event
    type, and authentication outcome.
4.  Send the minimum relevant event context to an NVIDIA Nemotron model
    through Nebius Token Factory.
5.  Validate the structured response and compare it with deterministic
    risk rules.
6.  Display the potential incident type, severity, evidence,
    uncertainty, missing information, and recommended investigation
    steps.
7.  Save the case and record the analyst's review decision.
8.  Export an investigation summary.

## Key features

The following is the **planned MVP scope**. Only mark a feature as
complete in project materials after it has been implemented and tested.

-   **Alert Explorer:** Browse and filter sample security events.
-   **Evidence-first investigation:** Show the input records supporting
    each finding.
-   **AI-assisted analysis:** Generate a structured explanation and
    follow-up steps.
-   **Transparent risk factors:** Keep rule-based risk calculations
    separate from the model's opinion.
-   **Missing-information checks:** Identify context an analyst should
    verify before deciding.
-   **Human-in-the-loop review:** Record whether a finding is accepted,
    dismissed, or needs more investigation.
-   **Case history:** Save investigation results and review events.
-   **Investigation report:** Export a readable summary for review.
-   **Synthetic demo data:** Demonstrate the workflow without using real
    organizational security logs.

## Architecture

``` text
Analyst
  |
  v
React + Vite dashboard
  |
  | REST API
  v
FastAPI backend
  |
  +--> Input validation and normalization
  |
  +--> Deterministic risk rules
  |
  +--> Nebius Token Factory API
  |       |
  |       +--> NVIDIA Nemotron model
  |
  +--> Structured response validation
  |
  +--> SQLite case store and audit trail
  |
  v
Evidence-backed investigation result
```

### Design principles

-   **Evidence before conclusions:** Keep raw event evidence available
    alongside generated explanations.
-   **Human control:** Do not execute disruptive response actions
    automatically.
-   **Traceability:** Record the alert, model identifier, output, and
    analyst review where practical.
-   **Honest uncertainty:** Show missing context and avoid presenting
    model output as confirmed fact.
-   **Privacy by design:** Use synthetic data for demos and send only
    necessary data to the inference service.

## Technology stack

  -----------------------------------------------------------------------
  Component               Technology              Purpose
  ----------------------- ----------------------- -----------------------
  Frontend                React, Vite, CSS        Analyst dashboard

  Backend                 Python, FastAPI,        REST API and
                          Uvicorn                 application logic

  Inference               Nebius Token Factory    Hosted model inference

  Model                   NVIDIA Nemotron model   AI-assisted
                          available in the Token  investigation
                          Factory catalog         

  Validation              Pydantic                Validate structured
                                                  inputs and outputs

  Database                SQLite                  Local case history and
                                                  audit records

  Testing                 Pytest                  Unit and integration
                                                  tests

  Version control         Git and GitHub          Team collaboration and
                                                  source history
  -----------------------------------------------------------------------

Exact model IDs, endpoints, and package versions should be confirmed
against the current Nebius documentation and the account's available
model catalog.

## Hackathon requirement: Nebius + NVIDIA

This project is designed to demonstrate both required components in the
actual application workflow:

-   **Nebius:** The backend makes a real inference request to Nebius
    Token Factory.
-   **NVIDIA:** The request uses an eligible NVIDIA open model, such as
    a Nemotron model available in the Token Factory catalog.

A model name in the README or a screenshot alone is not sufficient.
Verify that the running app makes a successful request to the selected
model. Document the exact model ID used, keep API keys secret, and show
the live inference workflow in the demo.

Before submission, re-check the official hackathon rules for the precise
qualifying deployment/API conditions and the accepted NVIDIA model
requirements.

## Getting started

### Prerequisites

-   Python 3.10 or newer
-   Node.js 18 or newer and npm
-   A Nebius Token Factory account/API key with access to an eligible
    NVIDIA model
-   Git

### 1. Clone the repository

``` bash
git clone <YOUR_PUBLIC_REPOSITORY_URL>
cd signaldesk-ai
```

Replace `<YOUR_PUBLIC_REPOSITORY_URL>` with your team's actual
repository URL.

### 2. Set up the Python backend

From the project root:

``` bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**

``` powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

``` bat
.venv\Scripts\activate.bat
```

**macOS / Linux**

``` bash
source .venv/bin/activate
```

Install dependencies after `requirements.txt` has been created:

``` bash
pip install -r requirements.txt
```

### 3. Configure environment variables

Create a local `.env` file from `.env.example` (if supplied), or set the
variables in your shell. See [Environment
configuration](#environment-configuration).

Never commit `.env` or API keys to Git.

### 4. Start the backend

After the backend entry point has been implemented:

``` bash
uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000
```

The API should be available at `http://127.0.0.1:8000`. If FastAPI's
interactive documentation is enabled, visit
`http://127.0.0.1:8000/docs`.

### 5. Start the frontend

In a second terminal:

``` bash
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite, commonly `http://localhost:5173`.

> These instructions describe the intended project setup. They will work
> once the corresponding files, scripts, and routes exist in the
> repository. Update this section if the implementation uses different
> commands or paths.

## Environment configuration

Create a local `.env` file and configure values supported by your
implementation. Example:

``` dotenv
NEBIUS_API_KEY=replace_with_your_secret_key
NEBIUS_BASE_URL=https://api.tokenfactory.nebius.com/v1/
NEBIUS_MODEL_ID=replace_with_an_available_nvidia_nemotron_model
DATABASE_URL=sqlite:///./signaldesk.db
```

**Configuration notes**

-   Confirm the current Token Factory base URL and the model ID from
    official Nebius documentation or your account's model catalog before
    running the app.
-   Do not use a placeholder model ID in the final demo.
-   Do not expose the API key in frontend code, browser requests,
    screenshots, logs, Git history, or the public repository.
-   Prefer the current official SDK/API documentation when implementing
    the inference client.

## Example investigation output

The example below is illustrative and is **not a real model result**:

``` json
{
  "incident_type": "Suspicious authentication",
  "severity": "high",
  "confidence": "medium",
  "evidence": [
    "25 failed login attempts",
    "A subsequent successful login"
  ],
  "explanation": "This sequence may indicate credential guessing.",
  "missing_information": [
    "Whether the source IP is expected",
    "Whether multi-factor authentication was completed"
  ],
  "recommended_actions": [
    "Review authentication history",
    "Verify account-owner activity",
    "Check related events"
  ],
  "human_review_required": true
}
```

The application should validate the response schema, reject or safely
handle malformed output, and ensure that cited evidence exists in the
input. Severity and confidence labels should not be presented as
guarantees.

## Testing and evaluation

Create a labelled set of synthetic alerts with expected outcomes.
Include both suspicious and benign examples to measure false positives
as well as missed detections.

Suggested checks:

-   Input validation for missing or malformed fields.
-   Rule-engine tests for each known event pattern.
-   Model-output schema validation and malformed-response handling.
-   API timeout, rate-limit, and unavailable-model handling.
-   Evidence references must point to actual input data.
-   Analyst review and audit-trail persistence.
-   A small evaluation set reporting measured precision, recall, and F1
    where ground-truth labels are available.

Report actual measurements only after running the tests. Do not claim a
particular accuracy or detection rate without evidence.

## Security and privacy

-   Use synthetic alerts in public demos.
-   Do not upload real credentials, personal data, or confidential
    production logs.
-   Keep the inference API key in server-side environment variables or
    the hosting provider's secret manager.
-   Never execute model-generated code or commands automatically.
-   Do not automatically block IP addresses, disable accounts, or change
    production systems.
-   Treat all model output as untrusted and validate it before rendering
    or saving.
-   Use a human approval step for any future response action.
-   Make it clear when inference sends data to a hosted service; this
    project is **not air-gapped** when using Nebius Token Factory.

## Limitations

-   Model output may be incorrect, incomplete, or overconfident.
-   Synthetic examples do not represent the full diversity of real SOC
    traffic.
-   A suspicious pattern is not proof of compromise.
-   Without integrations to real telemetry systems, the prototype cannot
    verify an organization's full security posture.
-   The app is intended for analyst decision support and is not a
    production SOC or incident-response system.

## Roadmap

-   [ ] Complete the first working end-to-end alert investigation.
-   [ ] Integrate and verify a qualifying NVIDIA Nemotron model through
    Nebius Token Factory.
-   [ ] Add structured output validation and clear evidence references.
-   [ ] Implement the dashboard, case persistence, and audit trail.
-   [ ] Add a labelled synthetic evaluation set and report measured
    results.
-   [ ] Deploy a judge-accessible demo and test it from a clean session.
-   [ ] Record a short demo showing the live inference call and the
    complete workflow.

## Team

Add the names and roles of your team members here.

-   **AI / Nebius integration:** `Name`
-   **Backend / analytics:** `Name`
-   **Frontend / UI:** `Name`
-   **Testing / deployment / presentation:** `Name`

## License

Choose and add an OSI-approved open-source license before publishing the
repository (for example, MIT or Apache-2.0), subject to your team's
requirements and the hackathon rules. Do not claim a license until the
corresponding `LICENSE` file is included.
