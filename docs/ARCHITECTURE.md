# Architecture: Sovereign Executive BI

## Overview

**Package ID:** `PKG-012`  
**Domain:** Executive BI & In-Browser SQL  
**Microservice Port:** `8790`  
**n8n Webhook Path:** `executive-bi-trigger`  
**GitHub:** [BlackFoxgamingstudio/executive-bi](https://github.com/BlackFoxgamingstudio/executive-bi)

Real-time executive dashboard engine with DuckDB in-browser SQL, KPI aggregation, anomaly detection, and automated board-report generation.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign Executive BI        │
                     │       Port: 8790            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  DuckDBEngine    | KPIAggregator   | AnomalyDetec  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `DuckDBEngine`
Handles all duckdb operations. Exposes async methods callable from the core dispatcher.

### `KPIAggregator`
Handles all kpiaggregator operations. Exposes async methods callable from the core dispatcher.

### `AnomalyDetector`
Handles all anomalydetector operations. Exposes async methods callable from the core dispatcher.

### `DashboardRenderer`
Handles all dashboardrenderer operations. Exposes async methods callable from the core dispatcher.

### `ReportGenerator`
Handles all reportgenerator operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-executive-bi", "port": 8790}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-executive-bi:
  image: sovereign-executive-bi:latest
  ports: ["8790:8790"]
  healthcheck:
    test: curl -f http://localhost:8790/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`bi`, `duckdb`, `kpi`, `analytics`
