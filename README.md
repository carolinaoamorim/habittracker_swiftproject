# Nutrition Tracker

A full-stack nutrition tracking app: log foods, build and score diet plans, and
manage intermittent-fasting protocols. Food data is sourced from the USDA
FoodData Central API.

## Tech stack

| Layer     | Technology                                        |
| --------- | ------------------------------------------------- |
| Backend   | FastAPI, SQLAlchemy, Alembic, Pydantic            |
| Database  | PostgreSQL 16                                     |
| Optimizer | PuLP (linear-programming diet optimization)       |
| Frontend  | React 18, Vite, React Router, Axios               |
| Infra     | Docker, Docker Compose                            |

## Project layout

```text
backend/
  app/
    api/         # FastAPI routers (foods, users, diet_plans, fasting)
    models/      # SQLAlchemy ORM models
    schemas/     # Pydantic request/response schemas
    services/    # Business logic (optimizer, diet_scoring)
    external/    # Third-party clients (USDA FoodData Central)
    config.py    # Settings (pydantic-settings)
    db.py        # Engine / session
    main.py      # App entrypoint
  migrations/    # Alembic migrations
  tests/
frontend/
  src/
    api/         # Axios client
    components/
    pages/
docs/
```

## Getting started (Docker)

1. Copy the environment template and adjust values as needed:

   ```bash
   cp .env.example .env
   ```

   Set `USDA_API_KEY` (get a [free key](https://fdc.nal.usda.gov/api-key-signup.html))
   and a strong `SECRET_KEY`.

2. Build and start the stack:

   ```bash
   docker compose up --build
   ```

3. Services:

   - Frontend (Vite dev server): <http://localhost:5173>
   - Backend (FastAPI + docs): <http://localhost:8000/docs>
   - Postgres: `localhost:5432`

## Local development (without Docker)

### Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

## Database migrations

```bash
# create a new migration from model changes
docker compose exec backend alembic revision --autogenerate -m "message"

# apply migrations
docker compose exec backend alembic upgrade head
```

## Tests

```bash
docker compose exec backend pytest
```
