# User Personas & Stakeholders

> **Last updated:** 2026-07-20
> **Scope:** Who uses and owns the banking service
> **Mode:** full
> **Status:** accepted knowledge unless flagged — see ../_discovery/assumptions-register.md

The code has **no authentication, roles, or user model**, so there are no personas expressed in
the system itself. The following reflect how the harness is actually used, confirmed with the
owner.

## User personas

| Persona | Who they are | What they do with the system | Key needs |
|---|---|---|---|
| Developer / learner | internal | Runs the API locally, calls the endpoints (or Swagger UI) to exercise account and transaction flows | A clear, correct reference implementation of DDD banking logic |
| AI agent / tooling | internal | Reads the code and these docs to reason about or modify the system (e.g. discovery, spec-driven changes) | Accurate onboarding docs and a small, well-structured codebase |

[assumption] If this were ever taken toward production, an **end customer** (account holder) and
a **bank operator/teller** persona would emerge — but neither exists in the code today.

## Stakeholders (not necessarily users)

| Stakeholder | Interest / responsibility |
|---|---|
| Repo owner (Aamer Sadiq) | Owns the codebase and its direction; validated the purpose and gaps during discovery |
