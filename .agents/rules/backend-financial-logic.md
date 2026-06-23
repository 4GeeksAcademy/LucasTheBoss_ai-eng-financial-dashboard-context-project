# Backend Financial Logic

## Scope

Apply this rule when changing files in `backend/app/` that affect metrics, filters, fixture generation, or analytical endpoints.

## Rationale

`backend/app/routes.py` currently carries HTTP handlers, domain models, fixture generation, filtering, and analytical calculations in one place. That is workable at the current size, but new logic added casually to the same module will make reviews and tests harder.

## Required behavior

1. Keep route handlers focused on request parsing and response shaping.
2. Put reusable financial calculations and fixture-generation helpers in dedicated backend modules once logic expands beyond the current shape.
3. Do not add another near-duplicate endpoint when a parameterized filter can express the behavior.
4. Do not mutate Python's global random state for new fixture or simulation code. Use a local `random.Random(...)` instance instead.

## Repository examples

- `generate_mock_movements` and aggregation helpers are implementation logic that should stay reusable.
- `get_b2b_metrics` and `get_b2c_metrics` show how quickly duplication can appear around filtered variants.

## Review check

If a backend change adds new metric logic, a reviewer should be able to point to a helper module or clearly bounded function instead of reviewing business logic embedded directly inside a route body.