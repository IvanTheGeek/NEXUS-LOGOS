# NEXUS Pipeline

How work flows through the NEXUS ecosystem from requirements to artifacts.

```
LOGOS (narrative .md + structured .toml records)
   │  Human + CORTEX capture facts → builds the Truth Graph (real SoT)
   ▼
Truth Graph
   │  Lens-independent graph of facts about the system
   ▼
ATLAS (modeling workspace — applies lenses to Truth Graph → Spec)
   │  Event Model lens → Actors, Commands, Events, Views, GWT
   │  FnHCI lenses   → interaction patterns, look/feel per output target
   │  Spec = Truth Graph projected through selected lenses
   ▼
FORGE (code generation — language-agnostic)
   │  Reads Spec + active lenses → source code in any target language
   ├── F# (primary — handlers, domain, services)
   ├── JavaScript / TypeScript
   ├── C++ / Rust / Haskell / other
   └── Configuration, schemas, documentation
   │  CORTEX generates; human reviews + promotes
   ▼
Artifact
   (web app, CLI, firmware, desktop app, service, library)
```

## Human Review Gates

Every arrow has a human review gate. Nothing auto-promotes from one stage to the next.

- LOGOS → ATLAS: human confirms requirements are complete and accurate before modeling begins
- ATLAS → FORGE: human reviews and finalizes the Spec before code generation (`status: finalized` in LOGOS)
- FORGE → Artifact: human reviews generated source code before promotion to build

## Development Loop

The loop that evolves NEXUS itself:

```
1. Define in LOGOS (markdown)
2. CORTEX drafts model + GWT (ATLAS session)
3. Identify repeated patterns
4. Extract into deterministic F# functions
5. CORTEX uses functions + spec instead of generating ad hoc
6. Apply to real domain
7. Find gaps (what couldn't be expressed? what repeated?)
8. Improve NEXUS (rules, types, functions)
```

Stability criterion: a phase is done when you can define a slice in markdown, represent it structurally, generate it deterministically, reuse the same pattern without redesign, and model a small real workflow end-to-end.
