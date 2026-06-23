# Frontend Metrics And Labels

## Scope

Apply this rule when changing files in `frontend/src/` that fetch dashboard data, compute metrics, render charts, or display date ranges.

## Rationale

The frontend currently derives KPIs and monthly chart data from `/api/metrics`, while the backend already exposes analytical endpoints and date-aware summaries. The dashboard also shows a fixed year label even though backend data generation is relative to the current date.

## Required behavior

1. Do not hard-code reporting period labels when the date range can be derived from API data or metadata.
2. If a metric already exists as backend business logic, prefer consuming the backend contract instead of re-implementing the same definition in the UI.
3. Remove or clearly isolate demo-only sample data once a screen is API-backed.
4. Keep calculation helpers outside React components so they remain directly testable.

## Repository examples

- `frontend/src/App.tsx` currently fetches only `/api/metrics` and computes display metrics locally.
- `frontend/src/components/dashboard/dashboard-header.tsx` and `frontend/src/App.tsx` currently hard-code a 2024 full-year label.
- `frontend/src/lib/mock-data.ts` is not part of the active rendering path.

## Review check

When a frontend dashboard change is proposed, reviewers should be able to trace the displayed metric definition and date label back to a data source instead of a hard-coded assumption.