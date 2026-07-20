# Assumptions Register

> **Last updated:** 2026-07-20
> **Purpose:** Single list of everything still needing attention. NOT an onboarding doc — do
> not link from CLAUDE.md / AGENTS.md.

| ID | Item | Flag | Confidence | Impact if wrong | Source | Resolution / next step |
|---|---|---|---|---|---|---|
| A-1 | Transfers are not atomic: `withdraw` then `deposit` then two separate `accountRepository.save()` calls with no DB session/transaction. A failure after the first save leaves balances inconsistent, with no rollback. | [risk] | H | Money can be debited without being credited (or vice versa); ledger and balances diverge. | `src/application/services/transactionApplicationService.ts:103-117`, `src/domain/services/accountService.ts:81-115` | Confirmed by stakeholder as a real risk to fix (wrap in a Mongo transaction / session). |
| A-2 | No authentication or authorization anywhere. A `bearerAuth` scheme is declared in Swagger but never applied to any route. | [risk] / [contradicted] | H | Any caller can read all accounts and move money. README/architecture claim an auth layer that does not exist. | `src/api/swagger/swagger.ts:26-34`; no auth middleware in `src/server.ts` / routes | Confirmed as a real gap to fix. Correct the doc claim. |
| A-3 | `Money` has no currency — it is a bare `number`. All amounts are assumed to be a single implicit currency. | [risk] | M | Multi-currency or FX scenarios are impossible; mixing currencies would be silent data corruption. | `src/domain/valueObjects/money.ts` | Confirmed as a real gap to fix if multi-currency is ever needed. |
| A-4 | FAILED transactions are never persisted. The app services only `save()` a transaction on the success path; when the domain service throws (e.g. insufficient funds) the error propagates and nothing is written, so the DB only ever holds COMPLETED transactions. | [unverified] | M | Transaction history/audit will never show failed attempts, despite a FAILED status existing in the model. | `src/application/services/transactionApplicationService.ts:59-79,84-121`; `src/domain/services/accountService.ts:48-76` | Behaviour derived from code; confirm whether persisting FAILED attempts is desired. |
| A-5 | `AccountRepository.delete()` queries by Mongo `_id` (`{ _id: id }`) whereas every other method resolves accounts by the custom `uuid` field. Passing a domain id (UUID) to `delete` would not match. | [unverified] | L | Latent bug, but `delete` is not exposed via any route today, so no live impact. | `src/infrastructure/repositories/accountRepository.ts:60-63` vs `:21-24` | Code inconsistency; fix if `delete` is ever wired up. |
| A-6 | Intended-but-unimplemented rules: per-transaction / daily transaction limits and/or a minimum balance. | [assumption] | M | Recorded as requirements the harness does not yet meet; must not be presented as enforced. | Stakeholder (2026-07-20) | Recorded as FR gaps in business-requirements.md; implement when in scope. |
| A-7 | Intended-but-unimplemented: account types (e.g. savings/checking) and/or an account lifecycle (open/frozen/closed). | [assumption] | M | Recorded as future requirements; the model has no `type` or `status` on Account today. | Stakeholder (2026-07-20) | Recorded as FR gaps in business-requirements.md. |
## Resolved

| ID | Item | Resolved to | By whom | Date |
|---|---|---|---|---|
| R-1 | Purpose of the repo (name "bank-harness" vs plain banking API) | A practice/demo reference banking service, not a production product | Aamer Sadiq | 2026-07-20 |
| R-2 | Domain service naming drift (docs mention `transactionService.ts`) | Only `accountService.ts` exists and holds deposit/withdraw/transfer logic; app services delegate to it | Code + stakeholder | 2026-07-20 |
| A-8 | Drifted design-time docs (`docs/architecture.md`, `docs/application-services.md`, `docs/implementation.md`) | Deleted (recoverable via git history) and superseded by the new `docs/` set | Aamer Sadiq | 2026-07-20 |
