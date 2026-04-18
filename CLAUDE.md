# NEXUS-LOGOS

> Workspace instructions and references: `../CLAUDE.md`, `../FSHARP.md`, `../WORKFLOW.md`

## Purpose

The NEXUS knowledge base. This repo is where NEXUS knows what it knows about itself.

- `docs/` — narrative documentation (`.md`): principles, architecture, methods, roadmap
- `records/` — structured facts (`.toml`): concepts, decisions (ADRs), slice specifications

CORTEX (Claude, GPT, etc.) reads this repo to understand NEXUS before starting any session.
Human and CORTEX write to this repo when decisions are made, patterns are discovered, or the architecture evolves.

## Conventions

- `.md` files: narrative, human + AI readable
- `.toml` files: structured records, machine-parseable
- One concept per file — small, focused files over large documents
- ADRs in `records/decisions/` are numbered sequentially: `001-...`, `002-...`
- Never delete — deprecate with `status = "superseded"` and point to the replacement

## Stack

Documentation and structured records only. No code.
