# SignalDesk AI

**An evidence-verified security investigation agent, powered by NVIDIA Nemotron on Nebius Token Factory.**

Built for the [Nebius x NVIDIA Global AI Hackathon](https://nebiusglobalaihackathon.devpost.com/) · Track: **Apps & Agents**

![Status](https://img.shields.io/badge/status-hackathon%20build-orange)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Backend](https://img.shields.io/badge/Backend-FastAPI-009688)
![Frontend](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB)
![Model](https://img.shields.io/badge/Model-NVIDIA%20Nemotron-76B900)
![Inference](https://img.shields.io/badge/Inference-Nebius%20Token%20Factory-4B5CFF)

> 🎥 **Demo video (under 3 min):** `TODO: add YouTube link`
> 🖼️ **Screenshot / GIF:** `TODO: add docs/demo.gif`

---

## The idea in one paragraph

Security analysts don't need another AI that says "this looks suspicious." They need an investigation they can trust. SignalDesk AI takes a security alert and **investigates it like an analyst would**: it plans, queries logs, correlates events, looks up threat intelligence on the live web, and writes a brief. Then comes the key step: **every claim in the brief must cite a real log event, and code verifies that those events exist.** Claims without evidence are flagged or rejected before the analyst sees them.

> **SignalDesk AI is decision support, not a SOC replacement.** It never takes response actions on its own. A human analyst accepts, dismisses, or escalates every finding.

## Why it's different

| Typical "AI alert triage" | SignalDesk AI |
|---|---|
| One prompt, one answer | Multi-step agent loop with tool calls |
| Model opinion shown as fact | Every claim must cite event IDs |
| Hallucinated evidence goes unnoticed | A deterministic verifier checks each citation against the logs |
| Trust the AI or don't | Rules-only vs. LLM vs. agent compared on a labelled benchmark |
| No external context | Live threat-intel lookup via Tavily |

## How it works

```text
Alert
  │
  ▼
FastAPI backend ──► Agent loop (Nemotron on Nebius Token Factory)
                      │
                      ├─ search_logs      query the synthetic log store
                      ├─ correlate_events link related events by account / IP / time
                      ├─ build_timeline   order events into an attack timeline
                      ├─ map_attack       map behaviour to MITRE ATT&CK techniques
                      └─ web_lookup       Tavily: IP reputation, CVEs, technique context
                      │
                      ▼
             Draft investigation brief (structured JSON)
                      │
                      ▼
           Citation verifier (deterministic code)
             ├─ every cited event ID exists?
             ├─ cited event actually supports the claim's fields?
             └─ unsupported claims → flagged / removed
                      │
                      ▼
      Analyst review: accept · dismiss · needs more investigation
                      │
                      ▼
          SQLite case history + audit trail + exported report
```

**Rule-based risk scoring runs separately** from the model, so the analyst always sees the deterministic risk factors next to the AI's interpretation.

## Features

Status key: ✅ built and tested · 🚧 in progress · ⬜ planned. Update this table honestly before you submit.

| Feature | Status |
|---|---|
| Synthetic log and alert dataset (attack scenarios + benign decoys) | ⬜ |
| Agent loop with tool calling on Nemotron via Nebius Token Factory | ⬜ |
| Tools: log search, correlation, timeline, ATT&CK mapping | ⬜ |
| Tavily web lookup for threat-intel enrichment | ⬜ |
| Citation verifier (reject or flag unsupported claims) | ⬜ |
| Separate rule-based risk factors | ⬜ |
| Analyst review workflow + audit trail | ⬜ |
| Investigation report export | ⬜ |
| Benchmark: rules vs. single LLM call vs. agent | ⬜ |

## Hackathon requirements

- **Runs on Nebius Token Factory:** the backend makes real inference calls to Nebius Token Factory for every investigation.
- **Uses an NVIDIA open model:** `TODO: exact Nemotron model ID from your Token Factory catalog`.
- **Tavily:** used for live threat-intelligence lookups inside the agent loop.
- **Open source:** released under the [MIT License](LICENSE). `TODO: add the LICENSE file`.

## Benchmark

We compare three approaches on the same labelled set of synthetic incidents:

| Approach | Precision | Recall | F1 | Unsupported claims caught | Avg. time per case |
|---|---|---|---|---|---|
| Rules only | TODO | TODO | TODO | n/a | TODO |
| Single LLM call | TODO | TODO | TODO | TODO | TODO |
| **SignalDesk agent + verifier** | TODO | TODO | TODO | TODO | TODO |

Fill this table only with numbers from runs you have actually performed. Include the dataset size and how to reproduce the results: `python -m eval.run_benchmark`. `TODO: confirm the path once implemented`.

## Example output

> Illustrative only, not a real model result. Replace with a real run before submission.

```json
{
  "incident_type": "Credential stuffing followed by account takeover",
  "severity": "high",
  "confidence": "medium",
  "findings": [
    {
      "claim": "25 failed logins from one IP in 4 minutes, then a successful login",
      "evidence": ["evt_0412", "evt_0413", "evt_0437"],
      "verified": true
    },
    {
      "claim": "Source IP is associated with known credential-stuffing activity",
      "evidence": ["web:tavily:result_2"],
      "verified": true
    }
  ],
  "attack_techniques": ["T1110.004 Credential Stuffing"],
  "missing_information": [
    "Was MFA completed on the successful login?",
    "Is this IP range expected for this user?"
  ],
  "recommended_actions": [
    "Review the account's session history",
    "Confirm activity with the account owner",
    "Check for related events on other accounts"
  ],
  "human_review_required": true
}
```

## Tech stack

| Layer | Technology |
|---|---|
| Inference | Nebius Token Factory (OpenAI-compatible API) |
| Model | NVIDIA Nemotron (`TODO: model ID`) |
| Web search | Tavily |
| Backend | Python, FastAPI, Pydantic |
| Frontend | React, Vite |
| Storage | SQLite |
| Tests | Pytest |

## Getting started

### Prerequisites

- Python 3.10+
- Node.js 18+ and npm
- A Nebius Token Factory API key with access to a Nemotron model
- A Tavily API key

### Setup

```bash
git clone TODO_YOUR_REPO_URL
cd signaldesk-ai

# Backend
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt

# Configuration
cp .env.example .env             # then fill in your keys
```

`.env`:

```dotenv
NEBIUS_API_KEY=your_key_here
NEBIUS_BASE_URL=https://api.tokenfactory.nebius.com/v1/
NEBIUS_MODEL_ID=your_nemotron_model_id
TAVILY_API_KEY=your_key_here
DATABASE_URL=sqlite:///./signaldesk.db
```

Never commit `.env`. API keys stay on the server and never appear in frontend code.

### Run

```bash
# Terminal 1: backend
uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000

# Terminal 2: frontend
cd frontend
npm install
npm run dev
```

Open the URL printed by Vite (usually `http://localhost:5173`).

## Safety and privacy

- Demos use **synthetic data only**. No real credentials, personal data, or production logs.
- Model output is treated as **untrusted**: it is schema-validated and citation-verified before display or storage.
- The agent has **read-only tools**. It never executes model-generated code or commands, and never blocks IPs, disables accounts, or changes systems.
- Any future response action would require explicit human approval.
- This project is **not air-gapped**: event context is sent to Nebius Token Factory, and lookup terms are sent to Tavily. Only the minimum necessary data is shared.

## Limitations

- Model output can be wrong, incomplete, or overconfident. The verifier checks that evidence exists, not that the model's reasoning is correct.
- Synthetic data doesn't capture the full variety of real SOC traffic.
- A suspicious pattern is not proof of compromise.
- There are no integrations with live telemetry (SIEM, EDR, cloud logs) yet.

## Roadmap

- Connectors for real log sources (SIEM exports, cloud audit logs)
- Analyst feedback loop to improve prompts and rules
- Additional tools (asset inventory, user-behaviour baselines)
- Approval-gated response actions

## Team

| Role | Name |
|---|---|
| Team lead / AI and agent loop | TODO |
| Backend and verifier | TODO |
| Frontend | TODO |
| Evaluation and demo | TODO |

## License

<<<<<<< HEAD
MIT, see [LICENSE](LICENSE). `TODO: add the LICENSE file before publishing`.
=======
MIT, see [LICENSE](LICENSE). `TODO: add the LICENSE file before publishing`.
>>>>>>> origin/main
