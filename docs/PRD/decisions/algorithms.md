# ROG - Algorithm Decisions

**Last Updated:** December 24, 2025
**Category:** Routing, pathfinding, branch detection, car assignment
**Total Decisions:** 7

---

## Branch Detection: Hybrid Auto-Detect + Session Master Review
#routing #topology #branch-detection #usability #adoption #mvp-critical #final

**Decision:** Automatically detect subdivisions and branches from topology, present to session master for review/modification
**Date:** 2025-12-24
**Status:** Final

**Context:**
Geographic routing requires identifying branches (operational zones) from layout topology. Two extremes exist: fully automatic (no control) or fully manual (tedious setup). Need balance between automation and control.

**Alternatives Considered:**

1. **Fully automatic branch detection:**
   - Pros: Zero setup effort, works immediately
   - Cons: May detect wrong boundaries, no operator input, hard to fix mistakes

2. **Fully manual branch definition:**
   - Pros: Complete control, session master knows layout best
   - Cons: Tedious, error-prone, barrier to adoption, discourages trying ROG

3. **Hybrid: Auto-detect with review (CHOSEN):**
   - Pros: Best of both—automation + control, fast setup with refinement option
   - Cons: Requires good auto-detection algorithm (worth the investment)

**Rationale:**
User's clarification revealed the pattern: "Ideally computed and 'just works' where recommended branches are correct, then session master tweaks or makes corrections."

This matches software best practices: intelligent defaults + user override. Most layouts will work with auto-detection; edge cases get manual refinement.

**Trade-offs:**
- Gained: Fast setup, good UX, encourages adoption
- Lost: Must implement smart detection algorithm (not trivial)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#branch-detection-algorithm)

---

## Pathfinding: Modified Dijkstra with Operational Costs
#routing #pathfinding #cost-optimization #mvp-critical #final

**Decision:** Use Dijkstra's algorithm with directional edges and operational cost weighting
**Date:** 2025-12-24
**Status:** Final

**Context:**
Cars need routing from origin to destination considering:
- Physical distance (shorter is better)
- Operational cost (turnouts, reversals, pushing train)
- Directional constraints (facing vs trailing turnouts)
- Runtime factors (mainline traffic direction)

**Alternatives Considered:**

1. **Shortest path only (simple Dijkstra):**
   - Pros: Simple, well-understood, fast
   - Cons: Ignores operational reality (derailment risk, reversals)

2. **Heuristic-based (A*):**
   - Pros: Faster for long paths, goal-directed
   - Cons: Need good heuristic (Euclidean doesn't work for modular layouts), complexity

3. **Modified Dijkstra with cost model (CHOSEN):**
   - Pros: Handles multi-factor costs, directional edges, proven algorithm
   - Cons: Slightly slower than A* (acceptable for typical layouts)

**Rationale:**
User provided detailed cost model from operational experience:
- Mainline travel: cheap (1.0× distance)
- Against traffic: very expensive (5.0× distance)
- Diverging turnouts: moderate cost (+15)
- Reversals: high cost (+50)
- Backups: very high cost (+100)

Dijkstra naturally handles multi-factor edge weights. Directional edges model facing vs trailing turnout constraints.

**Trade-offs:**
- Gained: Operationally realistic routing, handles complex scenarios (Grain Elevator 2 reversals)
- Lost: Slight performance overhead (negligible for typical 50-100 industry layouts)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#cost-based-pathfinding)

---

## Car Assignment: Full 4-Cycle with Recalculation
#routing #car-lifecycle #4-cycle-waybill #session-management #mvp-critical #final

**Decision:** Assign full 4-cycle pattern when car onboarded, recalculate on state changes
**Date:** 2025-12-24
**Status:** Final

**Context:**
Traditional car card/waybill system uses 4-cycle patterns (load → deliver → reload → deliver → return). Should ROG assign full cycle upfront or calculate next destination on-demand?

**Alternatives Considered:**

1. **On-demand (next destination only):**
   - Pros: Adapts naturally to state changes, simpler logic
   - Cons: May localize rolling stock (cars stay in one area), no strategic planning

2. **Full cycle (4-movement pattern) (CHOSEN):**
   - Pros: Promotes layout-wide movement, strategic train building, mimics traditional practice
   - Cons: Must recalculate when state changes (capacity, operator reassignment)

3. **Hybrid: Full cycle with recalculation (CHOSEN REFINEMENT):**
   - Pros: Strategic planning + adaptive to reality-first changes
   - Cons: More complex implementation (worth it for gameplay quality)

**Rationale:**
User noted: "It's interesting if the car's lifecycle is known up front...it might be best to do that."

Full cycle prevents localization (cars cycling only O'Brien ↔ Grain Elevator) while promoting variety. Recalculation handles reality-first principle (capacity full, operator leaves, etc.).

**Pattern (from domain-knowledge.md):**
1. Load at Industry A → Deliver to Industry B
2. Empty at Industry B → Load at Industry C
3. Load at Industry C → Deliver to Industry D
4. Empty at Industry D → Return to Industry A (cycle restart)

**Trade-offs:**
- Gained: Layout-wide traffic variety, strategic train building, traditional operations feel
- Lost: Must handle recalculation on state changes (acceptable complexity)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#car-assignment-logic)

---

## Operator-Scoped Routing: Branch Assignment Drives Scope
#routing #operator-assignment #branch-assignment #scoping #mvp-critical #final

**Decision:** Operator assigned to branches defines routing scope; foreign cars route to exchange points
**Date:** 2025-12-24
**Status:** Final

**Context:**
Multiple operators working simultaneously need clear responsibility boundaries. How to divide car routing work among operators?

**Alternatives Considered:**

1. **Global pool (first-come-first-served):**
   - Pros: Maximum flexibility, simple logic
   - Cons: Operators conflict, no clear responsibility, chaos at train shows

2. **Train-based assignment (traditional):**
   - Pros: Matches paper car cards, familiar
   - Cons: Rigid, doesn't handle fluid participation or short-line patterns

3. **Branch-based assignment (CHOSEN):**
   - Pros: Clear geographic scope, supports short-lines, enables multi-crew coordination
   - Cons: Requires branch detection (already solving that)

**Rationale:**
User's operational insight: "I think this discovers a key attribute to the gameplay: The session master assigns operators to branches and ROG controls delivery assignments based on that."

This architectural pattern enables:
- Clear operator scope ("work Black River to Palin Bridge")
- Foreign car handling (route to exchange point if outside scope)
- Short-line patterns (O'Brien ↔ Grain Elevator self-contained)
- Dynamic expansion (assign additional branch → scope expands)

**Trade-offs:**
- Gained: Clear responsibilities, realistic operations, short-line support, multi-crew coordination
- Lost: Requires exchange point detection (acceptable additional work)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#operator-scoped-routing)

---

## Exchange Points: Auto-Detect Non-Industry Sidings
#routing #exchange-points #operator-coordination #multi-crew #mvp-critical #final

**Decision:** Detect exchange points as non-industry sidings accessible by multiple branches
**Date:** 2025-12-24
**Status:** Final

**Context:**
When operator's scope doesn't include car's destination, need intermediate transfer point. How to identify exchange points?

**Alternatives Considered:**

1. **Session master designates all exchange points:**
   - Pros: Explicit control, clear intent
   - Cons: Tedious, error-prone, session master may not identify all options

2. **Any siding can be exchange point (on-demand):**
   - Pros: Maximum flexibility, no configuration
   - Cons: May use poor locations (far from branches, low capacity)

3. **Auto-detect with session master review (CHOSEN):**
   - Pros: Intelligent suggestions, session master can add/remove, fast setup
   - Cons: Must implement detection algorithm

**Rationale:**
User described pattern: "Exchange points seem like they can be derived from track layout...I looked for sidings not associated with industry that were shared track with more than one branch."

Detection algorithm:
1. Find all sidings (type = "siding" or "passing_track", not industry)
2. Determine branch accessibility (which branches can reach this siding)
3. Siding reachable by 2+ branches → candidate exchange point
4. Rank by proximity to branch boundaries, capacity, track length
5. Present to session master for review

**Example (UNW 2020):**
- Pure Oil siding: Accessible by Branch A + Branch B → exchange point
- Wooden Village siding: Accessible by Branch B + Branch C → exchange point

**Trade-offs:**
- Gained: Automatic identification, good defaults, promotes multi-crew coordination
- Lost: Session master must review suggestions (acceptable for hybrid approach)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#exchange-points)

---

## Short-Line Pattern Support
#routing #short-line #yardless-operations #operator-assignment #mvp-critical #final

**Decision:** Support self-contained branch operations without yard visits when appropriate
**Date:** 2025-12-24
**Status:** Final

**Context:**
Some branches have industries that trade constantly (O'Brien ↔ Grain Elevator). Traditional yard-based approach forces inefficient yard visits. How to handle?

**Alternatives Considered:**

1. **Always route through yard:**
   - Pros: Matches Class I railroad practice, centralized control
   - Cons: Inefficient for short-line patterns, adds no gameplay value

2. **Never route through yard:**
   - Pros: Efficient, realistic for short lines
   - Cons: Can't handle foreign cars, no layout-wide distribution

3. **Conditional: Within-scope direct, outside-scope to exchange (CHOSEN):**
   - Pros: Supports short-line when appropriate, handles foreign cars via exchanges
   - Cons: Requires scope-aware routing (already implementing)

**Rationale:**
User described realistic pattern: "If the industries at O'Brien and Grain Elevator were so matched that they're constantly trading goods, sending cars to Black River Yard is a waste of time. The O'Brien and Grain Elevator branch essentially becomes a short line."

This matches prototype practice (see yardless-operations.md): short lines and branch operations handle car classification themselves via progressive consist building, not centralized yards.

**How it works:**
- Operator assigned Branch C only (O'Brien → Grain Elevator)
- Both industries in scope → cars route directly between them
- No yard visits → self-contained short line
- Foreign cars (destinations outside Branch C) → route to exchange points
- Realistic operations, satisfying gameplay

**Trade-offs:**
- Gained: Realistic short-line operations, efficient routing, focused operator work
- Lost: None (foreign cars handled via exchange points)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#short-line-pattern-support)

---

## Train Building: Two Workflows (Session Master Onboards + Operator Builds Own)
#routing #train-building #session-management #operator-workflow #mvp-critical #final

**Decision:** Support two train-building workflows depending on session context
**Date:** 2025-12-24
**Status:** Final

**Context:**
Different session scenarios require different train-building approaches. How should ROG handle train creation?

**Alternatives Considered:**

1. **Session master always builds all trains:**
   - Pros: Centralized control, traditional approach
   - Cons: Bottleneck, doesn't support fluid participation

2. **Operators always build own trains:**
   - Pros: Distributed work, fluid participation
   - Cons: Doesn't work for session start (no cars assigned yet)

3. **Two workflows depending on context (CHOSEN):**
   - Session master onboards: For session start, cars being added
   - Operator builds own: For mid-session join, yard operations
   - Pros: Flexible, supports all scenarios
   - Cons: More complex UX (acceptable for better gameplay)

**Rationale:**
User described two distinct scenarios:

**Scenario 1 - Session master onboards:**
"The session master needs to onboard rolling stock into the ROG session. Cars are put into the game, and they are considered empty and unassigned...When an operator is assigned, ROG will assign destinations for the cars depending on which branch the operator will be working on."

**Scenario 2 - Operator builds own:**
"An operator is assigned a locomotive, but is responsible for building their own train...For operators joining an existing ROG session with a populated layout, this is the most likely scenario. You join the game, already in session."

Both are valid, real-world patterns. ROG must support both.

**Trade-offs:**
- Gained: Supports all session scenarios, flexible, realistic workflows
- Lost: Two code paths to implement (acceptable for correct behavior)

**Related PRD Section:** [routing-algorithm.md](../routing-algorithm.md#train-building-workflows)

---

## Summary

**Total decisions:** 7
**Status:** All Final
**Scope:** All MVP-critical

These decisions solve the core Q3 problem: **Geographic routing that enables "build a train for the West Branch" with intelligent car assignments.**

**Key architectural pattern discovered:** Branch assignment to operators drives routing scope, enabling short-line patterns, foreign car handling via exchange points, and multi-crew coordination.
