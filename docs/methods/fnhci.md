# FnHCI — Functional Human-Computer Interaction

FnHCI is part of the **FnTools** ecosystem (`FnTools.online`), a separate brand for
technical, developer-facing, and reusable tooling. FnHCI is not a NEXUS component —
it is an external dependency that NEXUS and its app projects consume.

> Repo: `IvanTheGeek/FnTools`
> Namespace: `FnTools.FnHCI`

## FnTools Brand Context

FnTools is orthogonal to NEXUS and to Cheddar. Both can use FnTools components.
The family of products under FnTools:

| Name | Role |
|---|---|
| **FnHCI** | Universal HCI abstraction — umbrella for all human-computer interaction modes |
| **FnUI** | Visual system brand — `FnTools.FnHCI.UI` / `FnTools.FnHCI.UI.Blazor`; Bolero replacement |
| **FnCLI** | Terminal/CLI interaction under FnHCI |
| **FnAPI** | API interaction primitives and library line |
| **FnA11y** | Accessibility layer under FnHCI |
| **FnMCP** | MCP servers and tooling (FnMCP.Penpot, FnMCP.Nexus, etc.) |

Domain: `FnTools.online`

## Namespace Structure

```
FnTools.FnHCI           — interaction primitives and shared abstractions (the umbrella)
FnTools.FnHCI.UI        — visual view/state/composition model
FnTools.FnHCI.UI.Blazor — Blazor host/runtime adapter (.NET 10)
```

Outward-facing package/brand names: `FnUI`, `FnUI.Blazor`

## Core Idea

FnHCI sits between the Spec (produced by ATLAS) and any specific output target.
A Spec flows through a FnHCI lens pipeline:

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
| Blazor (FnUI.Blazor) | .NET 10 Blazor components — event-sourced; NEXUS's replacement for Bolero |
| TUI / FnCLI | Terminal UI — key bindings, character layout, color codes |
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

## Relationship to NEXUS-FORGE

FORGE generates FnHCI components from Specs. The active lenses in a Spec determine which
FnHCI adapters FORGE targets. A Spec with the Blazor lens active → FORGE generates
`FnTools.FnHCI.UI.Blazor` components.
