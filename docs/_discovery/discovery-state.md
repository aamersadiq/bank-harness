# Discovery State

> **Last updated:** 2026-07-20
> **Purpose:** The evolving working memory for this extraction. Read at the start of every
> session; rewrite as understanding changes. NOT an onboarding doc.

## Scope & mode

- **Target:** Whole repo — `bank-harness` (banking-app), a Node.js/TypeScript DDD banking service.
- **Mode:** full (stakeholder interview conducted with the repo owner, Aamer Sadiq)
- **Available inputs:** git history (yes, but no domain signal — only `init` + skill-add commits) · code-intelligence/LSP (no) · sub-agents (available, not needed — repo is ~24 source files) · stakeholder (yes)
- **Navigation mode chosen:** grep + direct reads (whole repo read exhaustively; small codebase)

## Facts (accepted, unmarked)

- The system is a REST API for managing **bank accounts** and **money movements** (deposit,
  withdraw, transfer) — source: README.md, `src/api/routes/*`, stakeholder.
- Layered DDD: API → Application services → Domain (entities/value objects/domain service) →
  Infrastructure (Mongoose repositories + schemas). Source: `src/**`.
- Persistence is MongoDB via Mongoose; domain entities carry a UUID stored in a custom `uuid`
  field, separate from Mongo `_id`. Source: `src/infrastructure/schemas/*`, `.../repositories/*`.
- Money is a value object rounding to 2 decimal places. Source: `src/domain/valueObjects/money.ts:10`.
- Transaction lifecycle: PENDING → COMPLETED (`complete()`) or FAILED (`fail()`). Source:
  `src/domain/entities/transaction.ts:127-133`.
- No authentication/authorization is implemented; a `bearerAuth` scheme is declared in Swagger
  but never applied. Source: `src/api/swagger/swagger.ts:26-34`, absence of auth middleware.

## Assumptions

See `assumptions-register.md` for the full list with impact.

## Unknowns / open questions

All material ones were raised in the interview and are recorded as resolved in the assumptions
register. Remaining low-priority items are flagged inline in the docs.

## Decisions

- Purpose confirmed by stakeholder: this is a **practice/demo "harness" codebase** — a
  reference banking service used for experimenting with tooling (incl. this discovery skill),
  not a production system. Validated by: Aamer Sadiq (repo owner).

## Glossary in progress

See `docs/domain/domain-glossary.md` (synthesised).

## Existing-doc claims (from Phase 0), status after recon

| Claim | Source doc | Status after recon |
|---|---|---|
| Repositories live under `infrastructure/database/repositories/` | docs/architecture.md | [outdated] — actually `infrastructure/repositories/` |
| Value object `accountId.ts` exists | docs/architecture.md | [outdated] — no such file; id is a plain string |
| Domain service `transactionService.ts` exists | docs/architecture.md | [outdated] — only `accountService.ts` exists; it holds deposit/withdraw/transfer |
| Domain event `transactionCreatedEvent.ts` / "Domain events" | docs/architecture.md | [outdated] — no events implemented |
| Transaction `fromAccountId`/`toAccountId` stored as ObjectId | docs/architecture.md | [outdated] — stored as String |
| Account schema has no `uuid` field | docs/architecture.md | [outdated] — schema has a required unique `uuid` |
| `tests/integration/` exists | docs/architecture.md | [outdated] — only `tests/unit/` present |
| TransactionApplicationService contains inline balance logic | docs/application-services.md | [outdated] — delegates to domain `AccountService` |
| Controllers return bare JSON (e.g. `res.json(accounts)`) | docs/application-services.md | [outdated] — responses wrapped in `{ success, data }` |
| API layer provides "Authentication/authorization" | docs/architecture.md, README | [contradicted] — not implemented |
| docker-compose service named `mongodb` | docs/implementation.md | [outdated] — service is `mongo` |
| Endpoints (accounts CRUD-lite + deposit/withdraw/transfer) | README.md | accepted |
| DDD layered architecture | README.md, docs/architecture.md | accepted |
| Tech stack: Express, TS, Mongoose, Joi, Swagger, Jest | README.md | accepted |
