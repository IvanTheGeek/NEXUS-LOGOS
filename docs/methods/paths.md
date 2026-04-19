# NEXUS — PATHs

**Author:** Ivan (IvanTheGeek)
**License:** CC BY-SA 4.0

---

## What a PATH Is

A PATH is a **named, purposeful journey through the Event Model from a specific actor's perspective.**

PATHs are not:
- Informal user story descriptions
- UML use cases
- Navigation flows
- Test scripts (though they become one)

PATHs **are** typed design artifacts. They exist in LOGOS, are linked to the Event Model elements they traverse, and have their own lifecycle. They are first-class citizens of NEXUS, not documentation byproducts.

---

## PATH Structure

Every PATH has:

| Field | Description |
|---|---|
| **Name** | Meaningful to the domain. Not "flow 3" — "Driver submits monthly expense report" |
| **Actor** | The swim lane persona this journey belongs to |
| **Goal** | What the actor is trying to accomplish. PATH ends when goal is achieved or definitively not achieved |
| **Sequence of slices** | Specific command and projection slices traversed, in order |
| **Example data** | Concrete, realistic values that flow through every slice in the PATH |
| **Status** | Lifecycle state (see below) |

---

## Example Data

**PATHs are the source of all example data in NEXUS.**

Example data does not exist independently — it belongs to a PATH. When you define a PATH, you define the concrete values that travel through it. Those values then flow downstream into GWT test cases, ATLAS path walkthroughs, LOGOS topics, and eventually runtime verification. There is no shared "test data pool" separate from PATHs. If you want example data, you define a PATH.

Adam Dymitruk's Event Modeling prescribes sample data in the model. NEXUS extends this: **example data is integral to every PATH, not optional decoration.**

Example data is concrete and realistic — not abstract placeholders:

> `location='Pilot Travel Center', machine='Washer', qty=2, price=$3.75, payment='Card'`

Not:

> `username='user1', amount='123'`

### Why example data matters

**Communication** — concrete examples create shared vocabulary. When someone says "the truck stop path," the example data makes it unambiguous. No interpretation needed.

**Design validation** — working through real values during design reveals gaps and edge cases that abstract modeling misses. If example data can't flow cleanly through every slice, the design has a problem.

**Test data** — the same example data used during design becomes the given-when-then test cases. The `given` clause establishes state using PATH example data. The `when` clause executes commands with PATH example data. The `then` clause asserts events and state match the PATH.

> **Design and test are the same artifact.** There is no separate test specification.

---

## The PATH Registry vs. Minimal PATH Coverage

There are two distinct but related concepts that both involve PATHs. They serve different purposes and should not be confused.

### The PATH Registry — all known PATHs

The PATH registry is the complete collection of every PATH the system knows about, regardless of source or status. This includes:

- **Designed PATHs** — authored intentionally during Event Modeling, covering known branches
- **Discovered PATHs** — surfaced through choose-your-own-adventure exploration in ATLAS, where a viewer's ad hoc traversal reveals a branch that wasn't explicitly modeled
- **Telemetry PATHs** — real user sessions submitted via PAXUS that represent paths users actually took through the system, including paths nobody designed for

All of these are valuable. A PATH in the registry that came from user telemetry is evidence that users are taking a journey someone hadn't anticipated. A PATH discovered through ATLAS exploration is a gap in the original model being surfaced. None of this gets discarded — it all goes into the registry and informs future design decisions.

PATHs in the registry have varying levels of formalization. Some are fully specified with example data and GWT test cases. Others are raw observations — a recorded traversal waiting for a human to decide whether it warrants a named PATH with full lifecycle treatment.

### Minimal PATH Coverage — the official test set

Minimal PATH coverage is a curated **subset** of the PATH registry. It is the smallest collection of PATHs that achieves full branch coverage of the system — one PATH per unique branch, no redundancy.

This is the set that gets wired into automated UI testing (e.g., Playwright). Not every PATH in the registry. Just the minimal set needed to know the system works.

| PATH | Unique Branch Covered |
|---|---|
| Happy path — single entry | Basic flow end-to-end |
| GPS auto-fill | Location lookup branch |
| Multiple quantity | qty > 1, entry total calculation |
| Validation failure | Missing required field |
| Quick-fill price | Price preset selection |
| Second entry | Session total accumulation |

Every PATH in the minimal set justifies its existence by covering a branch nothing else covers. Adding a PATH to the minimal set is a deliberate human decision — not automatic.

### The relationship between them

```
PATH Registry (everything known)
  └── Minimal Coverage Set (curated subset → Playwright / official tests)
```

The registry grows continuously. The minimal coverage set is managed deliberately. When a new PATH in the registry reveals a branch not covered by the current minimal set, a human decides whether to promote it and author the necessary example data and GWT specs.

---

## ATLAS — PATH Visualization and Exploration

ATLAS is the modeling studio in NEXUS. It is where PATHs are visualized, explored, and walked.

### Predefined PATH mode

Select a PATH from the registry; ATLAS navigates through it with sample data pre-filled at each step. Used for: verifying implementations, onboarding new contributors, demonstrating behavior, running walkthroughs as documentation.

### Choose your own adventure mode

Freeform exploration through the Event Model. At each slice, the viewer sees all available branches and picks one. The system follows the choice forward and presents the next decision point. Used for: learning the model, exploring edge cases, understanding system behavior without following a scripted route.

In choose-your-own-adventure mode, the viewer is effectively **composing a new PATH on the fly** — their sequence of choices is itself a traversal. That traversal can be saved as a named PATH in the registry if it reveals something worth capturing.

### PATH walkthrough — NEXUS's own prototype surface

ATLAS includes its own linear PATH walkthrough view: one screen per step, one action per screen, sample data visible at each step. This is the equivalent of what a Penpot linear prototype would have provided — a screen-by-screen walkthrough of a specific PATH, authored and maintained within NEXUS rather than in an external design tool.

### Fidelity levels

ATLAS supports selectable fidelity levels. Different viewers enable the detail level relevant to them:

- **Executive** — happy path only; shows business needs. Closest to Adam Dymitruk's original intent.
- **UX** — screen interactions and actor touchpoints
- **Domain** — shipping clerk or end-user workflow focus
- **Technical** — implementation details, processor internals, projection dependencies

One model. Many lenses. PATHs exist at all fidelity levels simultaneously.

---

## Default PATHs

Every system modeled in NEXUS will have a set of default PATHs — PATHs that are automatically surfaced based on who is viewing the model and what context they're in.

**Default PATHs are not hardcoded.** They are resolved at runtime by matching the viewer's Actor type and fidelity level against the PATH registry.

| Viewer | Default PATH(s) |
|---|---|
| **Executive / Stakeholder** | Happy path for each major actor — shows what the system does at the business level |
| **New contributor / Onboarding** | Guided happy path with full example data — walk before you run |
| **Developer** | All implemented PATHs, sorted by domain area — complete branch coverage view |
| **QA / Tester** | All verified + unverified PATHs — shows what's been confirmed and what hasn't |
| **Support** | PATHs by actor + failure branches — where do users get stuck |
| **Domain expert** | PATHs filtered to their actor swim lane only |

### Happy path as universal default

The happy path — the primary success scenario for each actor — is always the starting default. It shows the system working as intended, with no failures, no edge cases. Every other PATH branches from or relates to the happy path.

### PATH sets

A collection of related default PATHs can be grouped into a **PATH set** — a named, curated bundle for a specific audience:

- `onboarding-set` — the 3–4 PATHs a new contributor walks to understand the system
- `executive-set` — the happy paths for each major actor, nothing else
- `full-coverage-set` — all PATHs in the minimal coverage set, used for regression verification

PATH sets are defined in LOGOS and referenced by ATLAS.

---

## PATH Lifecycle

```
Identified → Modeled → Specified → Implemented → Verified
```

| State | Meaning |
|---|---|
| **Identified** | Named. Actor and goal defined. Slices not yet fully modeled. |
| **Modeled** | Traced through the Event Model. Every slice identified. Example data defined. Gaps visible. |
| **Specified** | Slices have Spec topics in LOGOS. Ready for implementation. |
| **Implemented** | Slices are built. PATH can be traversed in the running system. |
| **Verified** | Tested end-to-end with example data. Actor can accomplish the goal. |

---

## PATH Chaining — Composed Journeys

Individual PATHs are atomic: one actor, one goal, one sequence. But real user journeys span multiple goals. A driver doesn't just log one expense — they open the app, log three expenses across two stops, review the session total, then close. That's not one PATH. It's several PATHs chained together.

**PATH chaining** is the composition of multiple atomic PATHs into a larger journey. Each PATH in the chain completes before the next begins. The output state of one PATH becomes the input context of the next.

### Why chaining matters

- **Realistic scenarios** — real usage is a sequence of goals, not a single isolated interaction
- **Integration testing** — a chain of PATHs is a natural integration test; each PATH is already unit-tested in isolation; the chain tests that they compose correctly
- **Demonstration and onboarding** — walking a new contributor through a chained scenario shows the system working as a whole
- **Runtime comparison** — PAXUS records the user's actual event log as a runtime PATH; chaining gives you the vocabulary to compare designed sequences against lived ones

### Chaining and example data

When PATHs chain, example data flows across the boundary. The end state of PATH A seeds the starting state of PATH B. This must be explicit in the chain definition: which facts produced by PATH A are consumed as given clauses in PATH B.

If the data doesn't connect cleanly, the chain has a gap. That gap is a design problem, surfaced early.

### Choose your own adventure as dynamic chaining

Choose-your-own-adventure mode in ATLAS is essentially **dynamic PATH chaining at exploration time**. The viewer's sequence of choices assembles a chain on the fly.

- Predefined chains = curated, authored sequences (demo script, onboarding walkthrough, regression suite)
- Choose-your-own-adventure = ad hoc chains composed interactively by the viewer

---

## Runtime PATHs — The User's Actual Event Log

PATHs aren't only design artifacts. **The user's actual event log IS a PATH.**

PAXUS (the telemetry subsystem) captures:
- Raw keystroke/mouse/system signals by default
- Always-on, local-only
- In-memory ring buffer → IndexedDB flush via Service Worker
- Aggregation layer converts raw signals to LOGOS Facts

When a user encounters a bug, they can export their full session log. That log is a PATH — a real traversal of the system. Loading it into ATLAS replays the sequence event by event:

> UI events → projections → processors → commands → exact failure point

This is **debugging as path replay**, not guessing. No "steps to reproduce." The developer observes exactly what happened.

Design PATHs = what *should* happen. Runtime PATHs = what *did* happen. The gap between them is where bugs live.

### User support via PATH replay

- User hits a problem → exports their session log
- Developer loads the exported PATH into ATLAS
- ATLAS replays the exact sequence
- The failure point is visible, not inferred

### INFLUANCE contribution

Users can contribute session data for improving the system:
- Choose fidelity level (full → interaction-level → aggregated)
- Apply optional anonymization before sharing
- Session data accrues INFLUANCE points in the contribution accounting system

---

## PATHs in LOGOS

Each PATH is a topic in the relevant project category in LOGOS. The topic contains:

- PATH definition (name, actor, goal, status)
- Example data
- Links to Event Model elements traversed
- Links to Spec topics for each slice
- Current lifecycle status

The PATH topic is the thread that connects **design intent to implementation reality.**

---

## Literal vs. Metaphorical Application

The word "PATH" is used in two distinct ways across NEXUS.

### Literal PATHs (Event Model PATHs)

The primary, technical definition. A PATH is a named sequence of Event Model slices traversed by a specific actor to accomplish a specific goal. It carries example data, has a lifecycle, generates test cases, and is stored in LOGOS.

Applies to: software system design (LaundryLog, etc.), ATLAS traversals, PAXUS event logs.

### Metaphorical PATHs (Navigation PATHs)

An extended application of the same concept to content navigation. When ivanthegeek.com uses "PATHs," it means a curated journey through a content graph — not through Event Model slices. The visitor selects an Actor type and intent; the server assembles a sequence of markdown documents that forms a coherent journey.

| | Literal PATH | Navigation PATH |
|---|---|---|
| **Traverses** | Event Model slices | Markdown content nodes |
| **Example data** | Concrete domain values | N/A (content is the data) |
| **Test cases** | GWT specs derived from data | N/A |
| **Storage** | LOGOS topic | Frontmatter + URL state |
| **Runtime** | PAXUS event log | URL path params |
| **Viewer** | ATLAS | Oxpecker-rendered site |

**Rule of thumb:** if the PATH traverses Event Model slices → literal. If it traverses content nodes → metaphorical.

---

## Key Relationships

```
Event Model
  └── Slices (Command + Projection)
        └── PATHs traverse slices in sequence
              ├── Example data sourced from PATHs → flows through every slice
              ├── Example data → GWT test cases (design = test)
              ├── PATH Registry (all known: designed, discovered, telemetry)
              │     └── Minimal Coverage Set (curated subset → Playwright / official tests)
              ├── ATLAS visualizes, walks, and explores PATHs
              │     ├── Predefined PATH mode (playback with sample data)
              │     ├── Choose-your-own-adventure (dynamic chaining, saveable)
              │     └── PATH walkthrough view (linear screen-by-screen)
              ├── LOGOS topic tracks lifecycle per PATH
              ├── Runtime: user event log = PATH replay (PAXUS)
              └── PATHs chain together
                    ├── Output state of PATH A → input context of PATH B
                    ├── Predefined chains = authored sequences (demo, onboarding, regression)
                    └── Ad hoc chains = choose-your-own-adventure (ATLAS)
```

---

## Design Principles

1. **PATHs are the source of example data.** Example data doesn't exist independently — it belongs to a PATH.
2. **Design and test are the same artifact.** There is no "write tests later."
3. **The PATH registry captures everything. The minimal coverage set is curated deliberately.** Not every known PATH is an official test — but every known PATH is worth keeping.
4. **The user's event log is a PATH.** Runtime and design use the same primitive.
5. **PATHs apply everywhere the concept of "journey through a system" applies** — not just application code.
6. **The Human is the ultimate authority.** PATHs are tools for human understanding and decision-making. CORTEX assists with PATH analysis; humans promote, approve, and verify.
