# Domain Glossary

> **Last updated:** 2026-07-20
> **Scope:** Business language used in the banking service
> **Mode:** full
> **Status:** accepted knowledge unless flagged — see ../_discovery/assumptions-register.md

| Term | Meaning | Also known as / code name | Related |
|---|---|---|---|
| Account | A bank account with a name and a balance that money moves in and out of | `Account`, stored as the `Account` collection | Balance, Transaction |
| Balance | The current amount of money held in an account; cannot go negative | `Account.balance` (a `Money`) | Money, Overdraft |
| Money | An amount of money, always rounded to 2 decimal places; no currency attached | `Money` value object | Amount |
| Amount | The size of a single transaction; must be positive | `Transaction.amount` | Money |
| Transaction | An immutable record of one money movement | `Transaction` | Deposit, Withdrawal, Transfer |
| Deposit | Money paid **into** one account (no source account) | `TransactionType.DEPOSIT` | Transaction |
| Withdrawal | Money taken **out of** one account (no target account) | `TransactionType.WITHDRAWAL` | Transaction, Overdraft |
| Transfer | Money moved from one account **to another** (both required, must differ) | `TransactionType.TRANSFER` | Transaction |
| Source account | The account money leaves; set for withdrawals and transfers | `fromAccountId` | Withdrawal, Transfer |
| Target account | The account money enters; set for deposits and transfers | `toAccountId` | Deposit, Transfer |
| Transaction status | Where a transaction is in its lifecycle | `TransactionStatus` = PENDING / COMPLETED / FAILED | Transaction lifecycle |
| Overdraft | Withdrawing more than the balance — **not allowed**; rejected as "Insufficient funds" | (no code term; enforced by balance check) | Balance, Withdrawal |
| Insufficient funds | The error raised when a withdrawal or transfer exceeds the source balance | error string `'Insufficient funds'` | Overdraft |
