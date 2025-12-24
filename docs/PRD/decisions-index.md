# ROG Decision Index

**Last Updated:** December 24, 2025
**Purpose:** Quick reference to find specific technical decisions without loading full decision logs
**Total Decisions:** 19 (5 platform, 4 data-model, 3 process, 7 algorithms)

---

## How to Use This Index

**Finding a decision:**
1. **By name:** Scan this index for the topic
2. **By tag:** Use tag search (see Tag System section below)
3. **By category:** Browse category sections
4. **By grep:** `grep -n "Decision Name" docs/PRD/decisions/category.md`

**Adding new decisions:**
1. Add to appropriate category file in `docs/PRD/decisions/`
2. **Add tags** to decision header (see Tag System section)
3. Add **Date** and **Status** fields
4. Update this index with new entry
5. If new tag, add to Tag Taxonomy section

---

## Platform Architecture Decisions

**File:** [decisions/platform.md](decisions/platform.md)

- **PWA vs Native Apps** [2025-12-07, Final] → Progressive Web App over native/desktop
  - Tags: `#platform #pwa #deployment #adoption #offline-sync #mvp-critical`

- **Cloud vs Self-Hosted** [2025-12-07, Final] → Cloud SaaS deployment over self-hosted
  - Tags: `#deployment #cloud #adoption #usability #train-show #mvp-critical`

- **Car Ownership Model** [2025-12-07, Final] → Car locking prevents sync conflicts
  - Tags: `#conflict-resolution #offline-sync #car-identity #ownership-model #real-time #mvp-critical`

- **No Paper Backup in MVP** [2025-12-07, Final] → Pure digital, no paper export for V1
  - Tags: `#error-recovery #offline-sync #scope-reduction #post-mvp`

- **Freemium Business Model** [2025-12-07, Final] → Free single operator, paid multi-operator
  - Tags: `#business-model #adoption #cloud #mvp-critical`

**Quick grep:**
```bash
grep -n "PWA vs Native" docs/PRD/decisions/platform.md
grep -n "#offline-sync" docs/PRD/decisions/platform.md
```

---

## Data Models & Topology Decisions

**File:** [decisions/data-models.md](decisions/data-models.md)

- **Turnout Directionality** [2025-12-20, Final] → Turnouts have facing_direction, crossover insight
  - Tags: `#topology #data-model #directionality #routing #mvp-critical`

- **Capacity in Car Count** [2025-12-20, Final] → Integer car count instead of physical length
  - Tags: `#capacity #data-model #usability #mvp-critical`

- **Visual Editor Essential** [2025-12-20, Final] → Hand-editing YAML impractical (30+ min, errors)
  - Tags: `#editor #usability #adoption #topology #mvp-critical`

- **Module-Level Topology** [2025-12-20, Final] → Two-level model (modules + layout connections)
  - Tags: `#topology #data-model #modular-layout #routing #mvp-critical`

**Quick grep:**
```bash
grep -n "Turnout Directionality" docs/PRD/decisions/data-models.md
grep -n "#topology" docs/PRD/decisions/data-models.md
```

---

## Process & Philosophy Decisions

**File:** [decisions/process.md](decisions/process.md)

- **Reality-First Data Model** [2025-12-07, Final] → Operator actions authoritative, system observes
  - Tags: `#error-recovery #session-management #usability #car-identity #mvp-critical`

- **Fluid Participation** [2025-12-07, Final] → Handles operators joining/leaving/switching mid-session
  - Tags: `#session-management #fluid-participation #usability #train-show #mvp-critical`

- **Configuration Variability** [2025-12-07, Final] → Assume modular layouts never same configuration twice
  - Tags: `#modular-layout #session-management #topology #train-show #mvp-critical`

**Quick grep:**
```bash
grep -n "Reality-First" docs/PRD/decisions/process.md
grep -n "#session-management" docs/PRD/decisions/process.md
```

---

## Routing & Algorithm Decisions

**File:** [decisions/algorithms.md](decisions/algorithms.md)

- **Branch Detection: Hybrid Auto-Detect** [2025-12-24, Final] → Auto-detect subdivisions/branches, session master reviews
  - Tags: `#routing #topology #branch-detection #usability #adoption #mvp-critical`

- **Pathfinding: Modified Dijkstra** [2025-12-24, Final] → Dijkstra with operational cost weights and directional edges
  - Tags: `#routing #pathfinding #cost-optimization #mvp-critical`

- **Car Assignment: Full 4-Cycle** [2025-12-24, Final] → Assign full 4-cycle pattern, recalculate on state changes
  - Tags: `#routing #car-lifecycle #4-cycle-waybill #session-management #mvp-critical`

- **Operator-Scoped Routing** [2025-12-24, Final] → Branch assignment drives routing scope
  - Tags: `#routing #operator-assignment #branch-assignment #scoping #mvp-critical`

- **Exchange Points: Auto-Detect** [2025-12-24, Final] → Detect non-industry sidings accessible by multiple branches
  - Tags: `#routing #exchange-points #operator-coordination #multi-crew #mvp-critical`

- **Short-Line Pattern Support** [2025-12-24, Final] → Self-contained branch operations without yard visits
  - Tags: `#routing #short-line #yardless-operations #operator-assignment #mvp-critical`

- **Train Building: Two Workflows** [2025-12-24, Final] → Session master onboards + operator builds own
  - Tags: `#routing #train-building #session-management #operator-workflow #mvp-critical`

**Quick grep:**
```bash
grep -n "Branch Detection" docs/PRD/decisions/algorithms.md
grep -n "#routing" docs/PRD/decisions/algorithms.md
```

---

## Tag System

**ALL decisions must be tagged.** Tags enable multi-dimensional search across categories.

### Tag Taxonomy

**Technical Areas:**
- `#offline-sync` - Offline capability and synchronization
- `#real-time` - Real-time multi-device collaboration
- `#topology` - Layout topology representation
- `#routing` - Routing algorithms and pathfinding
- `#pathfinding` - Pathfinding algorithms (Dijkstra, A*, etc.)
- `#branch-detection` - Subdivision and branch detection
- `#cost-optimization` - Cost-based optimization
- `#car-identity` - Car identification and tracking
- `#car-lifecycle` - Car state transitions and lifecycle management
- `#conflict-resolution` - Handling data conflicts
- `#deployment` - System deployment approach
- `#platform` - Platform choice (web, native, desktop)
- `#data-model` - Data structure decisions
- `#editor` - Visual/graphical editing tools
- `#capacity` - Capacity modeling
- `#directionality` - Directional modeling (turnouts, tracks)
- `#exchange-points` - Car exchange/transfer points
- `#operator-assignment` - Assigning operators to branches/trains
- `#branch-assignment` - Branch assignment patterns
- `#scoping` - Routing scope management
- `#train-building` - Train consist building workflows
- `#operator-workflow` - Operator-specific workflows
- `#operator-coordination` - Multi-operator coordination
- `#multi-crew` - Multi-crew operations
- `#short-line` - Short line railroad operations
- `#yardless-operations` - Operations without classification yards
- `#4-cycle-waybill` - 4-cycle car card/waybill system

**Architectural:**
- `#pwa` - Progressive Web App
- `#cloud` - Cloud hosting
- `#business-model` - Revenue/pricing
- `#ownership-model` - Ownership/locking patterns

**User Experience:**
- `#usability` - Usability/UX decisions
- `#adoption` - Adoption barriers/ease
- `#error-recovery` - Handling errors/reality divergence
- `#session-management` - Session lifecycle

**Context/Constraints:**
- `#train-show` - Train show environment constraints
- `#modular-layout` - Modular layout requirements
- `#permanent-layout` - Permanent layout requirements
- `#fluid-participation` - Operators joining/leaving

**Decision Status:**
- `#final` - Decision is final
- `#tentative` - Decision may change
- `#needs-validation` - Needs user testing/validation

**Scope:**
- `#mvp-critical` - Must have for MVP
- `#post-mvp` - Can defer to post-MVP
- `#scope-reduction` - Reduces MVP scope

### Tag Usage Examples

**Find all offline-sync decisions:**
```bash
grep -r "#offline-sync" docs/PRD/decisions/
```

**Find all MVP-critical decisions:**
```bash
grep -r "#mvp-critical" docs/PRD/decisions/
```

**Find decisions about both topology AND routing:**
```bash
grep -r "#topology" docs/PRD/decisions/ | grep "#routing"
```

**Find all tentative decisions (need revisiting):**
```bash
grep -r "#tentative" docs/PRD/decisions/
```

**Find all train-show constraint decisions:**
```bash
grep -r "#train-show" docs/PRD/decisions/
```

**Count decisions by tag:**
```bash
# Count all mvp-critical decisions
grep -r "#mvp-critical" docs/PRD/decisions/ | wc -l

# Count all topology-related decisions
grep -r "#topology" docs/PRD/decisions/ | wc -l
```

### Tag Maintenance Rules

**When adding a decision:**
1. **Add tags to decision header** (first line after ## title)
2. **Include minimum 3 tags:** At least one technical area, one scope tag, one status tag
3. **Add date and status fields** immediately after tags
4. **Update this index** with decision entry including tags
5. **If introducing new tag,** add to Tag Taxonomy section above

**When updating a decision:**
1. **Update status tag** if decision changes (#tentative → #final)
2. **Update scope tags** if MVP scope changes
3. **Add new tags** if decision scope expands
4. **Update index** with new tags

**Tag format:**
```markdown
## Decision Name
#tag1 #tag2 #tag3 #tag4 #status #scope

**Decision:** Brief statement
**Date:** YYYY-MM-DD
**Status:** Final | Tentative | Needs Validation
```

---

## Future Categories (As Needed)

When decision logs grow, create new category files:

**Potential categories:**
- `decisions/algorithms.md` - Routing, pathfinding, branch detection
- `decisions/workflows.md` - Role-specific operations, UX decisions
- `decisions/car-lifecycle.md` - State transitions, ownership, tracking
- `decisions/integration.md` - JMRI, external systems, data exchange

**Update this index when adding new categories.**

---

## Search Strategies

### 1. Grep the Index
```bash
# Find which file contains a decision
grep -i "ownership" docs/PRD/decisions-index.md
```

### 2. Grep All Decision Files
```bash
# Search all decision logs at once
grep -r "offline" docs/PRD/decisions/
```

### 3. Grep Specific Category
```bash
# Search just platform decisions
grep -n -i "sync" docs/PRD/decisions/platform.md
```

---

## Related Documents

- **PRD Sections:** Link to full requirements in respective PRD files
- **Decisions-log.md (deprecated):** Old monolithic file, replaced by categorized structure
- **Portfolio Patterns:** Process observations in `../portfolio/collaboration-patterns.md`

---

## Maintenance

**When adding decisions:**
1. Choose appropriate category (platform, data-models, process, etc.)
2. Add decision entry to category file
3. Update this index with decision name and one-line description
4. Include "Quick grep" example if category is new

**When category file exceeds ~20k tokens:**
- Consider splitting into subcategories
- Or archive old decisions to `decisions/archive/`
- Update index to reflect new structure
