# ROG - Geographic Routing Algorithm

**Last Updated:** December 24, 2025
**Status:** Q3 - Complete

---

## Overview

Geographic routing is **the core differentiating feature of ROG**. Previous operations systems failed because they lacked layout topology awareness, forcing operators to traverse entire layouts even when working "local" assignments.

**The problem we're solving:**
> "The generation algorithm had no awareness of layout configuration, so it was impossible to create trains that operated on a particular line, and operators were forced to go through the entire layout to work their train."

**ROG's solution:** Topology-aware routing that enables session masters to say "build a train for the West Branch" and get intelligent car assignments limited to that geographic area.

**Question Addressed:** Q3 - Geographic Routing Algorithm

**Prerequisites:**
- ✓ Layout topology model (Q1) - see [data-models.md](data-models.md)
- ✓ Platform architecture (Q22-26) - see [platform.md](platform.md)

---

## Core Concepts

### Subdivisions

**Subdivisions** are the highest-level operational divisions of a layout—separate networks or operational zones.

**Examples:**
- NTrak loop (Red, Yellow, Blue mainlines)
- OneTrak subdivision
- Different levels on multi-deck layouts
- Separate wings of benchwork
- Different rooms

**Detection criteria:**
- Connected graph components (no physical connection between subdivisions)
- Major transition points (e.g., Cascade Corner Wye transitioning NTrak → OneTrak)
- Session master designation (manual override for operational zones)

**Key insight:** Subdivisions may be physically connected but operationally distinct. The Wye at Cascade Corner creates a subdivision boundary because traffic patterns differ dramatically.

### Branches

**Branches** are geographic routing zones within subdivisions—the operational scope for a single train or operator.

**Definition:** The longest distance between two endpoints, where endpoints are:
- Physical end of track (stub/terminus)
- Major transition point (junction, interchange, yard)

**Characteristics:**
- **Accessibility-based:** Not all tracks are always reachable (orphaned tracks don't form branches)
- **Hierarchical:** Branches can contain sub-branches
- **Minimal overlap:** Prefer separate branches over heavily overlapping ones
- **Attribute naming:** Named by endpoints ("Black River to Palin Bridge") unless given formal names

**Example hierarchy (UNW 2020 OneTrak subdivision):**
```
OneTrak Subdivision
├─ Black River Yard to Palin Bridge (Branch A)
├─ Palin Bridge to Cascade South (Branch B)
└─ O'Brien to Grain Elevator (Branch C, sub-branch of B)
```

**Why minimal overlap?** Two branches from Palin Bridge → O'Brien and Palin Bridge → Cascade South would heavily overlap. Better: one branch Palin Bridge → Cascade South, separate branch O'Brien → Grain Elevator.

### Exchange Points

**Exchange points** are non-industry sidings where cars transfer between trains/operators working different branches.

**Detection criteria:**
- Siding track (not industry track)
- Shared by multiple branches (accessible from more than one branch)
- Sufficient capacity for temporary car storage

**Example (UNW 2020):** Car destined for Grain Elevator (Branch C) from Black River Yard (Branch A):
1. Branch A operator delivers to Pure Oil siding (exchange point between A and B)
2. Branch B operator picks up, delivers to Wooden Village siding (exchange point between B and C)
3. Branch C operator picks up, delivers to Grain Elevator

**Gameplay value:** Promotes operator interaction, lengthens gameplay, creates realistic multi-crew operations.

**When to use exchanges:**
- One operator per branch → use exchanges to pass cars
- Multiple branches assigned to one operator → can deliver directly
- Session master can designate exchange points explicitly or let system detect

### Operator Assignment

**The architectural insight:** Branch assignment to operators drives routing scope.

**How it works:**
1. Session master assigns operator to one or more branches
2. ROG routes cars within operator's scope
3. Foreign cars (destinations outside scope) route to exchange points
4. Operator can be reassigned additional branches → scope expands

**Example:**
- Operator assigned to Branch C (O'Brien → Grain Elevator)
- Foreign car arrives at O'Brien destined for Chesterfield (outside Branch C)
- ROG routes car to nearest exchange point (Wooden Village) for transfer
- If operator later assigned Branch B, can now deliver directly to Chesterfield

**Short-line pattern:**
- O'Brien and Grain Elevator trade goods constantly
- Cars cycle O'Brien ↔ Grain Elevator without yard visits
- Branch C becomes self-contained short line
- Foreign cars still route to exchange points

---

## Branch Detection Algorithm

### Phase 1: Subdivision Detection

**Input:** Layout topology graph (from Q1 data model)

**Algorithm:**
1. Build connectivity graph from layout connections
2. Identify strongly connected components
3. Find major transition points (Wyes, interchanges, yards)
4. Group tracks into operational zones
5. Present detected subdivisions to session master for review

**Transition points create subdivision boundaries when:**
- Traffic patterns change significantly (looping → point-to-point)
- Operational authority changes (NTrak → OneTrak different crews)
- Geographic separation (different rooms, levels)

**Example:** Cascade Corner Wye is a transition point because:
- NTrak has looping mainline traffic (Red, Yellow, Blue)
- OneTrak has point-to-point branch operations
- Different operational patterns justify subdivision boundary

### Phase 2: Branch Detection

**Input:** Subdivision topology graph

**Algorithm:**
1. **Find endpoints** within subdivision:
   - Physical track ends (stubs, terminus)
   - Junctions with diverging routes
   - Interchange points, staging yards
   - Major industry clusters

2. **Calculate endpoint pairs:**
   - For each pair of endpoints, find longest path
   - Path length = physical distance + operational cost
   - Longer paths = more significant branches

3. **Identify primary branches:**
   - Longest endpoint-to-endpoint paths become primary branches
   - Example: Black River Yard → Palin Bridge (longest in OneTrak)

4. **Detect sub-branches:**
   - Find junctions along primary branches
   - Trace diverging paths to their endpoints
   - Example: Palin Bridge → Cascade South has junction at O'Brien
   - O'Brien → Grain Elevator becomes sub-branch

5. **Minimize overlap:**
   - If two branches share >50% of track, consider merging or hierarchy
   - Prefer parent-child relationship over overlapping siblings
   - Example: Don't create "Palin → O'Brien" AND "Palin → Cascade South"
   - Instead: "Palin → Cascade South" with "O'Brien → Grain Elevator" sub-branch

6. **Name branches:**
   - Attribute naming: "{start_point} to {end_point}"
   - Session master can rename: "West Branch", "Mountain Line", etc.

**Output:** Hierarchical branch structure for session master review

### Phase 3: Exchange Point Detection

**Input:** Branch hierarchy

**Algorithm:**
1. **Find all sidings** (non-industry track segments with type "siding" or "passing_track")

2. **Determine branch accessibility:**
   - For each siding, identify which branches can reach it
   - Siding reachable by 2+ branches → candidate exchange point

3. **Calculate exchange point utility:**
   - Proximity to branch boundaries (closer = better)
   - Capacity (more cars = better)
   - Track length (longer = better for sorting)

4. **Rank and suggest:**
   - Present top candidates to session master
   - Session master can designate additional exchange points
   - Can also disable auto-detected ones

**Example (UNW 2020):**
- Pure Oil siding: Accessible by Branch A (Black River → Palin) and Branch B (Palin → Cascade South)
- Wooden Village siding: Accessible by Branch B and Branch C (O'Brien → Grain Elevator)
- Both become exchange points

### Session Master Review

After detection:
1. System presents detected subdivisions, branches, exchange points
2. Session master can:
   - Accept as-is
   - Rename branches
   - Merge branches
   - Split branches
   - Create custom branches manually
   - Designate additional exchange points
   - Disable auto-detected exchange points
3. Configuration saved with layout definition

---

## Cost-Based Pathfinding

### Cost Model

Different track elements have different operational costs:

**Track Segments:**
- Mainline travel (single track): **1.0 × linear feet**
- Mainline with opposing traffic: **5.0 × linear feet** (very high cost)
- Siding/passing track: **0.8 × linear feet** (slightly cheaper)
- Industry siding: **1.0 × linear feet**

**Turnouts:**
- Straight through (trailing point): **+5 cost**
- Diverging route (facing point): **+15 cost** (derailment risk)
- Mainline diverging route: **+25 cost** (very high risk)
- Crossover (multiple diverging): **+30 cost** (Kennedy crossover Blue→Red very expensive)

**Direction of Travel:**
- Pulling train (normal): **1.0 multiplier**
- Pushing train in reverse: **2.0 multiplier** for distance
- Pushing train over diverging turnout: **3.0 multiplier** for turnout cost

**Reversals:**
- Reversal (runaround move): **+50 cost** (high but sometimes necessary)
- Backup (no runaround available): **+100 cost** (very expensive)

**Cost Formula:**
```
Total Cost = Σ(segment_cost) + Σ(turnout_cost) + reversal_penalties
```

**Club Configuration:**
All cost values are configurable with sensible defaults. Clubs can adjust relative costs based on their layout characteristics and operational preferences.

### Pathfinding Algorithm

**Algorithm:** Modified Dijkstra's algorithm with directional edges

**Graph Structure:**
- **Nodes:** Module interfaces, turnouts, industries
- **Edges:** Track segments with directional constraints
- **Edge weights:** Operational cost (from cost model above)

**Directional Handling:**
- Some edges bidirectional (through tracks)
- Some edges unidirectional (diverging from trailing point can't turn)
- Direction of travel affects cost (pushing vs pulling)

**Algorithm Steps:**

1. **Build directed graph** from topology model
   - Add edges for track segments
   - Add edges for turnout transitions
   - Apply directional constraints (facing vs trailing)
   - Weight edges with operational costs

2. **Find path** from origin to destination
   - Run Dijkstra's algorithm
   - Consider direction of travel (affects costs)
   - Track reversals required (add to cost)
   - Return lowest-cost path

3. **Detect runaround opportunities**
   - Identify parallel tracks converging at both ends
   - Can span multiple modules (Pure Oil ladder + Bauxen Crate runaround)
   - Reversal cost < backup cost

4. **Optimize path**
   - Prefer trailing-point moves (lower cost)
   - Minimize turnout count
   - Avoid mainline exposure when possible
   - Minimize reversals

**Example: Grain Elevator routing (2 reversals required)**

From Black River Yard to Grain Elevator:
- Path goes through Chare Bros module twice
- First pass: O'Brien direction
- Reversal at O'Brien runaround
- Second pass: backing to Grain Elevator
- Algorithm detects this is lowest-cost path despite reversals
- Alternative paths would require more turnouts or longer distance

### Handling Complex Scenarios

#### Reversals vs Backups

**Reversal (runaround move):**
- Requires parallel track converging at both ends
- Locomotive runs around consist via siding
- Moderate cost (+50)
- Examples: Pure Oil to Bauxen Crate runaround, O'Brien runaround

**Backup (no runaround):**
- Pushing train in reverse
- No way to turn around
- High cost (+100)
- Example: Grain Elevator (stub-end industry)

**Algorithm decision:**
- Prefer reversal over backup when available
- Calculate cost of both options
- U-turn analogy: suggested when no other route OR cost of not U-turning is higher

#### Against-Traffic Routing

**Scenario:** Yellow mainline has clockwise looping traffic

**Routing decision:**
- Moving counter-clockwise on Yellow = very high cost (5.0× multiplier)
- Algorithm avoids unless no alternative
- Example: Crossing Red to Blue via Crossover module when Yellow opposes
  - Short distance across Yellow
  - May still be lowest cost despite high multiplier
  - Flagged to operator as "caution: against traffic"

#### Capacity-Constrained Routing

**Scenario:** Bauxen Crate has 2-car capacity, 3 cars need delivery

**Routing decision:**
1. Assign first 2 cars directly to Bauxen Crate
2. Third car routes to exchange point (Pure Oil siding)
3. Next turn, operator picks up from Pure Oil → delivers to Bauxen Crate
4. Alternative: If operator has multiple branches, deliver on return pass

**Algorithm awareness:**
- Tracks current siding occupancy
- Checks capacity before assignment
- Routes overflow to nearest exchange point
- Suggests multi-turn delivery plans

---

## Operator-Scoped Routing

### Branch Assignment Drives Scope

**Core principle:** Operators are assigned to branches, which defines their routing scope.

**How it works:**

1. **Session master assigns operator to branch(es)**
   - Single branch: "Work O'Brien to Grain Elevator"
   - Multiple branches: "Work Black River to Palin Bridge AND Palin to Cascade South"
   - Entire subdivision: "Work all of OneTrak"

2. **ROG calculates car assignments within scope**
   - Find all cars needing movement to/from/through operator's branches
   - Apply capacity constraints
   - Optimize routing within scope
   - Generate switch list

3. **Foreign cars route to exchange points**
   - Car destined outside operator's scope → route to nearest exchange point
   - Example: Branch C operator receives car for Chesterfield (NTrak) → deliver to exchange point
   - Enables multi-crew coordination

4. **Dynamic scope expansion**
   - Operator completes work on Branch A, asks for more
   - Session master assigns Branch B
   - ROG recalculates: can now deliver foreign cars directly to Branch B
   - Short-line patterns emerge when scope expands

### Foreign Car Handling

**Example:** Car arrives at O'Brien (Branch C) destined for Chesterfield (NTrak Blue Line)

**Routing logic:**
1. Check operator's assigned branches: Branch C (O'Brien → Grain Elevator)
2. Destination (Chesterfield Blue) not in Branch C
3. Find nearest exchange point accessible by branches leading to destination
4. Route: O'Brien → Wooden Village siding (exchange point between Branch C and Branch B)
5. Switch list: "Set out car #5678 at Wooden Village siding"
6. Branch B operator picks up on next turn

**If operator assigned Branch B + Branch C:**
- Destination now within scope
- Route: O'Brien → Palin Bridge → (via NTrak Red or Blue) → Chesterfield Blue
- Switch list: "Deliver car #5678 to Chesterfield Blue (industry siding)"

### Short-Line Pattern Support

**Scenario:** O'Brien and Grain Elevator trade goods constantly

**Traditional yard-based approach:**
- Car loads at O'Brien → deliver to Black River Yard → classify → new train → deliver to Grain Elevator
- Empty at Grain Elevator → return to yard → classify → deliver to O'Brien
- Inefficient: yard visit adds no value

**ROG short-line pattern:**
- Operator assigned Branch C only
- Car loads at O'Brien → deliver directly to Grain Elevator
- Empty at Grain Elevator → deliver directly to O'Brien
- Branch C becomes self-contained short line
- No yard visits needed

**Foreign cars still handled:**
- Car for Chesterfield arrives at O'Brien → route to exchange point
- Branch C operator not forced to handle cars outside scope

**Gameplay value:**
- Realistic short line operations (matches prototype, see yardless-operations.md)
- Focused operator work (satisfying to complete full cycles)
- Promotes specialization (operator becomes expert on their branch)

---

## Car Assignment Logic

### Full Cycle vs On-Demand

**Two approaches considered:**

**Full cycle (recommended):**
- Car onboarded → assign 4-cycle pattern immediately
  - Empty → Load at Industry A → Deliver to Industry B → Load at Industry C → Deliver to Industry D → Return to A
- Promotes layout-wide movement (prevents localization)
- Strategic train building (block cars by destination)
- Mimics real railroad dispatcher knowledge
- **But:** Recalculate on state changes (capacity full, operator reassignment, reality divergence)

**On-demand:**
- Car onboarded → assign next destination only
- Subsequent destinations assigned when car completes current move
- Adapts naturally to state changes
- Handles fluid participation (operator leaves mid-cycle)
- **But:** May localize rolling stock (O'Brien ↔ Grain Elevator only)

**Decision:** **Full cycle with recalculation**
- Assign full 4-cycle when car enters game
- If state changes (capacity, reassignment, etc.), recalculate remaining cycle
- Best of both worlds: strategic + adaptive

### 4-Cycle Pattern

Based on traditional car card/waybill system (see domain-knowledge.md):

1. **Load at Industry A → Deliver to Industry B** (car serves Industry B)
2. **Empty at Industry B → Load at Industry C** (car repositions)
3. **Load at Industry C → Deliver to Industry D** (car serves Industry D)
4. **Empty at Industry D → Return to Industry A** (car repositions for cycle restart)

**Pattern characteristics:**
- 4 movements before repeat
- Different cars on different phases (ensures variety across layout)
- Each industry receives loads and ships loads
- Empty repositioning creates realistic traffic

**ROG calculation:**
1. Car onboarded as empty at location X
2. Find industry needing this car type (commodity match)
3. Assign as Load destination (Industry A)
4. Calculate delivery destination (Industry B) based on commodity
5. Calculate reload point (Industry C) for different commodity
6. Calculate final delivery (Industry D)
7. Return to origin (Industry X or staging yard)

### Commodity Matching

**Car type to commodity specificity:**
- Very specific: Coal car → coal only, cement hopper → cement only
- Moderately specific: Open hopper → coal, ore, gravel, aggregate
- Generic: Boxcar → general merchandise, packaged goods, lumber

**Session master onboarding:**
1. Session master adds car to ROG
2. Specifies car type (boxcar, hopper, tank, gondola, etc.)
3. ROG suggests compatible commodities based on car type
4. Session master can customize: "This boxcar only hauls lumber"
5. ROG uses compatibility list for routing

**Industry matching:**
- Industries specify commodities consumed and produced
- ROG matches available cars to industry needs
- Example: Grain Elevator produces grain → needs covered hopper
- ROG finds available covered hopper, assigns load at Grain Elevator

**"Off layout" destinations:**
- Loads not serviced by layout industries → "off layout" category
- Car routes to staging yard or interchange
- Car recycled into pool for next assignment
- Mimics cars leaving to broader network

### Integration with Car Lifecycle (Q2)

**Dependencies:**
- Q2 will define all car states (empty, assigned, in_transit, spotted, loading, loaded, etc.)
- Routing algorithm needs to know: "Which cars are available for assignment?"
- Answer: Cars in `available`, `empty`, or `staged` states

**State transitions triggered by routing:**
- `available` → `assigned` (car added to train consist)
- `assigned` → `in_transit` (operator accepts train)
- `in_transit` → `spotted` (operator delivers to industry)
- `spotted` → `loading` (industry receives car)
- `loading` → `loaded` (time delay, ready for pickup)
- `loaded` → `assigned` (car added to outbound train)

**Routing awareness of state:**
- Don't assign cars in `loading` state (not available)
- Respect time delays (car loading for 2 sessions, can't move yet)
- Handle `out_of_game` (owner took car home, exclude from routing)

---

## Train Building Workflows

### Scenario 1: Session Master Onboards Rolling Stock

**Context:** Session start, cars being added to game

**Workflow:**
1. **Session master places physical cars**
   - Puts cars on siding or yard track
   - Adds locomotive

2. **Session master onboards cars in ROG**
   - Scans/enters car IDs (reporting marks, numbers)
   - Specifies car types, confirms commodity compatibility
   - ROG marks cars as `staged` at current location

3. **Session master declares train ready**
   - "This train ready for operator assignment"
   - Specifies which branch(es) this train will work

4. **Operator assigned**
   - Session master assigns operator to train + branch
   - ROG calculates destinations for each car based on:
     - Operator's assigned branches (scope)
     - Industry needs (commodity matching)
     - Capacity constraints
     - 4-cycle assignments
   - Generates switch list

5. **Operator receives switch list**
   - Lists each car with destination
   - Ordered by delivery sequence (optimized route)
   - Includes exchange points if foreign cars
   - Operator works the train

**ROG responsibilities:**
- Calculate car destinations dynamically when operator assigned
- Optimize routing within operator's scope
- Generate switch list automatically
- Handle capacity constraints (overflow to exchange points)

### Scenario 2: Operator Builds Own Train

**Context:** Operator joins ongoing session, layout already populated

**Workflow:**
1. **Operator assigned locomotive + branch**
   - Session master: "Take engine #5432, work Black River to Palin Bridge"
   - May start at yard or directly on branch line

2. **ROG identifies available cars in scope**
   - Finds all cars in `available`, `empty`, `staged` states
   - Filters to operator's assigned branches
   - Respects capacity constraints

3. **ROG suggests consist**
   - Presents list of cars needing movement
   - Operator can accept all, select subset, or request different cars
   - Flexible: operator has agency

4. **Operator builds physical train**
   - Collects suggested cars
   - Assembles consist
   - Confirms in ROG: "Train ready"

5. **ROG generates switch list**
   - Based on cars operator actually collected
   - Optimized delivery sequence
   - Operator works the train

**Yard operator variant:**
- Operator assigned to yard only (no branch)
- Responsibility: classify incoming cars, build outbound trains
- ROG shows which cars need blocking for which destinations
- Operator builds consists, assigns to branch-working operators

**Mid-session join:**
- New operator joins active session
- Session master assigns available locomotive + branch
- ROG: "Here are cars needing work on your branch"
- Operator picks up where previous operator left off (fluid participation)

---

## Switch List Generation

**Purpose:** Operational document telling operator what to do

**Content:**
```
Train: West Branch Local
Operator: John Smith
Branch: Black River Yard to Palin Bridge
Date: 2025-12-24

OUTBOUND WORK:
┌─────────┬──────────────┬─────────────────┬──────────────────────┐
│ Car     │ Type         │ Current Loc     │ Destination          │
├─────────┼──────────────┼─────────────────┼──────────────────────┤
│ UP 5678 │ Boxcar       │ Black River Yd  │ Bauxen Crate (spot 2)│
│ BNSF 901│ Hopper       │ Black River Yd  │ Pure Oil siding      │
│ SP 4423 │ Tank         │ Black River Yd  │ Palin Bridge yard    │
└─────────┴──────────────┴─────────────────┴──────────────────────┘

PICKUP WORK:
┌─────────┬──────────────┬─────────────────┬──────────────────────┐
│ Car     │ Type         │ Current Loc     │ Destination          │
├─────────┼──────────────┼─────────────────┼──────────────────────┤
│ CN 7821 │ Gondola      │ Pure Oil ind    │ Black River Yd       │
│ AT 3344 │ Boxcar       │ Bauxen Crate    │ Wooden Village (exch)│
└─────────┴──────────────┴─────────────────┴──────────────────────┘

ROUTE NOTES:
- Pure Oil siding: Exchange point (set out BNSF 901 for Branch B)
- Wooden Village: Exchange point (AT 3344 for Branch C)
- Bauxen Crate: 2-car capacity, spot UP 5678 at door 2

Total: 3 setouts, 2 pickups, 5 cars in final consist
```

**Delivery sequence optimization:**
- Orders setouts/pickups by route geography
- Minimizes backtracking
- Groups exchange point work
- Flags capacity constraints
- Notes runarounds required

**Mobile-friendly format:**
- Responsive design for phones/tablets
- Tap to mark complete
- Real-time sync with other operators
- Connectivity indicator (online/offline/syncing)

---

## Validation Examples

### Example 1: UNW 2020 Layout - Black River to Palin Bridge

**Branch Definition:**
- Start: Black River Yard
- End: Palin Bridge
- Industries: Pure Oil, Bauxen Crate, intermediate sidings

**Operator Assignment:**
- Operator: Alex
- Branch: Black River → Palin Bridge

**Available Cars:**
- 8 cars staged at Black River Yard (various types)
- 3 cars at Pure Oil ready for pickup
- 2 cars at Bauxen Crate ready for pickup

**ROG Routing:**
1. Matches car types to industries needing service
2. Assigns 4-cycle patterns
3. Checks Bauxen Crate capacity (2 cars)
   - 3 cars need delivery → 1 overflows to Pure Oil siding (exchange point)
4. Generates switch list with optimized sequence
5. Operator works Black River → Pure Oil → Bauxen Crate → Palin Bridge

**Result:** 12-car train (within siding length constraints), all work within scope, 1 exchange point setout for Branch B operator.

### Example 2: Grain Elevator (Complex Routing)

**Challenge:** Grain Elevator requires 2 reversals through Chare Bros

**Pathfinding:**
1. Origin: O'Brien
2. Destination: Grain Elevator
3. Algorithm calculates paths:
   - Path A: O'Brien → Chare Bros → back to O'Brien runaround → Chare Bros → backup to Grain Elevator
     - Cost: distance + 2 reversals (+100) + 1 backup (+100) = 200 + distance
   - Path B: Alternative route avoiding Chare Bros
     - Not available (Grain Elevator only accessible via Chare Bros)
4. Path A selected (only option)
5. Switch list flags: "Reversal required at O'Brien runaround, backup to Grain Elevator"

**Operator experience:**
- Receives clear instructions about reversals
- Knows runaround location (O'Brien)
- Expects backup move to Grain Elevator
- Realistic operations (matches prototype practice)

### Example 3: Short-Line Pattern (O'Brien ↔ Grain Elevator)

**Scenario:** Operator assigned Branch C only, two industries trade constantly

**Car Lifecycle:**
1. Empty hopper at O'Brien → load grain → deliver to Grain Elevator
2. Empty hopper at Grain Elevator → load fertilizer → deliver to O'Brien
3. Repeat cycle

**ROG Behavior:**
- Both industries in Branch C (operator's scope)
- No yard visits needed
- Cars cycle O'Brien ↔ Grain Elevator indefinitely
- Branch C becomes self-contained short line
- Foreign cars route to Wooden Village exchange point

**Gameplay outcome:**
- Operator has focused, satisfying work
- Realistic short-line operations
- No wasted yard movements
- Multi-crew coordination via exchange points

---

## Implementation Considerations

### Graph Database Option

Topology is inherently a graph. Consider graph database (Neo4j, etc.) for:
- Natural path queries (Cypher: `MATCH (a)-[*]-(b) RETURN shortestPath(a, b)`)
- Branch detection via graph algorithms
- Runaround detection (cycle detection)
- Complex routing queries

**Trade-off:** Adds technology complexity, but routing queries become simpler and faster.

**Decision deferred to Phase 3** (implementation planning).

### Runtime Performance

**Pathfinding frequency:**
- Session setup: Calculate all initial assignments (one-time)
- Car state change: Recalculate affected car only (incremental)
- Operator reassignment: Recalculate operator's scope (batch)

**Optimization:**
- Pre-calculate branch accessibility matrix (which branches can reach which industries)
- Cache common paths
- Incremental updates instead of full recalculation

**Scalability:**
- Typical layout: 50-100 industries, 200-500 cars, 3-12 operators
- Pathfinding: O(E log V) per car (Dijkstra)
- Acceptable performance on modern hardware

### User Experience

**Session master workflow:**
1. Layout setup: Review detected branches, accept/modify
2. Designate exchange points (or accept auto-detected)
3. Onboard rolling stock
4. Assign operators to branches
5. ROG handles routing automatically

**Operator workflow:**
1. Receive switch list (mobile device)
2. Work train according to list
3. Mark cars delivered (tap interface)
4. ROG updates state in real-time
5. Foreign cars noted as "exchange point setout"

**Visual feedback:**
- Branch overlay on layout diagram (session master view)
- Current operator locations (session master view)
- Connectivity status (all views)
- Capacity warnings (session master view)

---

## Open Questions & Dependencies

### Dependencies on Other Questions

**Q2: Car State Lifecycle**
- Need car states to determine which cars are "available" for routing
- State transitions triggered by routing assignments
- Routing algorithm filters by state

**Q6: Car Identity and Granularity**
- Commodity matching specificity depends on car identity model
- "50-foot insulated boxcar" vs "boxcar" affects routing precision

**Q7: Industry and Spot Definitions**
- Industry capacity determines routing constraints
- Commodity matching depends on industry definitions
- Spot count affects overflow routing

**Q10: Role-Specific Workflows**
- Yard operator workflow depends on routing output
- Session master UX for branch assignment
- Operator switch list presentation

### Open Questions for Future Refinement

**Branch reconfiguration:**
- Can session master change branch definitions mid-session?
- If layout reconfigured (modular), how do branches update?

**Multi-commodity cars:**
- Can tank car haul multiple liquids (cleaned between loads)?
- How to handle commodity flexibility?

**Priority routing:**
- Should some cars/industries get priority (time-sensitive loads)?
- Express vs local freight?

**Real railroad dispatcher knowledge:**
- Research: Does dispatcher know full car lifecycle or just next move?
- Validate full-cycle approach against prototype practice

---

## Related Documents

- [data-models.md](data-models.md) - Layout topology model (Q1)
- [../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md) - Detailed topology spec
- [platform.md](platform.md) - Offline sync, car ownership model (Q26)
- [car-lifecycle.md](car-lifecycle.md) - Car states and transitions (Q2)
- [user-workflows.md](user-workflows.md) - Role-specific operations (Q10)
- [../planning/research/domain-knowledge.md](../planning/research/domain-knowledge.md) - Railroad operations background
- [../planning/research/yardless-operations.md](../planning/research/yardless-operations.md) - Short line patterns
- [decisions/algorithms.md](decisions/algorithms.md) - Routing algorithm decision rationale
- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Q3 tracking

---

## Decision Summary

**Key decisions made:**
- Branch detection: Hybrid auto-detect + session master review
- Pathfinding: Modified Dijkstra with operational costs
- Car assignment: Full 4-cycle with recalculation on state changes
- Operator scoping: Branch assignment drives routing scope
- Foreign cars: Route to exchange points when outside scope
- Short-line support: Self-contained branch operations without yard visits
- Train building: Two workflows (session master onboards, operator builds own)

**This solves the core problem:** Geographic routing enables "build a train for the West Branch" with intelligent car assignments limited to that branch.
