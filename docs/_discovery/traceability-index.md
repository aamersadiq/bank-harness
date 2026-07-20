# Traceability Index

> **Last updated:** 2026-07-20
> **Purpose:** Maps each substantive claim in the docs to its evidence. Keeps citations out of
> the onboarding prose while preserving the audit trail. NOT an onboarding doc.

| Claim ID | Claim (short) | Evidence (path:line / stakeholder) | Confidence | Used in doc |
|---|---|---|---|---|
| C-1 | Three transaction types: DEPOSIT, WITHDRAWAL, TRANSFER | src/domain/entities/transaction.ts:7-11 | H | domain-model, domain-glossary, business-rules |
| C-2 | Transaction lifecycle PENDING→COMPLETED/FAILED | src/domain/entities/transaction.ts:16-20,127-133 | H | domain-model, business-rules, workflows |
| C-3 | Deposit/withdrawal amount must be positive (>0); zero rejected | src/domain/entities/account.ts:52,60; tests/unit/domain/entities/account.test.ts:36-38,51-53 | H | business-rules |
| C-4 | Withdrawal blocked when balance < amount (no overdraft) | src/domain/entities/account.ts:63-65; src/domain/services/accountService.ts:60-62 | H | business-rules, workflows |
| C-5 | canWithdraw = amount>0 && balance>=amount | src/domain/entities/account.ts:70-72; tests/.../account.test.ts:60-79 | H | business-rules |
| C-6 | Money rounds to 2 decimal places | src/domain/valueObjects/money.ts:8-11 | H | domain-model, business-rules |
| C-7 | Deposit must have toAccountId and no fromAccountId | src/domain/entities/transaction.ts:64-71; tests/.../transaction.test.ts:97-119 | H | business-rules |
| C-8 | Withdrawal must have fromAccountId and no toAccountId | src/domain/entities/transaction.ts:72-79; tests/.../transaction.test.ts:121-143 | H | business-rules |
| C-9 | Transfer must have both accounts and they must differ | src/domain/entities/transaction.ts:80-87; src/api/middleware/validation.ts:96-101; tests/.../transaction.test.ts:169-179 | H | business-rules |
| C-10 | Account created with zero balance by default | src/domain/entities/account.ts:17; src/domain/services/accountService.ts:13-15; tests/.../account.test.ts:20-23 | H | domain-model, business-rules, workflows |
| C-11 | Account balance cannot be negative (schema constraint) | src/infrastructure/schemas/accountSchema.ts:24-27 | H | business-rules |
| C-12 | Persisted amount min 0.01 (schema constraint) | src/infrastructure/schemas/transactionSchema.ts:28-31 | H | business-rules |
| C-13 | Input validation: name 3–50, description 3–100, amount positive precision 2 | src/api/middleware/validation.ts:6-104 | H | business-rules, business-requirements |
| C-14 | Layered DDD: API→Application→Domain→Infrastructure | src/** directory structure | H | current-architecture |
| C-15 | App services delegate transaction logic to domain AccountService | src/application/services/transactionApplicationService.ts:24,47,69,105 | H | current-architecture |
| C-16 | Persistence is MongoDB via Mongoose | src/infrastructure/database/connection.ts, schemas/* | H | current-architecture, integrations |
| C-17 | Domain UUID stored in custom `uuid` field, separate from Mongo _id | src/infrastructure/schemas/accountSchema.ts:29-33; repositories/accountRepository.ts:22,38-51 | H | current-architecture, domain-model |
| C-18 | REST endpoints: 4 account + 3 transaction | src/api/routes/accountRoutes.ts:60-64; src/api/routes/transactionRoutes.ts:71-74 | H | current-architecture, workflows, business-requirements |
| C-19 | API responses wrapped in { success, data } / { success, message } | src/api/controllers/accountController.ts:35-45; transactionController.ts:46-56 | H | current-architecture |
| C-20 | findByAccountId matches transactions where account is source OR target | src/infrastructure/repositories/transactionRepository.ts:29-38 | H | domain-model, workflows |
| C-21 | Middleware: cors, helmet, morgan, express.json | src/server.ts:20-23 | H | current-architecture |
| C-22 | Transfers are non-atomic (two separate saves, no session) | src/application/services/transactionApplicationService.ts:103-117 | H | current-architecture, business-requirements (risk A-1) |
| C-23 | No auth implemented; bearerAuth declared but unused | src/api/swagger/swagger.ts:26-34; no auth in server.ts/routes | H | current-architecture, business-requirements (risk A-2) |
| C-24 | Purpose: practice/demo harness, not production | Stakeholder (Aamer Sadiq, 2026-07-20) | H | README, business-requirements, personas |
| C-25 | Unimplemented intended rules: limits, account types/lifecycle | Stakeholder (2026-07-20) | M | business-requirements (assumptions A-6, A-7) |
