# Tech Stack

## Application stack

### Frontend

- React 19
- TypeScript
- Vite
- Tailwind CSS
- Recharts
- Vitest
- ESLint

### Backend

- Python 3.13 slim container image
- FastAPI
- Pydantic
- Uvicorn
- pytest
- debugpy

## Local runtime topology

- `docker-compose.yml` defines separate `frontend` and `backend` services.
- Frontend container exposes port `5173`.
- Backend container exposes port `8000` and debug port `5678`.
- `frontend/vite.config.ts` proxies `/api` traffic to `http://backend:8000` in local development.

## Dependency management state

- Frontend dependencies are defined in `frontend/package.json` and locked with `frontend/package-lock.json`.
- Backend dependencies are declared in `backend/requirements.txt` without pinned versions.

## Testing stack

- Backend behavior tests live under `backend/tests/`.
- Frontend utility tests live next to the utilities in `frontend/src/lib/`.
- The current workspace shell did not have installed Python or Node dependencies at audit time, so direct test execution from the host shell failed until dependencies are installed or containers are used.

## Type and contract model

- Backend route models use Pydantic plus `Literal` domain values.
- Frontend mirrors those contracts with TypeScript interfaces in `frontend/src/lib/financial-types.ts`.