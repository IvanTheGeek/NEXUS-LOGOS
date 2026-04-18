# FnHCI — Functional Human-Computer Interaction

FnHCI is a NEXUS component that defines a universal HCI abstraction — a target-agnostic model of human-computer interaction with adapters for every concrete output medium.

> Library: `NEXUS-FnHCI`

## Core Idea

FnHCI sits between the Spec (produced by ATLAS) and any specific output target. A Spec flows through a FnHCI lens pipeline:

```
Spec (core Event Model)
   → FnHCI abstract interaction model (Actor actions, view renders)
      → Target adapter (HTML, Blazor, TUI, LCD, ...)
         → Rendered artifact
```

Adding a new output target = writing a new adapter. The core and all other adapters are untouched.

## Targets

| Adapter | Output |
|---|---|
| HTML | Bare HTML, CSS — browser |
| Blazor | .NET 10 Blazor components — event-sourced; NEXUS's own replacement for Bolero |
| TUI | Terminal UI — key bindings, character layout, color codes |
| LCD | Embedded display — character constraints, line width, raster limits |
| Receipt printer | Receipt format — line width, character set, cut commands |
| Mobile | Touch targets, gesture model, platform conventions |
| Desktop | Window chrome, native controls, menus |

## Design Principle

Every time a UI/HCI pattern repeats 2–3 times → extract into a deterministic F# function.

```fsharp
// AI designs the interaction spec. Functions build the component.
val buildCommandSlice : ThemeTokens -> CommandSliceLayout -> 'TOutput
val buildViewSlice    : ThemeTokens -> ViewSliceLayout    -> 'TOutput
val buildFormRegion   : InputSpec list                    -> 'TOutput
val buildActionBar    : ActionSpec list                   -> 'TOutput
```

`'TOutput` is determined by the active adapter. The same function signature works across all targets.

## Relationship to Event Modeling

The Event Model lens maps naturally to FnHCI:

| Event Model | FnHCI meaning |
|---|---|
| CommandSlice | An interaction where the Actor issues intent (form, button, voice command, key press) |
| ViewSlice | A display where the Actor reads state (screen, printout, terminal output) |
| GWT Given | Preconditions for the interaction |
| GWT When | The Actor's action |
| GWT Then | The resulting state change and display update |
