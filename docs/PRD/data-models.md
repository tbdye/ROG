# ROG - Data Models

**Last Updated:** December 21, 2025
**Status:** Layout topology complete (Q1), other models pending

---

## Overview

This document defines the core data models for ROG: layout topology, cars, industries, trains, and sessions. These models enable geographic-aware routing and reality-first state tracking.

**Questions Addressed:**
- Q1: Layout Topology Representation ✓ Complete
- Q6: Car Identity and Granularity (pending)
- Q7: Industry and Spot Definitions (pending)

---

## Layout Topology Model (Q1 - Complete)

**Status:** ✓ Complete - Validated on real module examples

### Problem Statement

To enable geographic routing, ROG must understand:
- Track connectivity (how modules connect)
- Branch structure (what defines a "branch")
- Routing costs (distance, operational complexity, turnout count)
- Directional constraints (facing vs trailing points)

### Solution: Two-Level Model

**Module level:**
- Defines reusable module components
- Track interfaces (connection points)
- Internal topology (turnouts, segments, industries)
- Sequencing (ordered traversal for cost calculation)

**Layout level:**
- Combines module instances
- Connections between modules
- Runtime configuration (traffic direction on loops)

### Quick Reference

For complete data model specification, see:
**[../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md)**

### Module Structure

```yaml
Module:
  name: string                    # Module identifier
  owner: string                   # Module owner (optional)
  track_interfaces:               # Connection points at edges
    left_edge: [...]
    right_edge: [...]
  internal_topology:
    turnouts: [...]               # Switches with directionality
    track_segments: [...]         # Connections with costs
  industries: [...]               # Industries served (optional)
  sequencing:                     # Ordered traversal for routing
    track_id:
      westbound: [...]
      eastbound: [...]
```

**Key properties:**
- Modules are reusable across layouts
- Orientation-agnostic (labels are arbitrary)
- Topology enables graph-based routing

### Turnout Model

**Critical insight:** Turnouts have directionality—facing point (choice) vs trailing point (merge).

```yaml
Turnout:
  id: string                      # Unique identifier
  hand: left|right|wye|three-way  # Geometric property
  facing_direction: left|right    # Which direction encounters facing point
  facing_point: track_id          # Track where you have choice
  diverging_point: track_id       # Alternate route
  trailing_point: track_id        # Where routes merge
```

**Crossover discovery:**
Crossovers use same-hand turnouts facing OPPOSITE directions. Initial model had both facing same direction (impossible).

### Track Segments

```yaml
Track_Segment:
  id: string
  from: interface_or_turnout_id   # Start connection
  to: interface_or_turnout_id     # End connection (optional for stubs)
  type: mainline|siding|industry_siding|crossover|connector
  capacity_cars: integer          # For industry sidings
  length: string                  # Physical length (optional)
```

**Capacity:**
- Measured in car count (not physical length due to size variation)
- Critical for routing (can't route 4 cars to 2-car siding)

### Layout Structure

```yaml
Layout:
  name: string
  modules:
    - id: string                  # Instance identifier
      module_ref: string          # Reference to module definition
      position: {x, y, rotation}  # Physical position (optional)

  connections:
    - from: {module: id, interface: id}
      to: {module: id, interface: id}
      track_designation: string

  runtime_state:                  # Dynamic configuration
    loop_traffic_direction:
      red_line: clockwise|counter-clockwise
```

### Graph Representation

The topology model enables building a directed graph:

**Nodes:**
- Module interfaces (entry/exit points)
- Turnouts (decision points)
- Industries (destinations)

**Edges:**
- Track segments (distance/operational cost)
- Turnout transitions (complexity cost)
- Directionality constraints

**Properties:**
- Some edges bidirectional (through tracks)
- Some unidirectional (diverging routes from trailing)
- Edge weights enable cost-based pathfinding

### Operational Cost Model

Different track elements have different routing costs:

**Track segments:**
- Mainline with looping traffic: HIGH (interference risk)
- Mainline without traffic: MEDIUM
- Siding/industry track: LOW

**Turnouts:**
- Straight route (trailing): MEDIUM
- Diverging route: HIGH (derailment risk)
- Mainline diverging: VERY HIGH

**Routing optimization:**
- Minimize total path cost (distance + operational + complexity)
- Balance shortest vs safest vs simplest
- Prefer sidings over mainlines
- Minimize turnout count

### Validation Examples

Model validated on real NTrak modules:
- **Al - Chesterfield:** Complex multi-track with industry sidings
- **Scott - Kennedy:** Crossover demonstrating directional transitions
- **Pure Oil:** 3-track ladder forming multi-module runaround

### Key Decisions

**Visual editor essential:**
User spent 30+ minutes hand-editing YAML for one module, still had errors.
> "It really sucks doing this by hand... Building these absolutely will need to be through some kind of visual editor."

**Runaround detection:**
Runarounds = parallel tracks converging at both ends. Detectable from graph analysis, may span multiple modules (Pure Oil ladder + Kennedy crossover).

**Branch detection (deferred to Q3):**
System should derive branches from topology graph automatically. User mentioned junctions define branches, hierarchical structure possible.

### Implementation Notes

**Module library:**
- Modules are reusable across layouts
- Community library possible (share module definitions)
- Module owners can contribute to library

**Storage format:**
- YAML demonstrated as viable
- JSON alternative equally valid
- Database schema mirrors this structure

**Orientation handling:**
- Left/right/top/bottom labels arbitrary
- Consistency within module required
- Visual editor handles rotation/orientation

### Future Enhancements (Not Yet Specified)

- Three-way turnout handling (mentioned, not modeled)
- Wye turnout special cases
- Multi-level layouts (staging above/below)
- Dynamic track segments (movable bridges)
- Grade/elevation (affects locomotive power)
- Electrification boundaries (DC vs DCC)

---

## Car Identity Model (Q6 - Not Started)

**Status:** Pending

### Questions to Answer

- Duplicate cars exist—how should ROG handle?
- Is "box car" sufficient or need "50-foot insulated box car"?
- What identifiers required vs optional when checking in?
- Is car ownership tracking core data or metadata?

### Preliminary Structure

```yaml
Car:
  id: string                      # Unique identifier (if enforced)
  reporting_marks: string         # Railroad (e.g., "UP", "BNSF")
  car_number: string              # Car number
  car_type: string                # Box, hopper, tank, etc.
  length: string                  # Physical length (e.g., "50-foot")
  attributes: [...]               # Insulated, refrigerated, etc.
  owner: string                   # Physical owner (person)
  current_state: state_enum       # See car lifecycle (Q2)
  current_location: location_id   # Where is it right now?
  assigned_train: train_id        # Ownership model (Q26)
```

**Decision pending Q6.**

---

## Industry and Spot Model (Q7 - Not Started)

**Status:** Pending

### Questions to Answer

- What's the actual data model for an industry spot?
- Can multiple cars share one spot?
- How is spot capacity determined?
- How specific are commodity matchings?
- Do industries have operating hours/schedules?

### Preliminary Structure

```yaml
Industry:
  id: string
  name: string
  location: track_segment_id      # From topology model
  spots: [...]                    # Individual car spots
  consumes: [commodity_type, ...] # What it receives
  produces: [commodity_type, ...] # What it ships
  capacity_cars: integer          # Total capacity

Spot:
  id: string
  industry_id: string
  spot_number: integer            # Position (door 1, door 2, etc.)
  current_car: car_id             # What's spotted here now
  accepts: [car_type, ...]        # What car types fit
```

**Decision pending Q7.**

---

## Car Lifecycle Model (Q2 - Not Started)

**Status:** Pending

### Questions to Answer

- What are ALL states a car can be in?
- What triggers state transitions?
- Who can change car states?

### Preliminary States

Potential states (to be validated):
- `staged` - In staging, not yet in play
- `assigned` - Assigned to train, not yet picked up
- `in_transit` - On train, moving
- `spotted` - Placed at industry
- `loading` - Being loaded (time-delayed)
- `loaded` - Ready for pickup
- `unloading` - Being unloaded (time-delayed)
- `empty` - Unloaded, ready for next assignment
- `available` - Not assigned, available for routing
- `out_of_game` - Owner took home, not in session

**Decision pending Q2.**

---

## Train Model (Not Yet Addressed)

**Status:** Pending - depends on Q8 (consist building rules), Q10 (workflows)

### Preliminary Structure

```yaml
Train:
  id: string
  symbol: string                  # Train designation (e.g., "WB-1")
  type: local|through|yard        # Operational type
  assigned_operator: user_id      # Who's running it
  consist: [car_id, ...]          # Ordered list of cars
  route: [location_id, ...]       # Planned route
  current_location: location_id   # Where is train now
  work_remaining: [work_item, ...] # Pickups/setouts to do
  status: building|running|completed|abandoned
```

**Decision pending Q8, Q10.**

---

## Session Model (Not Yet Addressed)

**Status:** Pending - depends on Q4 (time modeling), Q12 (state tracking)

### Preliminary Structure

```yaml
Session:
  id: string
  name: string
  layout_id: layout_ref           # Which layout configuration
  session_master: user_id
  operators: [user_id, ...]
  start_time: timestamp
  current_time: timestamp         # Fast clock or real clock
  status: setup|active|paused|completed

  cars: [car_state, ...]          # Snapshot of all car states
  trains: [train_state, ...]      # Snapshot of all trains

  metrics:
    cars_spotted: integer
    trains_completed: integer
    sync_conflicts: integer
```

**Decision pending Q4, Q12, Q19 (session end state).**

---

## Model Relationships

```
Layout (topology)
  └─> Industries (spots)
       └─> Cars (positioned at spots or on trains)
            └─> Trains (consists)
                 └─> Operators (assigned)

Session
  ├─> Layout (configuration)
  ├─> Cars (state snapshot)
  ├─> Trains (work assignments)
  └─> Operators (participants)
```

---

## Design Principles

### 1. Reality-First
Car positions reflect actual state, not theoretical. When operator says car is at Industry C, system updates to match reality.

### 2. Car Ownership Prevents Conflicts
Cars locked to assigned train (see Q26). Most sync conflicts prevented by design.

### 3. Topology Enables Routing
Layout graph representation enables pathfinding with cost optimization.

### 4. Capacity Constraints
Car count matters (not just physical length). System enforces capacity limits.

### 5. State Persistence
Sessions can span multiple days. State doesn't reset when session pauses.

---

## Implementation Considerations

### Database Schema

Topology model maps well to relational DB:
- `modules` table (reusable definitions)
- `layouts` table (compositions)
- `module_instances` table (layout ↔ module many-to-many)
- `connections` table (interface relationships)

Car/train/session models likely normalized across multiple tables.

### Graph Database Alternative

Topology is inherently a graph. Neo4j or similar could be natural fit:
- Nodes: interfaces, turnouts, industries
- Edges: track segments, connections
- Queries: pathfinding, branch detection

Decision deferred to Phase 3 (implementation planning).

---

## Related Documents

- [../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md) - Detailed topology spec
- [routing-algorithm.md](routing-algorithm.md) - How routing uses topology
- [platform.md](platform.md) - Car ownership model (Q26)
- [open-questions.md](open-questions.md) - Q6, Q7 details
- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Progress tracking
