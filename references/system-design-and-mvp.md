# System Design and Greenfield MVP

Use this reference for a new application or when the user asks for system design plus implementation.

## Start from the product boundary

Define:

- primary user and job to be done;
- critical user journey and acceptance criteria;
- data sensitivity, tenancy, expected load, latency target, and availability need;
- required integrations, deployment environment, budget, and delivery constraints;
- explicit MVP non-goals.

If these are absent, make conservative assumptions that keep the design reversible. Ask only when a choice changes product semantics, regulatory exposure, or external cost.

## Choose the smallest viable architecture

Default to a modular monolith with one relational database and stateless application instances. Add a cache, queue, search engine, object storage, or service split only when a concrete workload or isolation requirement warrants it.

Typical boundaries:

```text
client -> web/API adapter -> application use cases -> domain rules
                                  |                    |
                                  +-> ports -> database/integrations
```

Keep transport models, persistence models, and domain concepts distinct when they evolve for different reasons. Avoid layers that merely rename values without enforcing a boundary.

## Design outputs

Before implementation, provide a compact design that maps directly to code:

1. **Context and containers:** users, external systems, application processes, data stores, and trust boundaries.
2. **Components:** ownership and public interfaces of major modules.
3. **Data flow:** happy path plus one important failure or retry path.
4. **Deployment:** build artifact, runtime processes, network exposure, secrets, migrations, health checks, logs, metrics, and rollback.
5. **Scale path:** the first measured bottlenecks that would justify replicas, caching, queues, partitioning, or service extraction.

## File structure

Adapt names to the selected framework. A useful baseline is:

```text
apps/
  web/                 frontend routes and composition
  api/                 transport and process entry point
packages/
  domain/              entities, policies, invariants
  application/         use cases and ports
  infrastructure/      database and integration adapters
  ui/                  reusable presentation components
db/
  migrations/
  seeds/
tests/
  integration/
  e2e/
ops/                   deployment and operational config
```

Do not force a monorepo or this exact structure on frameworks whose conventions already provide clear boundaries.

## Database design

- Model domain invariants with types, constraints, foreign keys, and uniqueness—not only application checks.
- Include stable primary keys, timestamps where operationally useful, and an explicit tenant/owner key for multi-tenant data.
- Index observed query predicates and joins. Do not create speculative indexes.
- Define deletion, retention, audit, and personally identifiable information handling.
- Use forward and rollback-safe migrations. For risky changes, use expand/migrate/contract rather than a breaking in-place deployment.
- Document transaction boundaries and concurrency behavior for state transitions.

Present the schema as real migration code plus a compact relationship summary. Seed only data needed for local development or tests.

## API design

For each endpoint or operation, specify method, path, authentication, authorization, request, success response, errors, idempotency, and pagination where applicable. Use consistent versioning and error envelopes.

Validate syntax at the adapter and business invariants in the use case or domain. Never trust client-supplied ownership, price, role, or state-transition fields. Bound list sizes and uploads. Add rate limits to abuse-sensitive or expensive operations.

## Cache and asynchronous work

Start without caching unless there is a latency or load reason. When a cache is justified, record:

- key shape and tenant isolation;
- source of truth;
- TTL and invalidation event;
- stale-data tolerance;
- stampede and outage behavior;
- metrics that show hit rate and correctness.

Use a queue for work that is slow, retryable, bursty, or should not block the request. Define idempotency, retry/backoff, poison-message handling, observability, and user-visible job state. Prefer an outbox pattern when database state and message publication must remain consistent.

## Implement the production minimum

A credible MVP includes:

- typed configuration with startup validation and `.env.example` without secrets;
- structured errors and logs with request or trace identifiers;
- authentication and least-privilege authorization when user data exists;
- database migrations and safe startup/deployment ordering;
- unit tests for core rules, integration tests for persistence/API boundaries, and one end-to-end critical flow;
- accessibility and responsive behavior for the primary UI;
- health/readiness checks and a minimal operations runbook;
- automated formatting, linting, type checking, tests, and reproducible builds.

Stub only truly external systems. Make stubs explicit and keep the critical local path runnable.

## Scale without premature distribution

Define the next step for each plausible pressure:

- application CPU: profile, optimize, then add stateless replicas;
- database reads: fix queries/indexes, then consider replicas or bounded caching;
- writes or contention: shorten transactions, batch, partition by ownership, or serialize hot aggregates;
- slow side effects: move to idempotent workers;
- large files: direct upload to object storage with scoped signed URLs;
- independent team or failure domains: extract a service only after the boundary is stable.
