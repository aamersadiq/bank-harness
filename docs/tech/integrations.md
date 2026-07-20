# Integrations & Dependencies

> **Last updated:** 2026-07-20
> **Scope:** External systems and dependencies of the banking service
> **Mode:** full
> **Status:** accepted knowledge unless flagged — see ../_discovery/assumptions-register.md

The service has **one external system**: its own MongoDB datastore. There are no third-party
APIs, message brokers, identity providers, payment rails, or data feeds.

## External systems

| System | Purpose | Direction | Protocol / mechanism | Notes |
|---|---|---|---|---|
| MongoDB | Primary datastore for `Account` and `Transaction` collections | outbound | Mongoose driver over TCP (`MONGO_URI`) | Run locally via docker-compose (service `mongo`). On connection failure at startup, the process exits (`process.exit(1)`). Critical — nothing works without it. |
| mongo-express | Web admin UI for MongoDB (dev convenience) | n/a | HTTP on `:8081` | Optional; from docker-compose. Not used by the app at runtime. |

## Key dependencies

- **Express** — HTTP framework and routing.
- **Mongoose** — MongoDB ODM; defines schemas and performs all persistence.
- **Joi** — request-body validation at the API edge.
- **swagger-jsdoc + swagger-ui-express** — generate and serve API docs from JSDoc annotations.
- **helmet, cors, morgan** — security headers, CORS, request logging.
- **uuid** — generates domain entity ids.
- **dotenv** — loads configuration from `.env`.

## Data feeds

None — no scheduled imports/exports, batch jobs, or file drops.

## Failure & coupling notes

- If MongoDB is unavailable at **startup**, the app logs the error and exits. There is no
  retry/backoff.
- There are **no retries, circuit breakers, or fallbacks** for datastore operations at request
  time — a Mongo error surfaces as a 500 (or a 400 for domain errors).
- [risk] Because transfers issue multiple independent writes without a transaction, a mid-write
  Mongo failure can leave data inconsistent (A-1).
