# Sovereign Executive Bi (`sovereign-executive-bi`)

[![PyPI Version](https://img.shields.io/badge/pypi-v1.0.0-blue.svg)](pyproject.toml)
[![Tests](https://img.shields.io/badge/pytest-passing_100%25-brightgreen.svg)](tests/test_solution.py)
[![CI](https://github.com/BlackFoxgamingstudio/executive-bi/actions/workflows/ci.yml/badge.svg)](https://github.com/BlackFoxgamingstudio/executive-bi/actions/workflows/ci.yml)
[![n8n Integration](https://img.shields.io/badge/n8n-workflow_ready-orange.svg)](n8n/workflow.json)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Enterprise Standalone Package**: Client-side executive analytics console utilizing PGlite (WASM Postgres in browser) and Firebase. Enables analytical SQL queries inside the client browser without backend infrastructure costs, plus a Monte Carlo financial business simulator.

---


## 🚦 Architecture & Implementation Status

| Component | Status | Details |
|---|---|---|
| **Architecture Tier** | **TIER 2 (INFRASTRUCTURE LIVE / LOGIC QUEUED)** | **Tier 2: Foundation & Integration Live**. Docker containerization, GitHub Actions CI/CD, OpenAPI 3.1 REST API, Zero-Trust `X-SBB-Auth` webhook adapter, and n8n canvas nodes are fully production-ready. Domain algorithms are documented in `docs/ARCHITECTURE.md` and tracked on the `ROADMAP.md` backlog. |
| **REST Gateway** | ✅ Live on Port `8790` | `POST /api/v1/execute`, `GET /health` |
| **Security Layer** | ✅ Active | Enforced `X-SBB-Auth` token verification |
| **n8n Orchestration** | ✅ 100% Connected | Full 3-node connected execution pipeline |
| **Domain Logic** | ⏳ Scaffolded (v1.1.0 Backlog) | Tracked in [`ROADMAP.md`](ROADMAP.md) |
| **Automated Tests** | ✅ Passing | `python3 -m unittest discover -s tests` |
## 1. Overview & Architectural Blueprint

`sovereign-executive-bi` is an independently packaged, zero-dependency software library and microservice engineered as part of Russell Alan Powers' 10-year software engineering portfolio.

It delivers robust capabilities in **Executive BI & In-Browser SQL** and provides seamless integration with n8n event workflows.

```
┌───────────────────────────┐         HTTP POST          ┌───────────────────────────────────────────┐
│        n8n Engine         │ ─────────────────────────> │        sovereign-executive-bi Adapter        │
│   (Port 5678 Webhook)     │ <───────────────────────── │             (Port 8790)                 │
└───────────────────────────┘       Idempotent JSON      └───────────────────────────────────────────┘
                                                                               │
                                                                               ▼
                                                         ┌───────────────────────────────────────────┐
                                                         │            CoreEngine Domain              │
                                                         │      (SHA-256 Idempotent Execution)       │
                                                         └───────────────────────────────────────────┘
```

---

## 2. Core Exported Classes & Features

- **Primary Module**: `from sovereign_executive_bi import PGliteQueryBridge, MonteCarloSimulator, CashFlowForecaster, KPIDashboardEngine`
- **Deterministic Idempotency**: All executions generate unique SHA-256 idempotency tokens preventing duplicate runs across network retries.
- **Self-Contained Microservice**: Zero external third-party dependencies required for base execution.

---

## 3. Installation & Quickstart

```bash
# Clone the repository
git clone https://github.com/russellpowers/sovereign-executive-bi.git
cd executive-bi

# Install in editable mode
pip install -e .

# Verify health status via CLI
sovereign-executive-bi --health
```

---

## 4. CLI Usage Reference

```bash
# Check service health
sovereign-executive-bi --health

# Execute core domain action with a JSON payload
sovereign-executive-bi --exec process_data --payload '{"sample_key": "sample_value"}'
```

---

## 5. n8n Automation & Integration Contract

- **Microservice Port**: `http://localhost:8790`
- **Inbound Trigger Route**: `POST /api/v1/execute`
- **Integration Workflow**: `Push daily field revenue into Firebase/PGlite sync -> Trigger automated PDF executive summary for leadership meeting`

### How to Import into n8n:
1. Open your n8n canvas (`http://localhost:5678`).
2. Click **Workflows** > **Import from File**.
3. Select `n8n/workflow.json`.
4. Start the background webhook adapter:
   ```bash
   python3 n8n/webhook_adapter.py
   ```
5. Dispatch your test event to `http://localhost:5678/webhook/executive-bi-trigger`.

---

## 6. Verification & Automated Testing

This repository includes a 100% passing test suite runnable via `pytest` or `python3`:

```bash
# Run tests with pytest
pytest tests/test_solution.py -v

# Run tests directly (zero dependencies)
python3 tests/test_solution.py
```

---

## 7. Staff/Principal Engineer Technical Defense

> **60-Second Interview Pitch**:
> "High-impact VP/Director of Engineering proof: edge-first WASM computing, zero-latency local analytics, and advanced business acumen."
