# DocVault — Product Requirements Document (v1.0)

## 1. Vision
DocVault is a simple, self-hosted, open-source document management system inspired by Papra and Paperless-ngx. It provides a flat, tag-driven document bucket with multi-tenant user isolation, designed to be easy to deploy via Docker on a personal server or VPS.

## 2. Tech Stack & Architecture
- **Repository:** Public Monorepo (`backend/`, `frontend/`, `docs/`, `docker-compose.yml`, `.github/workflows/`), MIT License.
- **Backend:** FastAPI, Pydantic, Async SQLAlchemy 2.0 (`asyncpg`), Alembic, managed with `uv`.
- **Frontend:** Vite, React, TypeScript, Tailwind CSS, `react-router`, `@tanstack/react-query`, `@hey-api/openapi-ts`, `zod`, `lucide-react`.
- **Database:** PostgreSQL.
- **File Storage:** Local filesystem in a mounted Docker volume behind a `StorageBackend` abstraction (S3/R2-swappable in future releases).
- **Auth & Tenancy:** Multi-tenant from Day 1. First user registers as Admin and manages other user accounts. Strict privacy: Admins cannot view other users' documents. Cookie-based JWT auth (`HttpOnly`, `Secure`, `SameSite=Lax`, access + refresh rotation). Nullable `password_hash` on `User` for future OAuth readiness.

## 3. Core Data Model (v1.0)
- **User:** `id`, `email`, `password_hash` (nullable), `role` (`admin` | `user`), `is_active`, timestamps.
- **Document:** `id`, `user_id`, `title` (defaults to first file's original filename), timestamps.
- **File (1-to-Many under Document):** `id`, `document_id`, `original_filename`, `mime_type`, `size_bytes`, `sha256_checksum`, `storage_key`, `file_metadata` (JSONB: `page_count` for PDFs, `width`/`height` for images), timestamps.
- **DocumentNote (1-to-Many under Document):** `id`, `document_id`, `content`, timestamps.
- **Tag (User-scoped, Many-to-Many with Document):** `id`, `user_id`, `name`, `color` (hex, auto-assigned from curated palette if omitted).

## 4. v1.0 Functional Scope
- Admin bootstrap registration, login/logout, token refresh, and Admin user management.
- Single-file upload UI (backed by the 1-to-Many `Document` -> `File` schema).
- Supported formats: PDF, JPEG, PNG, WebP (configurable max size, default 25 MB) with backend magic-byte validation.
- Exact per-user duplicate rejection (`409 Conflict` via `sha256_checksum`).
- Flat document bucket with search by title and filter by tags.
- Document detail view: native browser preview, edit title, manage multiple notes, assign/remove tags, download, hard-delete with confirmation.

## 5. Definition of Done (CI Gates)
- **Backend:** `ruff` (lint + format) and `pytest` (against ephemeral PostgreSQL).
- **Frontend:** `tsc --noEmit`, `eslint`, and `vite build`.

## 6. Deferred Parking Lot (Out of Scope for v1.0)
- **v1.1:** Multi-file / version upload UI, server-side PDF thumbnails, Trash / Recycle Bin.
- **v2+:** OCR & full-text search, AI title/tag suggestions, auto-tagging rules, email/folder ingestion, OAuth (Google/OIDC), organizations & document sharing.