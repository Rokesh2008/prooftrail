# ProofTrail

**Evidence for every claim.** ProofTrail helps teams identify when marketing content changes the meaning, strength, scope, or qualifications of an approved claim. Findings link to source excerpts, and final decisions remain with human reviewers.

> ProofTrail is a review aid, not a legal-compliance certification system. Missing evidence does not prove a statement is false. Demo workspace records are fictional.

## Core workflow

1. Create a Claim Passport with approved wording, product/market scope, permitted channels, conditions, source excerpts and a revalidation date.
2. Submit draft content by pasting text or uploading PDF, DOCX, TXT or Markdown (10 MB maximum by default). Scanned PDFs are not OCR-processed.
3. Compare claims with approved records using deterministic checks for numbers, absolute phrasing, qualifiers, evidence age and scope. An optional server-side OpenAI-compatible model provides advisory observations only.
4. Inspect each finding with the submitted phrase, closest approved claim, source excerpt, explanation and suggested correction.
5. Let a human reviewer approve, request changes or reject. Approving unresolved findings requires a written decision comment.
6. Review activity history and computed governance insights.

## Stack

- Frontend: React, TypeScript, Vite, React Router, Lucide, responsive custom CSS.
- Backend: FastAPI, Pydantic, SQLAlchemy, Alembic, PBKDF2 password hashes and database-backed HTTP-only sessions.
- Data: SQLite for local/demo use; PostgreSQL supported for deployment. Evidence files are kept outside the public web root.
- Deployment: Docker Compose, non-root containers, Nginx reverse proxy, health checks and GitHub Actions CI.

## Local development

Requirements: Python 3.12+, Node.js 22+ and npm.

Backend:

    cd backend
    python -m venv .venv
    # Windows: .venv\\Scripts\\Activate.ps1 | macOS/Linux: source .venv/bin/activate
    pip install -r requirements.txt
    uvicorn app.main:app --reload --port 8000

Frontend in another terminal:

    cd frontend
    npm install
    npm run dev

Open http://localhost:5173 and choose Open demo workspace. API docs are available at http://localhost:8000/api/docs in development.

## Docker Compose

    cp .env.example .env
    # Change PROOFTRAIL_SESSION_SECRET before exposing anything publicly.
    docker compose up --build

Open http://localhost:8080. Default Compose settings are for local demonstration only.

## Production deployment checklist

Do not expose the demo configuration publicly. Before production use:

1. Set PROOFTRAIL_ENVIRONMENT=production and PROOFTRAIL_DEMO_MODE=false. Keep self-service signup disabled until email verification and abuse protections exist.
2. Set a unique random PROOFTRAIL_SESSION_SECRET of at least 32 characters in a secret manager.
3. Terminate HTTPS at a trusted ingress and set PROOFTRAIL_COOKIE_SECURE=true.
4. Use PostgreSQL, TLS, least-privilege credentials, persistent private upload storage, backups and restore drills. Back up database and evidence files together.
5. Set PROOFTRAIL_CORS_ORIGINS to exact trusted origins.
6. Configure AI provider secrets only on the backend. When enabled, draft content and matched claim context are sent to the configured provider; review provider terms and disclose this to users.
7. Bootstrap one administrator with demo mode disabled: set PROOFTRAIL_ADMIN_EMAIL, PROOFTRAIL_ADMIN_PASSWORD and PROOFTRAIL_ADMIN_NAME, then run python -m app.create_admin once. Bootstrap refuses if a user already exists.
8. Add deployment-level rate limiting, monitoring, log redaction, dependency scanning and a security review before storing confidential source documents.
9. Confirm retention, permissions, backups, data isolation, incident response and accessibility in the target deployment. Do not claim detection accuracy, legal compliance or ROI without documented evaluation.

## API areas

- /api/health — health and database connectivity.
- /api/auth/* — demo access, login, logout, current session and password change.
- /api/claims — Claim Passports and archive operations.
- /api/sources — source records, document upload, secure download and validation.
- /api/reviews — text/document review, findings, reruns and human decisions.
- /api/insights, /api/activity and /api/settings — dashboard data, history and defaults.
- /api/members — administrator-managed accounts and roles.

## Test and build

    cd backend && PYTHONPATH=. pytest -q
    cd ../frontend && npm install && npm run build

GitHub Actions runs backend tests and the frontend build on pushes to main/develop and on pull requests.

## Known limitations

- The review engine is an assistive first pass, not a validated general-purpose entailment model. It may miss subtle implied claims or flag valid paraphrases.
- The optional LLM is advisory. Deterministic checks need a larger labelled evaluation set before any accuracy claim.
- OCR, image/video analysis, publishing integrations and workflow notifications are not implemented.
- This release is single-workspace, not multi-tenant SaaS. Do not run unrelated organisations in one instance. Accounts are administrator-created; email invitations and password-reset delivery are not implemented.
- No formal penetration test, accessibility audit or independent security review has been performed.

## Responsible use

ProofTrail compares copy with evidence supplied by the organisation. An absent record is an evidence gap, not proof of falsity; expired evidence requires revalidation, not automatic rejection. Approval records an internal review action, not independent validation of scientific, legal or commercial truth.
