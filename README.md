# Production App Engineer Skill

A production-oriented Codex skill for full-stack application design, implementation, codebase discovery, refactoring, debugging, performance optimization, clean architecture, reusable UI components, and explicit four-role engineering workflows.

## Install

```bash
git clone https://github.com/nbnb9/production-app-engineer-skill.git ~/.codex/skills/production-app-engineer
```

Restart or open a new Codex session after installation so the skill is discovered.

## Use

```text
Use $production-app-engineer to design and implement a production-ready MVP for my product.
```

```text
Use $production-app-engineer to understand this codebase, preserve behavior, and refactor it safely.
```

```text
Use $production-app-engineer with the architect, engineer, reviewer, and optimizer workflow.
```

## Structure

```text
production-app-engineer/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── codebase-engineering.md
    ├── four-role-workflow.md
    ├── quality-gates.md
    ├── system-design-and-mvp.md
    └── ui-components.md
```

The entrypoint routes each request to only the references needed for that operating mode, keeping context focused while preserving production quality gates.
