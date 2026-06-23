# Environment And Dependency Safety

## Scope

Apply this rule when changing Docker setup, dependency declarations, CORS behavior, or local development configuration.

## Rationale

The repository is designed for simple local startup, but some defaults are intentionally permissive. Those defaults should stay explicit and should not quietly become shared-environment assumptions.

## Required behavior

1. Keep local-development setup simple, but document any environment-sensitive behavior that would be unsafe outside local use.
2. Do not broaden cross-origin or credential behavior without explaining the deployment context.
3. Preserve reproducible installs across both stacks; if one stack uses lockfiles or version pinning, avoid letting the other drift without discussion.
4. When runtime instructions mention an environment file or startup path, ensure the referenced artifact actually exists in the repository.

## Repository examples

- `docker-compose.yml` and `frontend/vite.config.ts` make local full-stack startup straightforward.
- `backend/app/main.py` currently enables permissive CORS with credentials for local convenience.
- `frontend/package-lock.json` exists, while `backend/requirements.txt` is still unpinned.

## Review check

Infrastructure changes should leave a future maintainer with a reproducible local path and no hidden assumption that a dev-only setting is production-safe.