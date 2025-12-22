# ROG - Geographic Routing Algorithm

**Last Updated:** December 21, 2025
**Status:** Q3 - Not Started (Next Priority)

---

## Overview

This document will define the geographic routing algorithm—the core differentiating feature of ROG.

**Question Addressed:** Q3 - Geographic Routing Algorithm (THE CORE PROBLEM)

**Prerequisites:**
- ✓ Layout topology model complete (Q1) - see [data-models.md](data-models.md)
- ✓ Platform architecture defined (Q22-26) - see [platform.md](platform.md)

---

## The Problem

Previous implementation failure:
> "The generation algorithm had no awareness of layout configuration, so it was impossible to create trains that operated on a particular line, and operators were forced to go through the entire layout to work their train."

**ROG must solve this.**

---

## Questions to Answer

From Q3 framework:

1. What's the actual algorithm for geographic routing?
2. How does "build a train for the West Branch" work?
3. What defines a "branch"? Explicit in data model or inferred from topology?
4. How does system know which industries are "logically grouped"?
5. Should routing prefer fewer trains with longer routes, or more trains with focused routes?

**Known challenges:**
- Grain Elevator routing requires 2 reversals through Chare Bros twice
- Must handle directional constraints (some routes one-way only)
- Operational cost vs distance optimization
- Branch detection from topology graph

---

## Placeholder Content

**This section is under active development.**

Content will include:
- Branch detection algorithm
- Pathfinding with cost optimization
- Geographic grouping logic
- Complex routing scenarios (reversals, runarounds)
- Validation on real examples

---

## Related Documents

- [data-models.md](data-models.md) - Topology model (foundation)
- [../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md) - Detailed spec
- [platform.md](platform.md) - Offline sync constraints affecting routing
- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Q3 tracking
