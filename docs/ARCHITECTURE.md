# ProofTrail architecture

```text
Browser (React + TypeScript)
  | same-origin /api requests and HTTP-only session cookie
  v
Nginx static frontend + reverse proxy
  v
FastAPI API ---- optional OpenAI-compatible provider (server-side only)
  |              draft + matched claim context sent only when enabled
  +-- Evidence extractor: PDF / DOCX / TXT / Markdown
  +-- Deterministic checks: numbers, absolutes, qualifiers, age, scope
  +-- SQLAlchemy models: SQLite locally or PostgreSQL in deployment
  +-- Private file directory with random storage keys
```

## Invariants

- The server, not an LLM, assigns source IDs used by findings.
- Missing evidence is represented as a gap, not proof that the claim is false.
- A source begins in needs-review state; review is required before claims can rely on it.
- A Claim Passport may be approved only when linked to approved evidence.
- Approval of unresolved findings requires a written reviewer comment.
- Material edits and decisions create audit events.
- Provider credentials remain on the server. The deterministic checks still run if the optional provider is disabled or fails.

## Deployment boundary

The initial release is single-workspace, not multi-tenant. Do not host unrelated customer organisations in one instance or database. Before confidential production use, complete environment-specific security, backup, isolation, accessibility and reliability reviews.
