# NEXUS Components

The NEXUS ecosystem consists of the following components. Each has a single responsibility and a defined relationship to the others.

| Component | Role |
|---|---|
| **NEXUS-EventModeling** | Shared F# vocabulary — Actor, Command, Event, View, Slice, GWT types. The Event Model lens in library form. |
| **NEXUS-STRATUM** | Event persistence — `IEventStore` interface with pluggable backends (InMemory, FileSystem, Dropbox, GoogleDrive, DuckDB, VectorDB). |
| **NEXUS-LOGOS** | This repo. NEXUS knowledge base — principles, architecture decisions, methodology documentation. |
| **NEXUS-ATLAS** | Modeling workspace — where human + CORTEX collaborate to build Specs by applying lenses to the Truth Graph. |
| **NEXUS-FORGE** | Language-agnostic code generator — reads Spec + active lenses, generates source code in F#, JS, C++, Rust, or any target language. |
| **CORTEX** | AI assistance layer — Claude, GPT, Gemini; external to NEXUS; reads LOGOS, drives ATLAS sessions, executes FORGE generation. |
| **NEXUS-admin** | Workspace scaffolding — templates, checklists, and bootstrap guides for new NEXUS projects. |
| **PAXUS** | Telemetry subsystem — captures real user sessions as runtime PATHs; always-on local ring buffer → IndexedDB via Service Worker; user can export session log for PATH replay in ATLAS. |

## External: FnTools Ecosystem

FnTools (`FnTools.online`) is a separate brand for technical, developer-facing, and reusable tooling. It is orthogonal to NEXUS — neither a NEXUS sub-project nor a Cheddar sub-brand. NEXUS and its app projects consume FnTools components as dependencies.

| FnTools Component | Role |
|---|---|
| **FnHCI** (`FnTools.FnHCI`) | Universal HCI abstraction — target-agnostic interaction model. Umbrella for all interaction modes. |
| **FnUI** (`FnTools.FnHCI.UI` / `FnTools.FnHCI.UI.Blazor`) | Visual system — Blazor-based (.NET 10) replacement for Bolero; the outward-facing UI brand. |
| **FnCLI** | Terminal/CLI interaction layer under FnHCI. |
| **FnAPI** | API interaction primitives and library line. |
| **FnA11y** | Accessibility layer under FnHCI. |
| **FnMCP** | MCP servers and tooling (FnMCP.Penpot, FnMCP.Nexus, etc.). |

Repo: `IvanTheGeek/FnTools` — contains `FnTools.FnHCI`, `FnTools.FnHCI.UI`, `FnTools.FnHCI.UI.Blazor`.
Full reference: `docs/methods/fnhci.md`

## Relationships

```
LOGOS (facts) → Truth Graph → ATLAS (applies lenses) → Spec
                                                           ↓
                                                        FORGE → Source code → Artifact
                                                           ↑
                                               EventModeling (lens vocabulary)
                                               FnTools/FnHCI (HCI lenses — external)
                                               STRATUM (persistence, wired in app)
```

CORTEX participates in ATLAS (modeling) and FORGE (generation) sessions. It reads LOGOS before every session.
