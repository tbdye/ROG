# Layout Topology Data Model Specification

**Version:** 1.0  
**Date:** December 20, 2025  
**Status:** Validated through real module examples  
**Purpose:** Reference specification for representing modular railroad layouts in ROG

---

## Overview

This document defines the data model for representing railroad layout topology, including both individual module definitions and layout-level connections. The model was validated through iterative testing on real NTrak/FREE-MO module examples.

**Design Principles:**
- Module definitions are reusable across different layout configurations
- Topology enables graph-based routing algorithms
- Sequencing supports cost-based path optimization
- Model is orientation-agnostic (labels are arbitrary)

---

## Module Data Model

### Top-Level Structure

```yaml
Module:
  name: string                    # Module identifier
  owner: string                   # Module owner (optional)
  track_interfaces: {...}         # Connection points
  internal_topology: {...}        # Turnouts and track segments
  industries: [...]               # Industry definitions (optional)
  sequencing: {...}               # Ordered traversal (optional)
```

---

### Track Interfaces

Defines connection points at module edges where connections to adjacent modules occur.

```yaml
track_interfaces:
  left_edge:
    - {id: string, line: string, connects_through: boolean}
    - ...
  
  right_edge:
    - {id: string, line: string, connects_through: boolean}
    - ...
  
  # Additional edges as needed (top_edge, bottom_edge, etc.)
```

**Properties:**
- `id` - Unique identifier within module (e.g., "left_Blue", "right_Red")
- `line` - Track designation (e.g., "Blue", "Red", "Yellow", "Mountain")
- `connects_through` - Boolean indicating if track continues through module or terminates

**Notes:**
- Line names are arbitrary (NTrak standard: Red/Yellow/Blue, but custom lines allowed)
- `connects_through: false` indicates stub track (industry siding, etc.)
- Edge names (left/right) are relative to module orientation (arbitrary but consistent)

**Example:**
```yaml
track_interfaces:
  left_edge:
    - {id: "left_Blue", line: "Blue", connects_through: true}
    - {id: "left_Red_siding", line: "Red_Siding", connects_through: false}
```

---

### Internal Topology

Defines turnouts and track segments within the module.

#### Turnouts

```yaml
internal_topology:
  turnouts:
    - id: string                  # Unique turnout identifier
      hand: string                # "left" | "right" | "wye" | "three-way"
      facing_direction: string    # "left" | "right" | "up | "down"
      facing_point: string        # Track ID where facing point connects
      diverging_point: string     # Track ID for diverging route
      trailing_point: string      # Track ID where trailing point connects
```

**Properties:**

**`hand`** - Geometric turnout type:
- `"left"` - Diverging route goes left
- `"right"` - Diverging route goes right
- `"wye"` - Symmetric Y-shape (no main vs diverging)
- `"three-way"` - Single switch diverging both left and right

**`facing_direction`** - Which direction of travel encounters facing point:
- `"left"` - Train traveling LEFT encounters facing point (has choice)
- `"right"` - Train traveling RIGHT encounters facing point (has choice)
- `"up"` - Train traveling UP encounters facing point (has choice)
- `"down"` - Train traveling DOWN encounters facing point (has choice)

**`facing_point`** - Track segment ID where you approach facing point from

**`diverging_point`** - Track segment ID for the diverging route option

**`trailing_point`** - Track segment ID where routes merge (trailing point)

**Critical Relationships:**
- **Facing point:** Train encounters choice (continue straight OR diverge)
- **Trailing point:** Routes merge (no choice, trailing route joins)
- **Directionality:** Diverging routes ARE directional (can only take diverging route when hitting facing point)

**Example: Simple Right-Hand Turnout**
```yaml
- id: "TO_Blue_siding"
  hand: "right"
  facing_direction: "left"        # Traveling L→R hits facing point
  facing_point: "blue_through"
  diverging_point: "blue_industry_siding"
  trailing_point: "blue_through"
```

**Example: Crossover (Two Turnouts, Opposite Facing Directions)**
```yaml
# Yellow to Blue crossover turnout
- id: "TO_Yellow_to_Blue"
  hand: "left"
  facing_direction: "right"       # Eastbound has choice
  facing_point: "yellow_through"
  diverging_point: "crossover_connector"
  trailing_point: "yellow_through"

# Blue to Yellow crossover turnout
- id: "TO_Blue_to_Yellow"
  hand: "left"
  facing_direction: "left"        # Westbound has choice
  facing_point: "blue_through"
  diverging_point: "crossover_connector"
  trailing_point: "blue_through"
```

**Key Insight:** Crossovers use same-hand turnouts facing OPPOSITE directions.

---

#### Track Segments

```yaml
internal_topology:
  track_segments:
    - id: string                  # Unique segment identifier
      from: string                # Starting point (interface or turnout ID)
      to: string                  # Ending point (interface or turnout ID) [optional]
      type: string                # Track classification
      capacity_cars: integer      # Car capacity (for sidings) [optional]
      length: string              # Physical length [optional]
```

**Properties:**

**`id`** - Unique identifier (e.g., "blue_through", "red_industry_siding")

**`from`** - Starting connection point:
- Track interface ID (e.g., "left_Blue")
- Turnout ID (e.g., "TO_Red_ladder")

**`to`** - Ending connection point (optional for stub tracks):
- Track interface ID (e.g., "right_Blue")
- Turnout ID (e.g., "TO_Yellow_merge")
- Omitted for stub tracks that terminate at industry

**`type`** - Track classification:
- `"mainline"` - Through track, high operational cost if looping trains
- `"siding"` - Passing track or storage, lower operational cost
- `"passing_track"` - Specific type of siding for meets/passes
- `"runaround"` - Siding that enables locomotive repositioning
- `"industry_siding"` - Stub track serving industry
- `"crossover"` - Connector between parallel mainlines
- `"connector"` - Generic connecting segment

**`capacity_cars`** - How many cars fit (for industry sidings):
- Measured in car count (not physical length due to car size variation)
- Critical for routing (can't route 4 cars to 2-car siding)

**`length`** - Physical length (optional):
- May be used for distance-based routing cost calculation
- Can be omitted if not relevant to routing

**Example: Through Track**
```yaml
- id: "blue_through"
  from: "left_Blue"
  to: "right_Blue"
  type: "mainline"
```

**Example: Industry Siding**
```yaml
- id: "pure_oil_siding"
  from: "TO_Red_ladder"
  type: "industry_siding"
  capacity_cars: 3
```

**Example: Crossover Connector**
```yaml
- id: "crossover_connector"
  from: "TO_Yellow_to_Blue"
  to: "TO_Blue_to_Yellow"
  type: "crossover"
```

---

### Industries

Defines industries served by the module (optional).

```yaml
industries:
  - name: string                  # Industry name
    location: string              # Track segment ID where industry is located
    consumes: [string, ...]       # Commodity types consumed
    produces: [string, ...]       # Commodity types produced
    spots: integer                # Number of car spots
```

**Properties:**

**`name`** - Industry identifier (e.g., "Pure_Oil_Industry", "Grain_Elevator")

**`location`** - Track segment ID where industry is located (typically industry_siding)

**`consumes`** - List of commodity types this industry receives (e.g., ["crude_oil", "chemicals"])

**`produces`** - List of commodity types this industry ships (e.g., ["refined_products", "plastics"])

**`spots`** - Number of car spots available at industry

**Example:**
```yaml
industries:
  - name: "Pure_Oil_Refinery"
    location: "red_industry_siding"
    consumes: ["crude_oil", "metal", "chemicals"]
    produces: ["refined_products", "plastics", "gasoline"]
    spots: 3
```

---

### Sequencing

Defines ordered traversal of track segments and turnouts for each direction of travel. Enables cost-based routing calculations.

```yaml
sequencing:
  track_id:
    westbound:                    # Right to left
      - segment_or_turnout_id
      - segment_or_turnout_id
      - ...
    
    eastbound:                    # Left to right
      - segment_or_turnout_id
      - segment_or_turnout_id
      - ...

    northbound:                    # Bottom to Top
      - segment_or_turnout_id
      - segment_or_turnout_id
      - ...

    southbound:                    # Top to Bottom
      - segment_or_turnout_id
      - segment_or_turnout_id
      - ...
```

**Purpose:**
- Enables routing algorithm to calculate total path cost
- Includes both track segments (distance cost) and turnouts (operational cost)
- Ordered sequence matches actual traversal path

**Directional Conventions:**
- `westbound` - Right to left
- `eastbound` - Left to right
- `northbound` / `southbound` - For vertical orientations
- Conventions are arbitrary but must be consistent within module

**Example:**
```yaml
sequencing:
  blue_through:
    westbound:  # Right to left
      - right_Blue_interface
      - TO_Blue_siding            # Facing point (has choice)
      - TO_Blue_to_Yellow         # Facing point (can cross)
      - left_Blue_interface
    
    eastbound:  # Left to right
      - left_Blue_interface
      - TO_Blue_to_Yellow         # Trailing point (merge only)
      - TO_Blue_siding            # Trailing point (merge only)
      - right_Blue_interface
```

**Cost Calculation Use:**
Each element in sequence can have associated cost:
- Track segments: distance, mainline exposure
- Turnouts: derailment risk, especially on diverging routes

---

## Layout Data Model

### Top-Level Structure

```yaml
Layout:
  name: string                    # Layout identifier
  modules: [...]                  # Module instances
  connections: [...]              # Module-to-module connections
  runtime_state: {...}            # Runtime configuration (optional)
```

---

### Modules

List of module instances used in this layout.

```yaml
modules:
  - id: string                    # Instance identifier
    module_ref: string            # Reference to module definition
    position: {...}               # Physical position (optional)
```

**Properties:**

**`id`** - Unique identifier for this instance (e.g., "kennedy", "pure_oil_1")

**`module_ref`** - Reference to module definition by name (e.g., "Scott - Kennedy")

**`position`** - Physical position/orientation (optional, for visualization):
```yaml
position:
  x: number
  y: number
  rotation: number              # Degrees
```

**Example:**
```yaml
modules:
  - {id: "kennedy", module_ref: "Scott - Kennedy"}
  - {id: "pure_oil", module_ref: "Pure Oil"}
  - {id: "chare_bros", module_ref: "Mike - Chare Bros"}
```

---

### Connections

Defines which module interfaces connect to each other.

```yaml
connections:
  - from:
      module: string              # Module instance ID
      interface: string           # Interface ID from module definition
    to:
      module: string              # Module instance ID
      interface: string           # Interface ID from module definition
    track_designation: string     # Track name (informational)
```

**Properties:**

**`from`** - Source module and interface:
- `module` - Module instance ID (from modules list)
- `interface` - Interface ID from module's track_interfaces

**`to`** - Destination module and interface:
- `module` - Module instance ID (from modules list)
- `interface` - Interface ID from module's track_interfaces

**`track_designation`** - Track name for this connection (informational, for human readability)

**Example:**
```yaml
connections:
  # Kennedy Blue → Pure Oil Blue
  - from: {module: "kennedy", interface: "right_Blue"}
    to: {module: "pure_oil", interface: "left_Blue"}
    track_designation: "Blue"
  
  # Pure Oil OneTrack → Chare Bros OneTrack
  - from: {module: "pure_oil", interface: "right_Red"}
    to: {module: "chare_bros", interface: "left_Red"}
    track_designation: "Red_OneTrack"
```

---

### Runtime State

Runtime configuration that changes during operations (optional).

```yaml
runtime_state:
  loop_traffic_direction:         # For loop layouts
    red_line: "clockwise"         # or "counter-clockwise"
    yellow_line: "counter-clockwise"
```

**Properties:**

**`loop_traffic_direction`** - Current traffic direction on loop mainlines:
- Set by session master
- Can change multiple times during session
- Affects routing (avoid opposing traffic)

**Example:**
```yaml
runtime_state:
  loop_traffic_direction:
    red_line: "clockwise"
    yellow_line: "counter-clockwise"
    blue_line: "operations"       # Not looping, used for ops only
```

---

## Graph Representation for Routing

The topology model enables building a directed graph:

**Nodes:**
- Module interfaces (entry/exit points)
- Turnouts (decision points)
- Industries (destinations)

**Edges:**
- Track segments (with cost: distance, operational cost, type)
- Turnout transitions (with cost: derailment risk)
- Directionality constraints (facing vs trailing)

**Graph Properties:**
- Some edges are bidirectional (through tracks)
- Some edges are unidirectional (diverging routes when approaching from trailing)
- Edge weights enable cost-based pathfinding

**Runaround Detection:**
Runarounds = parallel tracks that converge at both ends
- Detectable by analyzing graph for alternate paths between same nodes
- May span multiple modules (Pure Oil ladder + Kennedy crossover)

**Branch Detection:**
Branches = connected subgraphs sharing common routing paths
- Derived from topology, not manually tagged
- Junctions (turnouts with diverging routes) define branch boundaries
- Hierarchical structure (main branch → sub-branches)

---

## Operational Cost Model

Different track elements have different operational costs:

**Track Segments:**
- Mainline with looping traffic: HIGH cost (interference risk)
- Mainline without traffic: MEDIUM cost
- Siding/industry track: LOW cost

**Turnouts:**
- Straight route (trailing point): MEDIUM cost
- Diverging route: HIGH cost (derailment risk)
- Mainline diverging route: VERY HIGH cost

**Routing Optimization:**
- Minimize total path cost (distance + operational + complexity)
- Balance shortest path vs safest path vs simplest path
- Prefer sidings over mainlines when possible
- Minimize turnout count when multiple paths exist

---

## Validation Rules

**Module Definition:**
- All turnout facing/diverging/trailing points must reference valid track segments
- All track segment from/to points must reference valid interfaces or turnouts
- Industry locations must reference valid track segments
- Sequencing must include all segments traversed in order

**Layout Connections:**
- Both sides of connection must reference valid module instances
- Interface IDs must exist in referenced module definitions
- No duplicate connections (same interface used twice)

**Topological Consistency:**
- Connected interfaces must have compatible track designations
- Graph must be connected (no isolated subgraphs unless intentional)
- Runarounds must have alternate paths that converge

---

## Example: Complete Module Definition

```yaml
Module:
  name: "Scott - Kennedy"
  owner: "Scott"
  
  track_interfaces:
    left_edge:
      - {id: "left_Green", line: "Green", connects_through: true}
      - {id: "left_Blue", line: "Blue", connects_through: true}
      - {id: "left_Yellow", line: "Yellow", connects_through: true}
      - {id: "left_Red", line: "Red", connects_through: true}
    
    right_edge:
      - {id: "right_Green", line: "Green", connects_through: true}
      - {id: "right_Blue", line: "Blue", connects_through: true}
      - {id: "right_Yellow", line: "Yellow", connects_through: true}
      - {id: "right_Red", line: "Red", connects_through: true}
  
  internal_topology:
    turnouts:
      - id: "TO_Green_siding"
        hand: "left"
        facing_direction: "right"
        facing_point: "green_through"
        diverging_point: "green_industry_siding"
        trailing_point: "green_through"
      
      - id: "TO_Blue_siding"
        hand: "right"
        facing_direction: "left"
        facing_point: "blue_through"
        diverging_point: "blue_industry_siding"
        trailing_point: "blue_through"
      
      - id: "TO_Yellow_to_Blue"
        hand: "left"
        facing_direction: "right"
        facing_point: "yellow_through"
        diverging_point: "crossover_connector"
        trailing_point: "yellow_through"
      
      - id: "TO_Blue_to_Yellow"
        hand: "left"
        facing_direction: "left"
        facing_point: "blue_through"
        diverging_point: "crossover_connector"
        trailing_point: "blue_through"
    
    track_segments:
      - {id: "green_through", from: "left_Green", to: "right_Green", type: "mainline"}
      - {id: "red_through", from: "left_Red", to: "right_Red", type: "mainline"}
      - {id: "blue_through", from: "left_Blue", to: "right_Blue", type: "mainline"}
      - {id: "yellow_through", from: "left_Yellow", to: "right_Yellow", type: "mainline"}
      - {id: "green_industry_siding", from: "TO_Green_siding", type: "industry_siding", capacity_cars: 2}
      - {id: "blue_industry_siding", from: "TO_Blue_siding", type: "industry_siding", capacity_cars: 2}
      - {id: "crossover_connector", from: "TO_Yellow_to_Blue", to: "TO_Blue_to_Yellow", type: "crossover"}
  
  sequencing:
    green_through:
      westbound:
        - right_Green_interface
        - TO_Green_siding
        - left_Green_interface
      eastbound:
        - left_Green_interface
        - TO_Green_siding
        - right_Green_interface
    
    red_through:
      westbound:
        - right_Red_interface
        - left_Red_interface
      eastbound:
        - left_Red_interface
        - right_Red_interface
    
    yellow_through:
      westbound:
        - right_Yellow_interface
        - TO_Yellow_to_Blue
        - left_Yellow_interface
      eastbound:
        - left_Yellow_interface
        - TO_Yellow_to_Blue
        - right_Yellow_interface
    
    blue_through:
      westbound:
        - right_Blue_interface
        - TO_Blue_siding
        - TO_Blue_to_Yellow
        - left_Blue_interface
      eastbound:
        - left_Blue_interface
        - TO_Blue_to_Yellow
        - TO_Blue_siding
        - right_Blue_interface
```

---

## Implementation Notes

**Visual Editor Requirements:**
- Hand-editing YAML is impractical (30+ minutes for single module, prone to errors)
- Visual editor should:
  - Allow drag-and-drop module assembly
  - Snap connections automatically
  - Generate topology definitions in background
  - Validate topology consistency
  - Visualize routing graph

**Storage Format:**
- YAML demonstrated as viable storage format
- JSON alternative would work equally well
- Database schema can mirror this structure

**Module Library:**
- Modules are reusable across layouts
- Module owners can share definitions
- Community library of standard modules possible

**Orientation Handling:**
- Labels (left/right/top/bottom) are arbitrary
- What matters is consistency within module
- Visual editor can handle rotation/orientation translation

---

## Future Enhancements

**Potential Extensions (Not Yet Specified):**
- Three-way turnout handling (mentioned but not modeled)
- Wye turnout special cases
- Multi-level layouts (staging above/below)
- Dynamic track segments (movable bridges, removable sections)
- Grade/elevation modeling (affects locomotive power requirements)
- Electrification boundaries (DC vs DCC sections)

**Not Included in Current Scope:**
- Car routing logic (separate from topology)
- Industry scheduling/operations
- Train consist rules
- Session state tracking
- Time modeling

---

## Version History

**v1.0 (2025-12-20):**
- Initial specification
- Validated on Kennedy, Pure Oil, Chesterfield modules
- Includes module and layout models
- Sequencing for cost-based routing
- Operational cost framework

---

## References

- **Phase 2 PRD Progress:** `phase2-prd-progress.md`
- **Session Summary:** `2025-12-20-topology-representation.md`
- **Module Examples:** Kennedy (crossover), Pure Oil (ladder), Chare Bros (simple through)
- **Layout Example:** UNW 2020 Layout (Kennedy ↔ Pure Oil ↔ Chare Bros)
