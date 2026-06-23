# Rule Validation

## Purpose

This document demonstrates that the repository rules are actionable against realistic tasks in this codebase.

## Scenario 1: Add a quarterly summary endpoint to the backend

- Governing rule: `backend-financial-logic.md`
- Expected guidance: add quarter-grouping logic in a reusable helper, keep the route thin, and avoid copying the existing summary route structure into another duplicate endpoint.
- Validation outcome: the rule gives a concrete implementation direction and a clear review standard.

## Scenario 2: Replace the dashboard's hard-coded year label with a real reporting range

- Governing rule: `frontend-metrics-and-labels.md`
- Expected guidance: derive the label from API data or backend metadata instead of shipping another fixed string.
- Validation outcome: the rule directly addresses a real inconsistency already present in the repository.

## Scenario 3: Change KPI logic to use backend comparison data

- Governing rule: `frontend-metrics-and-labels.md`
- Governing rule: `testing-for-metric-changes.md`
- Expected guidance: prefer backend-defined metric semantics and update frontend utility or integration tests to reflect the new contract.
- Validation outcome: the rule pair prevents silent metric-definition drift.

## Scenario 4: Tighten backend CORS for a shared preview environment

- Governing rule: `environment-and-dependency-safety.md`
- Expected guidance: replace wildcard behavior with explicit origin configuration and document the environment assumption.
- Validation outcome: the rule gives a practical path for hardening without sacrificing local clarity.

## Scenario 5: Remove the unused sample dataset module

- Governing rule: `frontend-metrics-and-labels.md`
- Expected guidance: delete or quarantine `frontend/src/lib/mock-data.ts` if it is not part of the active data path.
- Validation outcome: the rule is specific enough to guide cleanup work immediately.