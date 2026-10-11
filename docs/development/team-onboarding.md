# Team onboarding

Quick path for new developers on **[violinadoley/hackathon-starter](https://github.com/violinadoley/hackathon-starter)**.

## What you need from the team lead

Share via your **password manager** (never commit or paste in chat):

| Secret / config | Used for |
|-----------------|----------|
| `GEMINI_API_KEY` | Local backend AI calls |
| `NEXT_PUBLIC_FIREBASE_*` (optional) | Local Google sign-in + Firebase client |
| Repo access | GitHub collaborator on `hackathon-starter` |

**You do not need** GCP admin or GitHub Actions variables for normal feature work — those are already set for deploys.

## Live environments (no local setup)

| | URL |
|--|-----|
| Web app | https://project-bf77a013-c0b3-413c-a75.web.app/dashboard |
| API (Swagger) | https://ai-hackathon-api-2lcs3sivbq-el.a.run.app/docs |

## Local setup

**Prerequisites:** Node 20+ (22 matches CI), Python 3.11+, Git.

```bash
git clone https://github.com/violinadoley/hackathon-starter.git
cd hackathon-starter
git checkout develop
```

**Environment files:**

```bash
cp backend/.env.example backend/.env
cp frontend/.env.local.example frontend/.env.local
```

Edit `backend/.env` — set at least:

```env
GEMINI_API_KEY=your_key_from_team_lead
```

Edit `frontend/.env.local` — default `NEXT_PUBLIC_API_URL=http://localhost:8000` is enough for UI + API without login.

**Run** (two terminals, or read `./scripts/dev.sh`):

```bash
# Terminal 1 — API
cd backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000

# Terminal 2 — UI
cd frontend
npm install
npm run dev
```

Open http://localhost:3000 · API docs http://localhost:8000/docs

## Git workflow

1. Branch from **`develop`**: `git checkout -b feature/my-change`
2. Open a PR into **`develop`** — CI must pass (`backend`, `frontend`, `ai-evals`, `e2e`, `secret-scan`)
3. Merges to **`develop`** deploy API + Firebase Hosting (team GCP project)
4. Merge **`develop` → `main`** when you want a stable snapshot (no extra deploy on `main` alone)

See [CONTRIBUTING.md](../../CONTRIBUTING.md) and [workflow.md](./workflow.md).

## Verify your machine

```bash
cd backend && pip install -r requirements-dev.txt && pytest -q
cd ../frontend && npm ci && npm run lint && npm run typecheck && npm run build
```

## Stack overview

| Layer | Tech |
|-------|------|
| Frontend | Next.js, React, TypeScript, Tailwind, shadcn/ui |
| Backend | FastAPI, Python 3.11+, Pydantic Settings |
| AI | Google Gemini (`google-genai`) |
| Auth / data | Firebase Auth, Firestore, Cloud Storage (via API) |
| Deploy | Cloud Run + Firebase Hosting (GitHub Actions on `develop`) |

More detail: [README.md](../../README.md), [architecture.md](../architecture.md), [security-defaults.md](../security-defaults.md).

## Admin-only (not required for teammates)

- GitHub Actions **Variables** — Settings → Secrets and variables → Actions
- GCP OIDC / WIF — [github-google-oidc.md](../setup/github-google-oidc.md)
- Firebase setup — [firebase-hosting.md](../setup/firebase-hosting.md), [firebase-backend.md](../setup/firebase-backend.md)
