# Process & Philosophy Decisions

**Category:** Design Philosophy & Process
**Last Updated:** December 21, 2025

---

## Reality-First Data Model
#error-recovery #session-management #usability #car-identity #final #mvp-critical

**Decision:** System tracks actual car positions, not theoretical. Operator actions authoritative.
**Date:** 2025-12-07
**Status:** Final

**Context:**
Sessions are messy—derailments, incomplete work, operator errors. System state diverges from reality.

**Rationale:**
ROG observes and assists, doesn't prescribe. When operator says "I spotted car #5678 at Industry C," that's the truth—system updates to match reality.

**Implementation:**
- Operator's car movements authoritative for their assigned cars
- Admin can override to fix reality divergence (tracked and surfaced)
- Session master dashboard shows conflicts/overrides

**Trade-offs:**
- Gained: System reflects actual gameplay, flexible error recovery
- Lost: Perfect theoretical state consistency (not needed)

**Related PRD Section:** [../overview.md](../overview.md#key-architectural-principles)

---

## Fluid Participation Support
#session-management #fluid-participation #usability #train-show #final #mvp-critical

**Decision:** System handles operators joining/leaving/switching trains mid-session
**Date:** 2025-12-07
**Status:** Final

**Context:**
Real sessions: people arrive late, leave early, abandon trains, pick up others' work.

**Rationale:**
System must handle messiness gracefully:
- Operators can join mid-session
- Abandoned trains can be picked up by others
- Cars don't disappear when operator leaves
- State persists across session pauses

**Implementation:**
- Train ownership transferable
- Car states independent of operator presence
- Session master can reassign trains
- Multi-day sessions supported

**Related PRD Section:** [../platform.md](../platform.md#session-master-role)

---

## Configuration Variability Assumption
#modular-layout #session-management #topology #train-show #final #mvp-critical

**Decision:** Assume modular layouts never have same configuration twice
**Date:** 2025-12-07
**Status:** Final

**Context:**
Modular club layouts reconfigure between shows, different modules each time.

**Rationale:**
- System must handle unknown roster until setup day
- Topology can change completely between sessions
- No assumptions about "standard configuration"
- Quick reconfiguration essential

**Implementation:**
- Module definitions reusable
- Layout connections flexible
- Session setup workflow assumes new configuration
- Visual editor for rapid topology definition

**Related PRD Section:** [../data-models.md](../data-models.md#layout-topology-model)
