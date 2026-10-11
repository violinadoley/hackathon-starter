# GitHub branch protection (`main` + `develop`)

Target repo: **[violinadoley/hackathon-starter](https://github.com/violinadoley/hackathon-starter)** (public).

Both **`main`** and **`develop`** should require:

- A **pull request** before merge (0 approvals for solo hackathon; use 1 on `main` when you add teammates)
- **Strict** required status checks: `backend`, `frontend`, `ai-evals`, `e2e`, `secret-scan`
- No force-push or branch deletion (optional: allow admin bypass on `develop` only)

## Required CI check names

From the **CI** workflow:

| Check name | Job |
|------------|-----|
| `backend` | Lint, types, tests, pip-audit, bandit |
| `frontend` | Lint, typecheck, build |
| `ai-evals` | AI regression framework |
| `e2e` | Playwright |
| `secret-scan` | Tracked secret filename guard |

Do **not** require Deploy API / Deploy Frontend on PRs.

## UI steps

1. Open **https://github.com/violinadoley/hackathon-starter/settings/branches**
2. Add rule for **`main`**, then repeat for **`develop`**
3. Enable **Require a pull request before merging** and **Require status checks** (select checks above)
4. Save

## Verify

Open a test PR to `develop` — merge should stay blocked until CI is green.
