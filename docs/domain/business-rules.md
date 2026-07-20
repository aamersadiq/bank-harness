# Business Rules

> **Last updated:** 2026-07-20
> **Scope:** Rules enforced (and a few intended-but-unenforced) by the banking service
> **Mode:** full
> **Status:** accepted knowledge unless flagged — see ../_discovery/assumptions-register.md

These are the load-bearing rules a new joiner (human or AI) must know. Rules are enforced at
one or more layers: the **Joi request validation** (API edge), the **domain entities**
(`Account`, `Transaction`), and the **Mongoose schema** (persistence). Where a rule appears at
several layers it is noted.

## Money & amounts

- **Amounts are always positive.** Deposit, withdrawal and transfer amounts must be greater
  than zero; zero and negative amounts are rejected. Enforced in the domain (`Account.deposit`/
  `withdraw`, `Transaction` constructor) and at the API edge (Joi `positive()`).
- **Money is rounded to 2 decimal places.** Any amount is rounded on construction of a `Money`
  value (`Math.round(value * 100) / 100`). The API also rejects more than 2 decimal places up
  front (Joi `precision(2)`), and the schema requires a persisted amount of at least `0.01`.
- **No currency.** All amounts are a single implicit currency; there is no currency field
  (risk A-3).

## Accounts

- **New accounts start at a zero balance.** Account creation takes only a name; the opening
  balance is always zero.
- **Account name is 3–50 characters** and required (Joi).
- **Balance cannot go negative.** Enforced by the domain withdrawal check and by a schema
  `min: 0` constraint on the persisted balance.

## Withdrawals & the no-overdraft rule

- **No overdraft.** A withdrawal (or the debit side of a transfer) is only allowed when the
  source account's balance is greater than or equal to the amount. Otherwise the operation is
  rejected with **"Insufficient funds"**. This is the single most important money rule in the
  system. There are **no exceptions or overrides** in the code.

## Transaction shape (by type)

- **Deposit** must have a target account and **must not** have a source account.
- **Withdrawal** must have a source account and **must not** have a target account.
- **Transfer** must have **both** a source and a target account, and they **must be
  different**. Same-account transfers are rejected both at the API edge (Joi custom rule) and in
  the domain (`Transaction` constructor).
- **Description is required**, 3–100 characters (Joi).

## Transaction lifecycle

- A transaction is created **PENDING**, then set to **COMPLETED** once balances are updated
  successfully, or **FAILED** on error.
- [unverified] **FAILED transactions are not persisted today** — the application services only
  save on the success path, so a failed attempt throws before any write (see A-4). The FAILED
  state exists in the model but does not reach the datastore.

## Intended but NOT enforced (gaps)

These were confirmed by the stakeholder as intended future rules; the code does **not** enforce
them today. Do not present them as active rules.

- [assumption] **Transaction / balance limits** — e.g. a per-transaction cap, a daily limit, or
  a minimum balance (A-6).
- [assumption] **Account types & lifecycle** — e.g. savings vs checking, and open/frozen/closed
  states (A-7).

## Cross-cutting gaps that affect rule integrity

- [risk] **Transfers are not atomic** (A-1): the debit and credit are saved separately with no
  database transaction, so a mid-way failure can break the no-overdraft/consistency guarantees.
- [risk] **No authentication/authorization** (A-2): none of the rules above are protected by
  access control — any caller can invoke any operation.
