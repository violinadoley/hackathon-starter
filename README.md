# AI Hackathon Starter

**Repo:** [violinadoley/hackathon-starter](https://github.com/violinadoley/hackathon-starter) · **Release:** [v1.0.0](https://github.com/violinadoley/hackathon-starter/releases/tag/v1.0.0)

| | |
|---|---|
| **Live app** | [Dashboard](https://project-bf77a013-c0b3-413c-a75.web.app/dashboard) |
| **API (Swagger)** | [Cloud Run /docs](https://ai-hackathon-api-2lcs3sivbq-el.a.run.app/docs) |
| **New teammate?** | [Team onboarding](docs/development/team-onboarding.md) |
| **Starter guide** | [docs/starter-template.md](docs/starter-template.md) |

Google-native **starter monorepo** for an AI hackathon: Next.js dashboard, FastAPI + Gemini, Firebase hooks, Docker for Cloud Run, tests, and CI/CD.

Customize problem statement, branding, and domain logic for your competition track.

## Overview

Production-quality platform starter — not the final product. After v1.0.0, your team adds prompts, routes, and UI on **`develop`**.

## Problem

TODO: Add competition problem statement

## Solution

TODO: Describe the proposed AI solution

## Key Features

- Landing page and AI dashboard (text + file upload)
- Gemini via `google-genai` (model from env)
- Firebase Auth (Google), Firestore activity feed, GCS uploads via API
- Optional auth (`AUTH_REQUIRED=false` for local dev)
- Structured API errors and JSON logging for Cloud Run
- Backend pytest, ai-evals regression harness, GitHub Actions CI

## Tech Stack

| Layer | Technologies |
|-------|----------------|
| Frontend | Next.js, React, TypeScript, Tailwind CSS, shadcn/ui |
| Backend | Python 3.11+, FastAPI, Uvicorn, Pydantic Settings |
| AI | Google Gemini (`google-genai`) |
| Data / Auth | Firebase (Auth, Firestore, Storage) |
| Deploy | Cloud Run (API), Firebase Hosting (static UI) |

## Local Setup

See **[docs/development/team-onboarding.md](docs/development/team-onboarding.md)** for the full teammate flow.

```bash
git clone https://github.com/violinadoley/hackathon-starter.git
cd hackathon-starter && git checkout develop
cp backend/.env.example backend/.env
cp frontend/.env.local.example frontend/.env.local
# Set GEMINI_API_KEY in backend/.env
```

Or run `./scripts/dev.sh` for terminal commands.

## Environment Variables

**Backend** (`backend/.env`): see [backend/.env.example](backend/.env.example).

**Frontend** (`frontend/.env.local`): see [frontend/.env.local.example](frontend/.env.local.example).

**GitHub Actions (deploy):** see [docs/security-defaults.md](docs/security-defaults.md) — `PUBLIC_API_URL`, `BACKEND_API_URL`, GCP WIF vars, Firebase build vars.

## Running Frontend

```bash
cd frontend && npm install && npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Running Backend

```bash
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Health: [http://localhost:8000/health](http://localhost:8000/health)

## Testing

See [docs/development/testing-strategy.md](docs/development/testing-strategy.md).

```bash
cd backend && pip install -r requirements-dev.txt
ruff check app tests && mypy app && pytest -q
PYTHONPATH=. pytest ../ai-evals/regression -q

cd frontend && npm ci
npm run lint && npm run typecheck && npm run build
npm run test:e2e
```

## Git workflow

- Feature branches → PR → **`develop`** (deploys to Cloud Run + Hosting)
- **`main`** = stable snapshot (CI only; no deploy on push to `main` alone)
- [CONTRIBUTING.md](CONTRIBUTING.md)

## CI/CD

| Workflow | Trigger |
|----------|---------|
| [ci.yml](.github/workflows/ci.yml) | PR + push to `main` / `develop` |
| [deploy.yml](.github/workflows/deploy.yml) | push to `develop` |
| [deploy-frontend.yml](.github/workflows/deploy-frontend.yml) | push to `develop` |

Setup: [docs/setup/github-google-oidc.md](docs/setup/github-google-oidc.md) · [firebase-hosting.md](docs/setup/firebase-hosting.md) · [firebase-backend.md](docs/setup/firebase-backend.md)

## Deployment

- [docs/deployment.md](docs/deployment.md)
- [docs/deployment/cloud-run.md](docs/deployment/cloud-run.md)
- [docs/runbooks/rollback.md](docs/runbooks/rollback.md)

## Project Structure

```
frontend/          Next.js App Router UI
backend/app/       FastAPI application
backend/tests/     Pytest suite
docs/              Architecture, ADRs, runbooks, onboarding
ai-evals/          AI regression framework
infra/             Infra notes (no Terraform in starter)
scripts/           dev.sh, deploy helpers, smoke tests
.github/workflows/ CI + deploy
```

## Starter template status

Platform starter (CI/CD, Hosting, Cloud Run, Auth, activity, uploads) is **complete on v1.0.0**. See [docs/starter-template.md](docs/starter-template.md).

## Future Enhancements (product team)

- [ ] Competition-specific prompts and workflows
- [ ] Richer chat history and user-scoped activity
- [ ] RAG + Gemini embeddings
- [ ] Google ADK agentic tools

## Competition Submission Checklist

- [ ] Problem statement and Gemini integration
- [ ] Functional prototype and cloud deployment
- [ ] Public demo URL, demo video, proposal PDF
- [ ] Documentation and impact / scalability narrative

## License

[MIT](LICENSE)
