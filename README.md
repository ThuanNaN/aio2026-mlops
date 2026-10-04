# aio2026-mlops

AI VIETNAM AIO (2026) — MLOps Series

A hands-on MLOps series that takes an AI inference product from code to
production, in three parts:

1. **Backend** — build a local AI inference platform (FastAPI + Supabase + OCR models)
2. **Containerize** — package the platform with Docker
3. **Deploy** — ship it to production

## Repository structure

Each part lives in its own repository, tracked here as a git submodule:

| # | Directory | Source repository | Focus |
| --- | --- | --- | --- |
| 01 | [`01-backend-fastapi`](./01-backend-fastapi) | [dangnha/routes-for-AI-inference](https://github.com/dangnha/routes-for-AI-inference) | Three-tier app: Streamlit UI → FastAPI + Supabase backend → OCR AI service |
| 02 | [`02-containerize-docker`](./02-containerize-docker) | [ThuanNaN/packaging-ai-inference-with-docker](https://github.com/ThuanNaN/packaging-ai-inference-with-docker) | Dockerizing the platform |
| 03 | [`03-deploy-ai-inference`](./03-deploy-ai-inference) | [ThuanNaN/deploying-ai-inference-to-production](https://github.com/ThuanNaN/deploying-ai-inference-to-production) | Deploying the inference service to production |

## The platform (parts 01–02)

A fully local OCR platform in three tiers:

- **Frontend** — Streamlit: sign in, upload an image or pick an example, view results.
- **Backend** — FastAPI + Supabase: auth (Supabase Auth), task management, presigned
  uploads to Supabase Storage, inference orchestration, and webhook callbacks.
- **AI service** — FastAPI serving three OCR models as background jobs that report
  results back via webhook:

| Slug | Model | Notes |
| --- | --- | --- |
| `trocr` | `microsoft/trocr-base-printed` | Single line of printed text |
| `easyocr` | EasyOCR (CRAFT + CRNN) | Multi-line detection + recognition (receipts, signs) |
| `gotocr` | `stepfun-ai/GOT-OCR2_0` | General OCR → Markdown (requires a GPU) |

Supabase is the single data layer: **Postgres** (tasks), **Auth** (users),
**Storage** (images). Model weights are downloaded once into `models/`, so
inference runs fully offline afterwards.

See [`01-backend-fastapi/README.md`](./01-backend-fastapi/README.md) for full
setup, API reference, environment variables, and testing instructions.

## Getting started

Clone with submodules:

```bash
git clone --recurse-submodules <repo-url>
cd aio2026-mlops
```

If you already cloned without them:

```bash
git submodule update --init --recursive
```

Each part has its own `README.md` — start with
[`01-backend-fastapi/README.md`](./01-backend-fastapi/README.md) for the
platform walkthrough.

## Requirements

- Python 3.12+
- A Supabase project (Postgres + Auth + Storage)
- NVIDIA GPU recommended (GOT-OCR2.0 requires a GPU); CPU works for the rest
- Docker (part 02)

## Series progress

- [x] 01 — Backend: FastAPI + Supabase + local OCR models
- [ ] 02 — Containerize with Docker
- [ ] 03 — Deploy to production
