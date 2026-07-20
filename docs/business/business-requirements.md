# Business Requirements

> **Last updated:** 2026-07-20
> **Scope:** Functional + non-functional requirements of the banking service, reconstructed from code and validated with the owner
> **Mode:** full
> **Status:** accepted knowledge unless flagged — see ../_discovery/assumptions-register.md

## Purpose & outcomes

The system is a **practice/demo banking service** used as a reference codebase for exercising
tooling and techniques (including this discovery skill) — it is not a production product. Its
job is to correctly model bank accounts and the movement of money between them (deposit,
withdraw, transfer) behind a small REST API, applying core money-safety rules such as
no-overdraft.

## Functional requirements

| ID | Requirement | Source | Status |
|---|---|---|---|
| FR-1 | The system shall let a caller create an account with a name; it opens at a zero balance. | code | accepted |
| FR-2 | The system shall list all accounts and fetch a single account by id. | code | accepted |
| FR-3 | The system shall deposit a positive amount into an account, increasing its balance. | code | accepted |
| FR-4 | The system shall withdraw a positive amount from an account, provided the balance covers it (no overdraft). | code | accepted |
| FR-5 | The system shall transfer a positive amount between two distinct accounts, debiting the source and crediting the target. | code | accepted |
| FR-6 | The system shall record every movement as a Transaction (type, amount, accounts, description, status, timestamp). | code | accepted |
| FR-7 | The system shall return an account's transaction history (as source or target), newest first. | code | accepted |
| FR-8 | The system shall validate request input (name 3–50, description 3–100, amount positive with ≤2 decimals). | code | accepted |
| FR-9 | The system should enforce transaction/balance limits (per-transaction cap, daily limit, or minimum balance). | stakeholder | [assumption] — not implemented (A-6) |
| FR-10 | The system should support account types (e.g. savings/checking) and/or an account lifecycle (open/frozen/closed). | stakeholder | [assumption] — not implemented (A-7) |
| FR-11 | The system should persist failed transaction attempts for audit. | code analysis | [unverified] — not implemented today (A-4) |

## Non-functional requirements

Because this is a demo harness, no production NFR targets have been set. The following are the
constraints observed in code plus the gaps the owner wants treated as real risks to fix.

| ID | Category | Requirement / constraint | Source | Status |
|---|---|---|---|---|
| NFR-1 | Security | Requests set `helmet` and permissive `cors`; **no authentication/authorization** exists though a `bearerAuth` scheme is declared in Swagger. | code | [risk] — auth must be added (A-2) |
| NFR-2 | Data integrity | Transfers must move money atomically; today the debit and credit are separate saves with no DB transaction. | code | [risk] — atomicity must be added (A-1) |
| NFR-3 | Correctness | Money must not lose precision or mix currencies; today `Money` is a bare number with no currency. | code | [risk] — currency model needed if multi-currency (A-3) |
| NFR-4 | Observability | Errors are logged to console; requests logged via `morgan('dev')`. No structured logging/audit trail. | code | accepted (as-is) |
| NFR-5 | Testing | Domain entities are unit-tested (Jest); no integration/API tests exist. | code | accepted (as-is) |

## Out of scope / known gaps

- No authentication, authorization, users, or roles.
- No atomic/rolled-back transfers.
- No currency or multi-currency support.
- No limits, minimum balance, account types, lifecycle, interest, or fees.
- No integration tests, background jobs, or external integrations beyond MongoDB.
