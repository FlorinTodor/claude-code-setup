# <project name>

<!-- One or two lines. What this repo is and what it is for. Claude reads this
     file at the start of every session, so every line here costs context.
     Rule of thumb from the docs: under 200 lines, and for each line ask
     "would removing this cause Claude to make a mistake?" -->

<project name> is a <what it is> for <who uses it>. Python 3.12, Flask, PostgreSQL.

## Commands

<!-- Commands Claude cannot guess by reading the code. Keep the exact flags. -->

```bash
make dev          # run locally on :8000 (needs .env, see below)
make test         # pytest -q, runs in under a minute
make lint         # ruff check . && ruff format --check .
make migrate      # alembic upgrade head
```

Run `make test` after any change to `src/`. Prefer a single test file (`pytest tests/test_x.py`) over the whole suite while iterating.

## Layout

<!-- Only what is not obvious from the tree. No file-by-file descriptions. -->

- `src/<package>/domain/` : entities and use cases. No framework imports here.
- `src/<package>/adapters/` : Flask routes, SQLAlchemy repositories, external APIs.
- `src/<package>/infra/` : config, logging, dependency wiring.
- `tests/` : mirrors `src/`. Fixtures live in `tests/conftest.py`.
- `migrations/` : Alembic. Never edit an applied migration; add a new one.

## Conventions

- Type hints everywhere. `ruff` is the only formatter; do not reformat by hand.
- Errors cross layer boundaries as domain exceptions (`src/<package>/domain/errors.py`), never as HTTP codes.
- Pydantic models for every request and response body. No raw `dict` in route signatures.
- Database access only through repository classes. No SQLAlchemy session in a route.
- Logging via `get_logger(__name__)`. No `print`.

## Environment

- Copy `.env.example` to `.env`. `DATABASE_URL` and `SECRET_KEY` are required; the app refuses to start without them.
- Local Postgres runs with `docker compose up -d db`.
- Python version is pinned in `.python-version`. Use the project venv (`.venv/`), never system pip.

## Git

- Branch from `main` as `feat/<short-name>` or `fix/<short-name>`.
- Commit messages in imperative mood, one logical change per commit.
- Never commit `.env`, `*.db`, or anything under `data/`.
- Do not push or open a PR unless asked.

## Rules for Claude

<!-- Short, verifiable, specific. "Write clean code" is noise. -->

- IMPORTANT: do not change files under `migrations/` unless the task is explicitly a migration.
- Before marking a task done, run `make lint` and `make test` and paste the result.
- If a request is ambiguous about scope, ask one question before editing.
- When you are unsure how something works, read the code first; do not guess the API of internal modules.
- When compacting, always preserve the list of modified files and the test command.

## Known gotchas

<!-- The things you would otherwise re-explain every session. Add to this list
     every time Claude trips on something twice. -->

- `make test` needs the `db` container up or the integration tests hang.
- The `users` table has a legacy `email_lower` column; always write both `email` and `email_lower`.
