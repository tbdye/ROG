# ROG - Open Questions

**Last Updated:** December 21, 2025
**Phase:** Phase 2 - PRD Development
**Progress:** 6/26 questions answered (23%)

---

## Overview

This document tracks all 26 questions identified during Phase 1 assumption testing. These questions must be addressed to complete the PRD.

**See also:** [../planning/phase2-progress.md](../planning/phase2-progress.md) for detailed session-by-session tracking.

---

## Progress Summary

| Category | Complete | Total | % Complete |
|----------|----------|-------|------------|
| Critical (Must Address in PRD) | 1 | 5 | 20% |
| Important (Affects Design) | 0 | 5 | 0% |
| Refinement (Can Defer) | 0 | 11 | 0% |
| Platform and Deployment | 5 | 5 | 100% |
| **TOTAL** | **6** | **26** | **23%** |

---

## CRITICAL (Must Address in PRD)

### Q1: Layout Topology Representation ✓

**Status:** Complete (2025-12-20)
**Documented in:** [data-models.md](data-models.md#layout-topology-model)

**Questions:**
- What specific data describes a module?
- How are connections between modules represented?
- For FREE-MO vs NTRAK, what interface data differs?
- Do curves matter operationally or just physically?
- How is "this industry is on the West Branch" captured in the data model?

**Answers:**
- Module data model: interfaces, turnouts, track segments, industries, sequencing
- Layout connection model: module instances + connections
- Arbitrary line names, `connects_through` flag differentiates
- Curves: physical only (not modeled operationally)
- Branch membership: derived from topology, not tagged

**Key deliverables:**
- Complete data model validated on real modules
- Detailed spec: [../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md)

---

### Q2: Car State Lifecycle

**Status:** Not Started
**Will be documented in:** [car-lifecycle.md](car-lifecycle.md)

**Questions:**
- What are ALL the states a car can be in?
- What triggers state transitions?
- Who can change car states? (system auto, session master, operators?)

**Dependencies:**
- Car ownership model (Q26) ✓ Complete
- Reality-first principle established

---

### Q3: Geographic Routing Algorithm (THE CORE PROBLEM)

**Status:** Not Started - **NEXT PRIORITY**
**Will be documented in:** [routing-algorithm.md](routing-algorithm.md)

**Questions:**
- What's the actual algorithm for geographic routing?
- How does "build a train for the West Branch" work?
- What defines a "branch"? Explicit in data model or inferred from topology?
- How does system know which industries are "logically grouped"?
- Should routing prefer fewer trains with longer routes, or more trains with focused routes?

**Prerequisites:**
- ✓ Layout topology representation (Q1) complete
- Topology model provides foundation for routing algorithm

**Known challenges:**
- Grain Elevator routing requires 2 reversals through Chare Bros twice
- Must handle directional constraints (some routes one-way only)
- Operational cost vs distance optimization
- Branch detection from topology graph

**Critical context:**
Previous implementation failed here. This is THE problem ROG exists to solve.

---

### Q4: Time Modeling

**Status:** Not Started

**Questions:**
- Is time discrete (session 1, session 2) or continuous?
- Do fast clocks affect ROG or just operator perception?
- How long does loading/unloading take? (fixed time? variable? operator-triggered?)
- Can a car be "loading" and unavailable for pickup?
- If multi-day sessions exist, how is time tracked between days?

---

### Q5: Staging Mechanics

**Status:** Not Started

**Questions:**
- Is staging just "off-layout storage" or does it represent specific other locations?
- When a train "goes to staging," where did it actually go (fictionally)?
- How do cars enter staging initially? (session master places them? system generates them?)
- Can staging be empty or must it always have trains ready?
- Difference between hidden staging vs visible interchange track?

---

## IMPORTANT (Affects Design)

### Q6: Car Identity and Granularity

**Status:** Not Started
**Will be documented in:** [data-models.md](data-models.md#car-identity-model)

**Questions:**
- Duplicate cars exist - how should ROG handle this?
- Is "box car" sufficient granularity or do you need "50-foot insulated box car"?
- When checking in cars, what identifiers are required vs optional?
- Car ownership tracking - is this core data or metadata?

---

### Q7: Industry and Spot Definitions

**Status:** Not Started
**Will be documented in:** [data-models.md](data-models.md#industry-and-spot-model)

**Questions:**
- What's the actual data model for an industry spot?
- Can multiple cars share one spot?
- How is spot capacity determined?
- How specific are commodity matchings? (coal -> any hopper, or coal -> open-top hopper only?)
- Do industries have operating hours or service schedules that affect availability?

---

### Q8: Train Consist Building Rules

**Status:** Not Started

**Questions:**
- Beyond geographic routing, what other constraints exist?
- Car order in consist (hazmat rules, reefer placement, weight distribution)?
- Maximum train length limits?
- Locomotive power calculations?
- Or is it purely geographic and everything else is operator judgment?

---

### Q9: Multi-Session Continuity

**Status:** Not Started

**Questions:**
- For permanent layouts: state just persists, right?
- For modular layouts: if layout is disassembled, what happens to car positions?
- Do cars "go into storage" between shows?
- How does state transfer when the same modules are reconfigured differently?

**Platform support:**
- State persistence enabled (Q26)
- Multi-day sessions architecturally supported

---

### Q10: Role-Specific Workflows

**Status:** Not Started
**Will be documented in:** [user-workflows.md](user-workflows.md)

**Questions:**
- **Yardmaster:** What does a yardmaster actually DO minute-by-minute? Building consists manually or system suggests? Authority to override routing?
- **Dispatcher:** What information do dispatchers need that conductors don't? Controlling track warrants or just coordinating? Relevant for all layouts or only multi-train?
- **Local Switcher vs Through Freight:** Fundamentally different workflows or same workflow with different scope?

**Partial answer:**
- Session master role defined (Q26) - see [platform.md](platform.md#session-master-role)

---

## REFINEMENT (Can Defer to Detailed Design)

### Q11: "Checking In Cars" Workflow

**Status:** Not Started

**Questions:**
- What's the actual UI/process for this?
- What happens if someone checks in a car that's already in the system?
- Can multiple people check in cars simultaneously?
- What validation occurs?

---

### Q12: Session State Tracking

**Status:** Not Started

**Questions:**
- How granular is position tracking? (exact track? just location? "somewhere in this town"?)
- When an operator says "I spotted this car" - what actually gets recorded?
- What happens when reality diverges (operator says car is at Industry A but system thinks Industry B)?
- Is there an "undo" mechanism?

**Partial answer:**
- Reality-first principle: operator's action is authoritative
- Admin can override to fix divergence (Q26)

---

### Q13: Train Assignment and Handoff

**Status:** Partially Answered

**Questions:**
- How does a car get assigned to a train?
- **ANSWERED:** Cars CANNOT be on multiple trains' manifests simultaneously (prevents deadlocks)
- When operator abandons a train, what happens to cars "in transit"?
- Can another operator just pick up that train's manifest and continue?

**Answered:**
- Car ownership model prevents multi-train assignment (Q26)
- See [platform.md](platform.md#car-ownership-model)

**Still need:**
- Handoff mechanics details
- Abandoned train handling

---

### Q14: Missing or Misdelivered Cars

**Status:** Not Started

**Questions:**
- When a car is physically missing, what should happen in system?
- Mark as "lost"? Remove from session? Leave in wrong location?
- How is it recovered later?

**Platform support:**
- Admin can mark cars "out of game" (Q26)

---

### Q15: Cars Going "Out of Game"

**Status:** Not Started

**Questions:**
- When owner takes their cars home, how is this handled?
- Does system need to track "borrowed" vs "permanent" rolling stock?
- Can you query "show me only cars that are actually here today"?

**Platform support:**
- Admin can mark cars "out of game" (Q26)

---

### Q16: Layout Changes Mid-Session

**Status:** Not Started

**Questions:**
- Unlikely but: what if a module is removed mid-session?
- What happens to cars that were on that module?
- Can system detect this and flag affected operations?

---

### Q17: Scale Operational Differences

**Status:** Not Started

**Questions:**
- Do HO/N/O scales affect ROG operations?
- Or is it purely physical handling (smaller cars harder to manipulate)?
- Do different scales have different operational conventions in the community?

---

### Q18: Capacity Constraints

**Status:** Partially Answered

**Questions:**
- How does system know track is "full"?
- Is it car count? Physical length? Visual crowding?
- What happens when operator tries to spot a car but track is full?

**Partial answer:**
- Capacity measured in car count (Q1)
- Offline spotting can cause capacity conflicts → session master notified (Q26)

**Still need:**
- Operator UX when attempting to spot to full track
- Validation workflow

---

### Q19: Session End State

**Status:** Not Started

**Questions:**
- What defines a "natural breakpoint"?
- Specific trains completed? All assigned work done? Time limit? Operator exhaustion?
- Can a session "save" mid-train (train halfway through route)?
- What needs to be documented for next operator to continue?

---

### Q20: Success Metrics

**Status:** Partially Answered

**Questions:**
- Beyond "work completed," are there other success measures?
- Time efficiency? Number of moves? Errors made?
- Or purely intrinsic satisfaction?

**Partial answer:**
- Stat tracking in MVP scope (Q23)
- Session metrics, operator metrics, system health
- See [platform.md](platform.md#integration-and-compatibility)

---

### Q21: Interchange Between Railroads

**Status:** Not Started

**Questions:**
- In modular settings with multiple "railroads" (different clubs?), how does interchange work?
- Is this just staging by another name or different mechanics?
- Do cars need routing info across "foreign" railroads?

---

## PLATFORM AND DEPLOYMENT CONTEXT

### Q22: Technical Platform ✓

**Status:** Complete (2025-12-07)
**Documented in:** [platform.md](platform.md#q22-technical-platform)

**Decisions:**
- Progressive Web App (PWA) with cloud backend
- Mobile-first for operators, desktop for session master
- Offline tolerance: 30+ second drops
- Real-time multi-device collaboration
- Proactive connectivity indicator

---

### Q23: Integration and Compatibility ✓

**Status:** Complete (2025-12-07)
**Documented in:** [platform.md](platform.md#q23-integration-and-compatibility)

**Decisions:**
- MVP: Stat tracking (session, operator, system health metrics)
- Future (not MVP): JMRI import/export, post-session export, layout sharing
- Paper backup removed from scope

---

### Q24: Project Scope and Audience ✓

**Status:** Complete (2025-12-07)
**Documented in:** [platform.md](platform.md#q24-project-scope-and-audience)

**Decisions:**
- Community-wide (any club, operator, layout owner)
- Open development (public GitHub)
- Freemium model (free single operator, paid multi-operator)
- License deferred to implementation

---

### Q25: Physical Artifacts and Backup ✓

**Status:** Complete (2025-12-07)
**Documented in:** [platform.md](platform.md#q25-physical-artifacts-and-backup)

**Decisions:**
- Pure digital for MVP
- Offline cached state provides backup
- Complete device failure acceptable edge case for V1

---

### Q26: User Authentication and Authority ✓

**Status:** Complete (2025-12-07)
**Documented in:** [platform.md](platform.md#q26-user-authentication-and-authority)

**Decisions:**
- Email-based auth
- Session master role (assignable, god mode)
- Car ownership model prevents conflicts
- Admin authority fixes reality divergence
- Conflict tracking and logging

---

## Dependencies

### Critical Path

1. ✓ Platform questions (Q22-26) - COMPLETE
2. ✓ Layout Topology (Q1) - COMPLETE
3. **→ Geographic Routing (Q3) - NEXT**
4. Car State Lifecycle (Q2)
5. Time Modeling (Q4)
6. Then address Important/Refinement questions

### Question Dependencies

**Q3 (Routing) depends on:**
- ✓ Q1 (Topology) - Complete

**Q2 (Car Lifecycle) relates to:**
- ✓ Q26 (Car ownership) - Complete
- Q6 (Car identity)

**Q10 (Workflows) relates to:**
- ✓ Q26 (Session master role) - Complete
- Q12 (State tracking)

**Q12 (State Tracking) relates to:**
- ✓ Q26 (Admin authority) - Complete
- Q2 (Car lifecycle)

---

## Next Actions

**Immediate priority:** Q3 - Geographic Routing Algorithm

**Why Q3 next:**
- Prerequisites complete (topology model exists)
- Core differentiating feature of ROG
- Previous implementation failed here
- Foundational for other decisions (car routing, train building, etc.)

**After Q3:**
- Q2 (Car lifecycle) - enables state tracking
- Q4 (Time modeling) - affects session progression
- Q10 (Workflows) - role-specific UX

---

## Related Documents

- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Detailed session tracking
- [../planning/current-work.md](../planning/current-work.md) - What we're working on now
- [overview.md](overview.md) - Problem statement and scope
- [platform.md](platform.md) - Platform decisions (Q22-26)
- [data-models.md](data-models.md) - Data model decisions (Q1, Q6, Q7)
- [routing-algorithm.md](routing-algorithm.md) - Routing design (Q3)
- [user-workflows.md](user-workflows.md) - Role workflows (Q10)
- [car-lifecycle.md](car-lifecycle.md) - Car states (Q2)
