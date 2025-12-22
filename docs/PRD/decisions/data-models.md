# Data Model Decisions

**Category:** Data Models & Topology
**Last Updated:** December 21, 2025

---

## Turnout Directionality Model
#topology #data-model #directionality #routing #final #mvp-critical

**Decision:** Turnouts have `facing_direction` indicating which travel direction encounters facing point (choice)
**Date:** 2025-12-20
**Status:** Final

**Context:**
Modeling track topology for routing algorithm. Need to represent that diverging routes are directional (can only take diverging route when approaching from facing point).

**Initial misunderstanding:**
AI initially modeled turnouts without clear directionality, assuming bidirectional. Crossover example revealed this was wrong (had both turnouts facing same direction, which is impossible).

**Breakthrough:**
User correction on Kennedy crossover module:
> "You have two turnouts that are opposing each other, both connecting the diverging tracks together... You correctly identified that they were both left turnouts, but you incorrectly had them both facing right, which is impossible."

Crossovers use same-hand turnouts facing OPPOSITE directions.

**Rationale:**
- Tracks are inherently bidirectional
- Turnouts are bidirectional BUT diverging routes are directional
- Facing point = choice (can diverge)
- Trailing point = merge (no choice)

**Trade-offs:**
- Gained: Accurate operational representation, enables correct routing
- Lost: Some model simplicity (necessary complexity)

**Validation:** Tested on Kennedy (crossover), Pure Oil (ladder), Chesterfield modules.

**Related PRD Section:** [../data-models.md](../data-models.md#turnout-model)

---

## Capacity Measured in Car Count, Not Length
#capacity #data-model #usability #final #mvp-critical

**Decision:** Industry siding capacity measured in integer car count
**Date:** 2025-12-20
**Status:** Final

**Context:**
Need to validate that trains can fit at industries, prevent over-spotting.

**Alternatives Considered:**

1. **Physical length (feet):**
   - Pros: Prototypically accurate
   - Cons: Cars vary in length, requires complex calculation, unclear UX

2. **"Visual crowding" (qualitative):**
   - Pros: Flexible
   - Cons: Not enforceable by system, inconsistent

**Rationale:**
- Car sizes vary significantly (40-foot box car vs 89-foot auto rack)
- Operators think in "number of spots" not "linear feet"
- Simple integer constraint easy to validate
- Matches real railroad practice ("this siding holds 3 cars")

**Trade-offs:**
- Gained: Simple validation, clear UX, matches operator mental model
- Lost: Some prototypical precision (acceptable for game)

**Related PRD Section:** [../data-models.md](../data-models.md#track-segments)

---

## Visual Editor Essential (Not Optional)
#editor #usability #adoption #topology #final #mvp-critical

**Decision:** Visual module editor is essential, hand-editing YAML impractical
**Date:** 2025-12-20
**Status:** Final

**Context:**
Validating topology data model by creating module definitions. User hand-edited Pure Oil module in YAML.

**User's discovery:**
> "I spent at least 30 minutes trying to write that Pure-Oil-Module.yaml, in notepad, and still got it wrong. It really sucks doing this by hand... Building these absolutely will need to be through some kind of visual editor."

**Rationale:**
- Even with correct data model, manual editing is error-prone
- 30+ minutes for single module (impractical at scale)
- Visual editor can validate constraints, prevent impossible configurations
- Drag-and-drop module assembly matches mental model

**Trade-offs:**
- Gained: Usability, validation, adoption
- Lost: None (YAML still good storage format, just not editing format)

**Implementation note:** YAML/JSON storage format works, visual editor generates definitions in background.

**Related PRD Section:** [../data-models.md](../data-models.md#implementation-notes)

---

## Module-Level Topology Model
#topology #data-model #modular-layout #routing #final #mvp-critical

**Decision:** Two-level model (module definitions + layout connections)
**Date:** 2025-12-20
**Status:** Final

**Context:**
Representing modular layouts where configuration changes between setups.

**Rationale:**
- Modules are reusable across layouts
- Orientation-agnostic (labels arbitrary)
- Enables graph-based routing
- Supports community module library

**Key insight:** Left/right labels are arbitrary but necessary for communication. Model works regardless of absolute orientation.

**Validation:** Tested on real NTrak modules (Kennedy, Pure Oil, Chesterfield).

**Related PRD Section:** [../data-models.md](../data-models.md#layout-topology-model)
