# Existing Codebase Engineering

Use this reference for architecture discovery, behavior-preserving refactors, production debugging, clean-architecture improvements, and performance work.

## Build an evidence-based model

Inspect in this order, adapting to the repository:

1. repository instructions, manifests, build/test scripts, and deployment files;
2. process entry points, routes or handlers, background workers, and scheduled jobs;
3. a representative request or event from boundary to database and integrations;
4. schema and migrations, configuration, authorization, and error handling;
5. tests that define current behavior;
6. recent or relevant history only when it helps explain intent.

Summarize the system as runtime processes, module boundaries, data ownership, and principal flows. State what was directly observed and what remains inferred.

## Architecture and maintainability review

Look for concrete symptoms rather than labeling patterns in isolation:

- modules with multiple reasons to change;
- domain rules duplicated across UI, handlers, workers, or persistence code;
- dependency cycles or business logic coupled to framework globals;
- inconsistent validation, authorization, errors, transactions, or configuration;
- hidden side effects, temporal coupling, shared mutable state, or brittle initialization;
- schema drift, unsafe migrations, N+1 queries, or missing ownership filters;
- tests that over-mock implementation details or omit critical behavior;
- dead abstractions, parallel utilities, and copied components.

For every issue, give evidence, impact, affected paths, and a bounded remedy. Rank by correctness and operational risk before style.

## Behavior-preserving refactor

1. Define the observable contract: inputs, outputs, errors, side effects, ordering, and performance constraints.
2. Add characterization tests at stable boundaries when coverage is weak.
3. Choose a seam and move one responsibility at a time.
4. Keep compatibility adapters during transition; remove them only after all callers move.
5. Avoid mixing broad formatting or dependency upgrades with structural changes unless required.
6. Re-run focused tests after each step and the broader suite before delivery.

Clean architecture is a direction, not a folder-count target. Create boundaries around business rules, unstable integrations, and independently testable use cases. Do not add interfaces that have only one implementation and no testing or volatility benefit.

## Production debugging

Use the chain:

```text
symptom -> reproducible trigger -> failing state transition -> violated invariant -> root cause
```

### Investigation

- Establish impact, frequency, affected versions or tenants, and whether the issue is ongoing.
- Reproduce with the smallest realistic input. Preserve logs, traces, payload shapes, and timestamps needed to compare runs.
- Follow data and control flow; identify the first point where actual state diverges from expected state.
- Form falsifiable hypotheses and test the cheapest discriminating evidence first.
- Check boundaries: null/empty/large values, time zones, locale, concurrency, retries, partial failures, stale caches, permissions, and version skew.

### Fix

Patch the earliest safe point that restores the invariant. Add a regression test that fails for the original trigger. Consider existing corrupted data, in-flight jobs, cache contents, backward compatibility, rollout order, and rollback.

Do not hide an error with a retry, default, broader exception handler, or null guard unless that behavior is the intended contract.

### Debugging output

Report what the code does, the observed failure, the causal mechanism, contributing conditions, edge cases, the fix, and verification. Separate root cause from impact and from incidental code smells.

## Performance engineering

Optimize only after defining a workload and capturing a baseline. Measure wall time, throughput, error rate, CPU, memory, I/O, query count, payload size, or render count as relevant.

Investigate by layer:

- **Database:** query plans, N+1 access, missing or unused indexes, over-fetching, lock waits, transaction length, connection pool pressure.
- **Backend:** algorithmic complexity, repeated serialization, blocking I/O, unbounded concurrency, allocation pressure, duplicate remote calls.
- **Network/API:** waterfall requests, oversized payloads, missing pagination or compression, poor cacheability.
- **Frontend:** render frequency, unstable props, large bundles, long tasks, layout thrashing, unvirtualized lists, duplicate fetches.
- **Cache/queue:** hit rate, hot keys, stampedes, retry storms, backlog age, oversized messages.

Fix the dominant cost before micro-optimizing. Preserve correctness with tests, then repeat the same benchmark and report the comparison. Include tradeoffs such as memory growth, staleness, complexity, or weaker consistency.

## Refactor and performance deliverables

- Architecture summary and principal data flows.
- Findings ordered by severity, with exact evidence.
- Incremental strategy with safe checkpoints and rollback.
- Implemented code and tests when requested.
- Before/after measurements for performance changes, or an explicit measurement plan if the environment cannot produce them.
