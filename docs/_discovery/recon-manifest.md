# Recon Manifest

> **Last updated:** 2026-07-20
> **Purpose:** Records what fed the recon so later runs can detect staleness. NOT an
> onboarding doc.

## Run info

- **Mode:** full
- **Scope:** whole repo (`bank-harness`)
- **Available inputs:** git history (yes — no domain signal) · code-intelligence/LSP server (no) · sub-agents (available, unused) · stakeholder (yes)
- **Navigation mode chosen:** grep + direct reads (small repo; every source file read)

## Existing docs read (Phase 0)

| Doc | Path | Notes |
|---|---|---|
| README | ./README.md | Setup + endpoint list; mostly accurate |
| Architecture | docs/architecture.md | Design-time doc; several structural claims drifted |
| Application services | docs/application-services.md | Inlines code that no longer matches implementation |
| Implementation plan | docs/implementation.md | Design-time doc / plan; inlines earlier code snapshots |

## Source files analysed (Phase 1)

Git blob hashes captured 2026-07-20 for the freshness check.

| Path | Hash (git blob) | Area |
|---|---|---|
| src/domain/entities/account.ts | a5e084538baef9ec69547e4eb2f80ba235592434 | data model / rules |
| src/domain/entities/transaction.ts | ebff1ada69f757c46e2e2e9dce75220fc80f9d25 | data model / rules |
| src/domain/valueObjects/money.ts | 3c3d33898570d712373a333a846b0fef14aca81f | data model / rules |
| src/domain/services/accountService.ts | 2ae1c8f1bbd99ee5c291c96acd9d3fbbe2e2425a | rules / workflow |
| src/application/services/accountApplicationService.ts | 92ff6f134341dd81d37a54e355a43d383577ad9d | workflow |
| src/application/services/transactionApplicationService.ts | 888280d07c7242c3a24553f5742df8b34e4baa83 | workflow |
| src/application/dtos/accountDto.ts | 6cfa8bbc1a69f58f2b6e16d6d1a2ff14cd97c518 | contract |
| src/application/dtos/transactionDto.ts | 56cb59ea7080db554bc99b33ca9477ebf56059fd | contract |
| src/api/controllers/accountController.ts | 2803553521271c88c9b1ac2e5c28e6370ccac39c | contract / edge |
| src/api/controllers/transactionController.ts | d25a94671ca4d105e1b6ebdb60d3c3377d2f18ed | contract / edge |
| src/api/routes/accountRoutes.ts | 9741f072c4d225dc5f2757573f37250810b06536 | contract / edge |
| src/api/routes/transactionRoutes.ts | 447ea3f0a88ae053f6825ee060bdd02c2bfb1835 | contract / edge |
| src/api/middleware/validation.ts | 779d830b938fa9d8c66f7071706ee268c0ae7337 | rules |
| src/api/middleware/errorHandler.ts | c759de90e09ef395163c796361138d2433cc233f | edge |
| src/api/swagger/swagger.ts | 65d2eede9da3fac07a77b341dfe9d543c414091e | contract |
| src/infrastructure/repositories/accountRepository.ts | 8b628e2f3104163680425d7c9f061a97e879ff6e | data access |
| src/infrastructure/repositories/transactionRepository.ts | e2f67e03fb41b2fd2c1872263c7891d5c693a97e | data access |
| src/infrastructure/schemas/accountSchema.ts | 9662c343a415dec786be0a36bc508200f24ac8ed | data model |
| src/infrastructure/schemas/transactionSchema.ts | ce1069b161fcc0d1dce03cf488222d3eef894835 | data model |
| src/infrastructure/database/connection.ts | 1c50d00ca68ea509b1329e998e9991dd5d5a7a2e | infra |
| src/infrastructure/config/config.ts | 3a8241e23f1b28c76754711ea5e0b1cad28bde38 | infra |
| src/server.ts | bde323d5dad71c79e7734c29e34796443b371515 | entry point |
| tests/unit/domain/entities/account.test.ts | d5f6be4181bf9bd38e070f54776cf0b7dfb00519 | behavioural spec |
| tests/unit/domain/entities/transaction.test.ts | baea589814667ed485675e8ee42fd685a461bbe9 | behavioural spec |

## Freshness check log

| Date checked | Files changed since last recon | Action taken |
|---|---|---|
| 2026-07-20 | n/a — first recon | full recon |
