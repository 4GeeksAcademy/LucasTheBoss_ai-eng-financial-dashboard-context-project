# Current Status

## Implemented behavior

### Backend

- Health check endpoint is implemented.
- Financial movement listing supports date, category, and operation-type filters.
- Analytical endpoints exist for facets, grouped summaries, top categories, period comparison, and alerts.
- Business-type-specific endpoints exist for B2B and B2C datasets.

### Frontend

- Dashboard data fetch from `/api/metrics` is implemented.
- KPI computation and monthly aggregation utilities are implemented and tested.
- KPI cards, charts, loading skeletons, and fetch error messaging are implemented.

## Known gaps

1. The frontend does not yet consume most analytical backend endpoints.
2. The dashboard period label is hard-coded and can diverge from the actual data window.
3. Backend route logic is concentrated in a single large module.
4. The backend currently uses permissive CORS defaults appropriate only for local or training-oriented use.
5. Unused frontend sample data remains in the repository and can confuse future contributors.
6. Backend dependencies are not pinned, which weakens reproducibility compared to the frontend stack.

## Immediate priorities

1. Decide whether the backend or frontend is the canonical source for displayed KPI definitions.
2. Remove date-label drift by deriving the visible reporting period from actual data.
3. Extract reusable backend financial logic away from the main route module as new behavior is added.
4. Either remove `frontend/src/lib/mock-data.ts` or explicitly document it as inactive demo-only code.
5. Improve environment hardening before treating the repository as more than a local training project.

## Maintenance notes

- The repository now includes `.agents/rules/` for contributor guidance.
- Phase-based documentation work has been committed separately to preserve evaluator-visible history.
- There is an unrelated local modification in `frontend/src/lib/financial-types.ts` that was intentionally excluded from these stewardship commits.