# DocVault

A simple, self-hosted, multi-tenant document management system inspired by [Papra](https://papra.app/) and [Paperless-ngx](https://docs.paperless-ngx.com/).

> **Status:** Early development (`v1.0` in progress).

## Overview

DocVault provides a flat, tag-driven document bucket with strict per-user isolation (similar to Immich). It is designed to be lightweight, easy to self-host with Docker, and straightforward to extend.

## Tech Stack

- **Backend:** FastAPI, Pydantic, Async SQLAlchemy 2.0 (`asyncpg`), Alembic, managed with `uv`
- **Frontend:** Vite, React, TypeScript, Tailwind CSS, React Router, TanStack Query, Zod, OpenAPI TS client
- **Database:** PostgreSQL
- **Storage:** Local filesystem behind a `StorageBackend` abstraction (S3/R2-ready)
- **Deployment:** Docker & Docker Compose

## Documentation

- [Product Requirements Document (v1.0)](docs/PRD.md) — full architecture, data model, scope, and roadmap.

## License

This project is licensed under the [MIT License](LICENSE).