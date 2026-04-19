# NEXUS

NEXUS is not a brand. It is a concept — a way of approaching information, knowledge,
and software development. Everything Ivan builds falls under this concept. The name
represents the idea that all work is connected: methodology, tooling, applications,
and the systems that generate and validate them are all part of one ecosystem.

NEXUS-prefixed repos (NEXUS-EventModeling, NEXUS-STRATUM, NEXUS-LOGOS, etc.) are
the infrastructure layer of this ecosystem. FnTools, CheddarBooks, LaundryLog, and
other projects are distinct brands or products built on and validated by this
infrastructure. CORTEX (Claude, GPT, etc.) reads NEXUS knowledge before every session
so that any AI entering the ecosystem understands the context without re-explanation.

---

# NEXUS Principles

## 1. Truth Graph is the Source of Truth

The Truth Graph is the fundamental, lens-independent representation of everything known about a system. All other views — the Event Model, the UI spec, the data model, the API contract — are lenses applied to the Truth Graph.

No single lens is the truth. Every lens is a projection.

## 2. Lenses, Not Layers

A lens is a projection of the Truth Graph for a specific purpose. Multiple lenses can be active simultaneously. Adding a lens does not change the truth — it reveals a new aspect of it.

The Event Model (Adam Dymitruk) is a lens. FnHCI targets (HTML, Blazor, TUI) are lenses. API contracts are lenses. Data models are lenses.

## 3. Determinism Over Cleverness

AI (CORTEX) fills gaps. Functions own stable patterns.

When a pattern repeats 2–3 times, extract it into a deterministic F# function. The goal is to progressively replace freeform AI generation with typed, deterministic builders. The system becomes more reliable as patterns stabilize.

## 4. Markdown Converges

Markdown is the starting point, not the end state. Every piece of narrative documentation should converge toward structured records, typed specifications, or deterministic functions. Markdown that never converges is not useful.

## 5. Human Review Gates

Nothing auto-promotes. Every transition in the pipeline (LOGOS → ATLAS, ATLAS → FORGE, FORGE → artifact) requires human review and explicit promotion. The system assists and generates; the human decides and approves.

## 6. Applications Are Validation

Applications (LaundryLog, LonnysCardGame, CheddarBooks) are not just deliverables. They are stress tests, pattern generators, and gap detectors for NEXUS itself. Every application is a chance to find what NEXUS cannot express.

## 7. NEXUS Models NEXUS

The ultimate validation is for NEXUS to model and generate its own components using the NEXUS pipeline. ATLAS, FORGE, and FnHCI should each be producible by the system they are part of.
