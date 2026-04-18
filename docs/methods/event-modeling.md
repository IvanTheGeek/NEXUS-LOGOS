# Event Modeling — The Event Model Lens

Event Modeling is a methodology created by Adam Dymitruk for designing systems as a timeline of events. Within NEXUS, it is the primary lens applied to the Truth Graph for system design and behavior specification.

> Full F# vocabulary: `NEXUS-EventModeling` library (Actor, Command, Event, View, Slice, GWT types)
> Full methodology reference: `NEXUS-EventModeling/EVENT_MODELING.md`

## Core Elements

- **Actor**: source of decisions and actions — `Human of role`, `Automation of name`, `ExternalSystem of name`
- **Command**: intent expressed by an Actor; handled by a hidden pure CommandHandler; blue box
- **Event**: immutable fact, past tense, never changes; the backbone and only coupling point; orange box
- **View**: shaped data produced by a hidden EventHandler for an Actor to consume; green box

## Two Slice Types

- **CommandSlice**: `Actor → Command → Event(s)` — state change; data flows top-down
- **ViewSlice**: `Event(s) → View → Actor` — state view; data flows bottom-up

## GWT — Spec and Test in One

Given/When/Then is simultaneously the business specification and the test suite. Example data IS the tests.

- CommandSlice GWT: Given = prior events (state), When = command, Then = resulting events
- ViewSlice GWT: Given = events, When = filter/sort criteria, Then = shaped view

## Grouping Hierarchy

- **Flow**: short sequence of slices for one focused interaction
- **Workflow**: series of Flows or nested Workflows for a larger process
- **Area**: top-level grouping of related Workflows

## Key Rules

- Handlers are hidden and pure — no side effects, no I/O, no state
- Slices are immutable once finalized — replaced, never modified
- No code shared between slices — each owns its logic entirely
- The Event Model is a lens — it is not the Truth Graph itself
