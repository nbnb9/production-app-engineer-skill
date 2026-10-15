# Four-Role Engineering Workflow

Use this only when the user explicitly requests multiple agents, delegation, parallel work, or the architect/engineer/reviewer/optimizer workflow. If agent tooling is unavailable, perform the same role passes sequentially and disclose that.

## Coordination rules

The lead agent owns scope, integration, user communication, and final verification. Give each role a concrete brief, relevant repository paths, constraints, expected output, and a stopping condition.

Avoid concurrent edits to the same files. Parallelize independent investigation or clearly partitioned file ownership. Reviews should use the actual diff and test evidence, not the implementation plan alone.

## Roles

### Architect

Read-only first pass. Produce:

- product and technical assumptions;
- current or proposed system boundaries and data flow;
- data model and API/UI contracts;
- consequential tradeoffs, risks, and an incremental implementation plan;
- explicit file ownership suggestions for parallel work.

The architect should not create speculative infrastructure or rewrite the user's stack without evidence.

### Engineer

Implement the agreed smallest vertical slice. Own code, migrations, tests, configuration examples, and run instructions. Report deviations from the design and why they were necessary. Do not mark the task complete without exercising the critical path.

### Reviewer

Review the actual implementation independently for correctness, security, maintainability, migration safety, test quality, accessibility, and requirement coverage. Rank findings by severity and cite exact files and lines. Distinguish blockers from optional improvements. Do not rewrite code unless explicitly assigned a fix pass.

### Optimizer

Act after correctness is established. Inspect measured performance and operational cost, apply only evidence-backed changes, and record before/after results and tradeoffs. Also remove accidental complexity that does not alter behavior. Do not introduce a cache, queue, or new dependency without a clear bottleneck.

## Recommended sequence

```text
lead intake
  -> architect design
  -> lead resolves scope and partitions work
  -> engineer implementation
  -> reviewer independent review
  -> engineer fixes blockers
  -> optimizer measures and improves
  -> lead runs final quality gates and integrates
```

The reviewer and optimizer may investigate in parallel only after a stable implementation exists and when they do not edit overlapping files.

## Role handoff format

Each role returns:

1. evidence inspected;
2. conclusions or changes;
3. verification performed;
4. risks, open questions, or blockers;
5. exact files affected or recommended.

The lead reconciles disagreements using repository evidence and user requirements. Do not dump raw agent transcripts into the final response; synthesize architecture, implementation, review feedback, fixes, and measured optimization.

## Completion criteria

The workflow is complete only when reviewer blockers are resolved or explicitly accepted, optimizer claims have measurements or are labeled hypotheses, tests and builds pass at the appropriate scope, and the final diff matches the requested behavior.
