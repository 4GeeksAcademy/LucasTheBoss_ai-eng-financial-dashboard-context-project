# Engineering Practices Audit

## Scope

This audit reviews the current repository implementation across architecture, typing, testing, developer experience, and documentation alignment. Findings are based on source files, tests, and runtime configuration present in the repository.

## Good Practices

### 1. Clear typed contracts on both backend and frontend

- Evidence: `backend/app/routes.py` defines Pydantic response models and `Literal`-based domain types.
- Evidence: `frontend/src/lib/financial-types.ts` defines matching frontend domain interfaces.
- Why it helps: this lowers ambiguity in the API contract and keeps dashboard calculations explicit.

### 2. Deterministic backend fixture generation for tests and demos

- Evidence: `generate_mock_movements(seed=42)` is used consistently across route handlers and tests.
- Evidence: `backend/tests/test_routes.py` relies on deterministic outputs for date and filter assertions.
- Why it helps: seeded data makes endpoint behavior reproducible enough for tests and UI demos.

### 3. Business logic is partially extracted from presentation code

- Evidence: `frontend/src/lib/financial-utils.ts` owns KPI and monthly aggregation calculations.
- Evidence: `frontend/src/components/dashboard/*` remain mostly presentational.
- Why it helps: dashboard rendering is easier to reason about and test because calculations are not buried in components.

### 4. Both application layers already have automated tests

- Evidence: `backend/tests/test_routes.py` covers endpoint behavior and filtering logic.
- Evidence: `frontend/src/lib/financial-utils.test.ts` covers KPI calculation, chronological aggregation, and formatting.
- Why it helps: the repository already has a base safety net for future maintenance.

### 5. Local development wiring is straightforward

- Evidence: `docker-compose.yml` runs frontend and backend together.
- Evidence: `frontend/vite.config.ts` proxies `/api` to the backend service.
- Why it helps: developers can run the full stack without custom networking setup.

### 6. UI states account for loading and fetch failure

- Evidence: `frontend/src/App.tsx` tracks `loading` and `error`.
- Evidence: dashboard cards and charts render skeletons or empty-state messaging.
- Why it helps: the frontend degrades more safely than a purely happy-path implementation.

## Risky Or Weak Practices

### 1. Backend route module is overloaded with too many responsibilities

- Evidence: `backend/app/routes.py` is 391 lines and contains domain types, Pydantic models, fixture generation, filtering, aggregation, anomaly detection, and HTTP handlers.
- Impact: this concentrates change risk in a single file and makes future feature work harder to review or test in isolation.
- Rule direction: split route handlers from domain models and financial computation helpers when adding new backend behavior.

### 2. Analytics are duplicated across backend and frontend instead of having a single source of truth

- Evidence: the backend exposes `/api/metrics/summary`, `/comparison`, `/alerts`, and `/categories/top`.
- Evidence: the frontend still fetches only `/api/metrics` in `frontend/src/App.tsx` and recomputes its own KPI/chart aggregates in `frontend/src/lib/financial-utils.ts`.
- Impact: backend and frontend can drift on period logic, formatting assumptions, or business definitions.
- Rule direction: when a metric exists as backend business logic, prefer consuming that backend contract instead of re-deriving it independently in the UI.

### 3. Visible date labeling can drift from actual data range

- Evidence: `frontend/src/App.tsx` passes `period="2024 - Full Year"` to the header.
- Evidence: `frontend/src/components/dashboard/dashboard-header.tsx` defaults to `2024 — Full Year`.
- Evidence: backend data generation in `backend/app/routes.py` uses `date.today()` to build a rolling 12-month range.
- Impact: the UI can communicate an incorrect reporting window even when the charts are otherwise functioning.
- Rule direction: derive visible date ranges from data or API metadata rather than hard-coding period labels.

### 4. Global random state is mutated inside shared backend helpers

- Evidence: `generate_mock_movements` calls `random.seed(seed)` directly.
- Impact: this creates hidden coupling with any future logic that also relies on Python's global random generator.
- Rule direction: use a local `random.Random(seed)` instance for fixture generation instead of mutating global RNG state.

### 5. CORS is fully open while credentials are allowed

- Evidence: `backend/app/main.py` sets `allow_origins=["*"]` and `allow_credentials=True`.
- Impact: this is too permissive for any environment beyond local training use and makes promotion to shared environments risky.
- Rule direction: keep permissive CORS limited to explicit local-development defaults and require environment-based origin configuration for broader use.

### 6. Dead sample-data module can mislead contributors

- Evidence: `frontend/src/lib/mock-data.ts` defines a static dataset.
- Evidence: repository search shows no imports of that module from the active frontend.
- Impact: unused datasets become stale and confuse developers about the real data source.
- Rule direction: remove or clearly quarantine demo-only data modules when the application has already switched to live API-backed fetching.

### 7. Dependency reproducibility is uneven across stacks

- Evidence: the frontend has `package-lock.json`, but backend dependencies in `backend/requirements.txt` are unpinned.
- Impact: Python environments can drift across installs, making debugging and review less repeatable.
- Rule direction: pin or constrain backend dependency versions once package behavior becomes part of the maintained workflow.

## Draft Rule Categories For Phase 3

### Backend architecture

- Keep route files focused on HTTP concerns.
- Move reusable financial calculations and fixture generation helpers into dedicated modules when logic grows.
- Avoid duplicating near-identical endpoint flows when a parameterized filter can express the same behavior.

### Data and contract governance

- Make one layer authoritative for business metric definitions.
- Derive UI date labels and ranges from backend data or endpoint metadata.
- Preserve typed contracts across Python and TypeScript when introducing new fields.

### Testing and change safety

- Add or update backend tests whenever endpoint filters, grouping, or comparison logic changes.
- Add or update frontend utility tests whenever KPI or chart aggregation logic changes.
- Treat dead code and stale fixtures as maintenance risks, not harmless leftovers.

### Environment and delivery

- Keep local dev defaults simple, but isolate them from assumptions that would be unsafe in shared environments.
- Prefer reproducible dependency definitions across both stacks.

## Phase 3 Input Summary

The strongest rule candidates are:

1. Keep backend financial logic out of route modules once behavior expands.
2. Do not hard-code reporting periods in the UI.
3. Do not duplicate business-metric definitions across backend and frontend without a documented reason.
4. Keep test updates mandatory for metric or filter behavior changes.
5. Remove or clearly isolate unused sample-data paths.
6. Tighten environment-sensitive defaults such as CORS and dependency pinning.