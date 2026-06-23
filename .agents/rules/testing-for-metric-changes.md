# Testing For Metric Changes

## Scope

Apply this rule whenever a change affects API filters, grouping, comparisons, alerts, KPI calculations, chart aggregation, or formatting.

## Rationale

This repository already has meaningful test coverage in `backend/tests/test_routes.py` and `frontend/src/lib/financial-utils.test.ts`. Those tests are part of the maintenance contract and must evolve with metric behavior.

## Required behavior

1. Backend metric or filter changes must update or add pytest coverage in `backend/tests/`.
2. Frontend KPI, aggregation, or formatting changes must update or add Vitest coverage near the affected utility.
3. Changes that alter reporting periods, category semantics, or business-type behavior must add at least one assertion covering the new edge case.
4. If the local shell lacks dependencies, validate through the documented runtime path and state the limitation explicitly in the artifact or review notes.

## Repository examples

- `backend/tests/test_routes.py` covers filters, summary, top categories, comparisons, and alerts.
- `frontend/src/lib/financial-utils.test.ts` covers chronological aggregation across years and formatting behavior.

## Review check

A metric change is incomplete if a reviewer cannot find the matching test update that proves the behavior now expected in this repository.