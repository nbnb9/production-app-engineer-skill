# Production Quality Gates

Use these gates before handing off implementation. Apply proportionally: a local component does not require a distributed-systems review, but every changed trust boundary needs validation and security consideration.

## Requirement coverage

- Map each acceptance criterion to code and a verification step.
- Exercise the main user journey and one important failure path.
- Confirm explicit non-goals were not accidentally expanded.

## Correctness and data

- Types, schemas, and boundary validation agree.
- Invariants are enforced in the appropriate layer and, when possible, in the database.
- Transactions cover atomic state changes; retries are idempotent where needed.
- Migrations are reviewed for existing data, locks, rollout order, backward compatibility, and rollback.
- Time zones, locale, empty values, large values, duplicates, and concurrency are considered where relevant.

## Security and privacy

- Authentication and authorization are distinct and tested; ownership is checked server-side.
- Inputs, uploads, redirects, query construction, and rendered content are handled safely.
- Secrets and sensitive values are absent from code, logs, errors, fixtures, and client bundles.
- Rate limits and abuse controls exist on sensitive or expensive operations.
- Personal data collection, retention, export, and deletion match the product need.
- Dependencies and generated artifacts do not introduce known unnecessary risk.

## Reliability and operations

- Errors are structured, actionable, and do not leak internals.
- Timeouts, cancellation, retry limits, backoff, and degraded behavior are defined for remote calls.
- Health/readiness behavior reflects real dependencies without causing restart loops.
- Logs have useful correlation fields; key success, failure, latency, and queue metrics are observable.
- Deployment and rollback order are documented for changes that span code and schema.

## Frontend and accessibility

- Loading, empty, error, retry, disabled, and permission states work.
- Keyboard navigation, focus, labels, semantic structure, contrast, zoom, reduced motion, and narrow screens are checked as applicable.
- Async actions resist double submission and communicate progress and completion.
- No avoidable request waterfalls, repeated renders, or oversized payloads were introduced.

## Engineering hygiene

- Formatting, linting, type checking, focused tests, broader tests, and production build run as available.
- New behavior has tests at the lowest stable boundary plus integration coverage for important seams.
- Generated files, debug output, temporary credentials, and unrelated edits are excluded.
- Configuration examples and run commands match the implementation.

## Evidence table

Summarize verification compactly:

| Check | Command or method | Result |
| --- | --- | --- |
| Critical flow | test name or manual path | pass/fail/not run |
| Static checks | lint/type command | pass/fail/not available |
| Data migration | dry run or review | pass/fail/not applicable |
| Build | production build command | pass/fail/not run |
| Performance | benchmark/profile | before -> after/not measured |
| Accessibility | keyboard/automated/manual | pass/issues/not applicable |

Never convert “not run” into “pass.” Explain environmental blockers and the smallest way to complete the missing check.
