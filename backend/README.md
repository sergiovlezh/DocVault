# DocVault backend

FastAPI backend for DocVault. Dependencies are managed with `uv` and pinned in `uv.lock`.

## Requirements

- Python 3.14+
- [uv](https://docs.astral.sh/uv/)

## Setup

```bash
uv sync --frozen
```

## Configuration

Create a `.env` file in this directory or export the variables directly:

```bash
DATABASE_URL=postgresql+asyncpg://docvault:change_me_in_local_env@localhost:5432/docvault
CORS_ORIGINS=http://localhost:5173
```

## Status

Only the uv dependency setup and a placeholder `main.py` exist. There is no FastAPI app, Alembic config, or test suite yet.

## Checks (once source and tests exist)

```bash
uv run ruff check .
uv run pytest
```
