# Project Summary

## Purpose

This repository is a two-service financial dashboard starter composed of a FastAPI backend and a React + TypeScript frontend. The current implementation focuses on displaying derived financial metrics from deterministic mock data rather than persisting real business records.

## Architecture

- `backend/` exposes HTTP endpoints for financial movements, filters, grouped summaries, top categories, period comparisons, and anomaly-style outcome alerts.
- `frontend/` fetches financial movement data from the backend and computes dashboard KPIs plus monthly chart series on the client.
- `docker-compose.yml` wires both services together for local development. The frontend Vite dev server proxies `/api` requests to the backend container.

## Backend Behavior

The API is assembled in `backend/app/main.py` with permissive CORS and a single router from `backend/app/routes.py`.

The backend does not read from a database. Instead, it generates seeded mock movements with these characteristics:

- 12 months of data
- 30 movements per month
- operation types: `income`, `outcome`
- categories: `suppliers`, `sales`, `operational`, `administrative`, `others`
- business types: `B2B`, `B2C`

Available endpoints:

- `GET /health`
- `GET /api/metrics`
- `GET /api/metrics/facets`
- `GET /api/metrics/summary`
- `GET /api/metrics/categories/top`
- `GET /api/metrics/comparison`
- `GET /api/metrics/alerts`
- `GET /api/metrics/b2b`
- `GET /api/metrics/b2c`

Filtering is supported by date range, category, operation type, and, on selected endpoints, business type.

## Frontend Behavior

The frontend entry point in `frontend/src/App.tsx` requests `GET /api/metrics`, then derives presentation data in `frontend/src/lib/financial-utils.ts`.

The UI currently renders:

- a dashboard header
- four KPI cards: total income, total outcome, profit, and profit margin
- an income versus outcome line chart
- a profit margin percentage line chart
- loading skeletons and an error banner when the fetch fails

The calculation split is important:

- backend: generates and filters raw movement records plus analytical endpoints
- frontend: computes the main dashboard KPIs and monthly chart aggregations from raw movement data

## Verified Implementation Notes

- Backend tests in `backend/tests/test_routes.py` cover the health endpoint, filters, B2B/B2C splits, facets, summary, top categories, comparison, and alerts.
- Frontend tests in `frontend/src/lib/financial-utils.test.ts` cover KPI calculation, chronological monthly aggregation, and value formatting.
- The current shell environment does not have backend or frontend dependencies installed, so direct test execution in this workspace failed due missing `fastapi` and `vitest`. The code-level behavior described above is still directly verifiable from source and test files.

## Current Product Boundaries

- No persistence layer or external API integration is present.
- The frontend currently consumes only `/api/metrics`; it does not yet use backend facets, comparison, alerts, summary, or top-category endpoints.
- The dashboard header presents a fixed annual label, while backend data generation is relative to the current date, so the UI label should not be interpreted as authoritative source-of-truth for the actual date range.

## Local Runtime Model

- Frontend: Vite on port `5173`
- Backend: FastAPI/Uvicorn on port `8000`
- API docs: `/docs`
- Python debug port exposed: `5678`

## Summary Assessment

This repository is already a working full-stack training project with a broader analytical backend than the frontend currently exposes. The most important maintenance reality is that visible dashboard metrics are client-derived from raw mock data, while the backend already contains additional analytical capabilities that are only exercised by tests today.