# Phase 2: PRD Development - Progress Tracker

**Status:** In Progress - Question 1 Complete
**Started:** December 7, 2025
**Phase Goal:** Create comprehensive Product Requirements Document answering the 26 critical questions identified during Phase 1

---

## Progress Summary

**Overall Completion:** 6/26 questions addressed

- **Critical (Must Address in PRD):** 1/5 complete
- **Important (Affects Design):** 0/5 complete
- **Refinement (Can Defer to Detailed Design):** 0/11 complete
- **Platform and Deployment Context:** 5/5 complete [x]

---

## Question Status

### CRITICAL (Must Address in PRD)

#### 1. Layout Topology Representation
**Status:** [x] Complete
**Session:** 2025-12-20
**PRD Section:** TBD

**Questions Answered:**
- ✓ What specific data describes a module? (track interfaces, turnouts, segments, industries, sequencing)
- ✓ How are connections between modules represented? (layout-level connection model)
- ✓ For FREE-MO vs NTRAK, what interface data differs? (arbitrary line names, connects_through flag)
- ✓ Do curves matter operationally or just physically? (Physical only - not modeled)
- ✓ How is "this industry is on the West Branch" captured? (Derived from topology, not tagged)

**Deliverables:**
- Comprehensive data model validated on real module examples
- Module definition structure (interfaces, turnouts, segments, industries, sequencing)
- Layout connection model
- Reference specification document created

**Key Decisions:**
- Turnouts: hand, facing_direction, facing/diverging/trailing points
- Sequencing includes track_segments for cost-based routing
- Track inherently bidirectional; diverging routes are directional
- Capacity measured in car count (not physical length)
- Runarounds detectable as parallel tracks converging at both ends
- Visual editor essential (hand-editing YAML impractical)

**Notes:** This is foundational for geographic routing (Q3)

---

#### 2. Car State Lifecycle
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What are ALL the states a car can be in? (staged, assigned, in-transit, spotted, loading, loaded, unloading, empty, available, out-of-game?)
- What triggers state transitions?
- Who can change car states? (system auto, session master, operators?)

**Notes:**

---

#### 3. Geographic Routing Algorithm (THE CORE PROBLEM)
**Status:** [ ] Not Started - NEXT PRIORITY
**Session:** TBD (next session)
**PRD Section:** TBD

**Questions to Answer:**
- What's the actual algorithm for geographic routing?
- How does "build a train for the West Branch" work?
- What defines a "branch"? Explicit in data model or inferred from topology?
- How does system know which industries are "logically grouped"?
- Should routing prefer fewer trains with longer routes, or more trains with focused routes?

**Prerequisites Met:**
- [x] Layout topology representation complete (Q1)
- Topology model provides foundation for routing algorithm

**Known Challenges:**
- Grain Elevator routing requires 2 reversals through Chare Bros twice
- Must handle directional constraints (some routes one-way only)
- Operational cost vs distance optimization
- Branch detection from topology graph

**Notes:** This is THE problem ROG must solve - previous implementation failed here

---

#### 4. Time Modeling
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Is time discrete (session 1, session 2) or continuous?
- Do fast clocks affect ROG or just operator perception?
- How long does loading/unloading take? (fixed time? variable? operator-triggered?)
- Can a car be "loading" and unavailable for pickup?
- If multi-day sessions exist, how is time tracked between days?

**Notes:**

---

#### 5. Staging Mechanics
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Is staging just "off-layout storage" or does it represent specific other locations?
- When a train "goes to staging," where did it actually go (fictionally)?
- How do cars enter staging initially? (session master places them? system generates them?)
- Can staging be empty or must it always have trains ready?
- Difference between hidden staging vs visible interchange track?

**Notes:**

---

### IMPORTANT (Affects Design)

#### 6. Car Identity and Granularity
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Duplicate cars exist - how should ROG handle this?
- Is "box car" sufficient granularity or do you need "50-foot insulated box car"?
- When checking in cars, what identifiers are required vs optional?
- Car ownership tracking - is this core data or metadata?

**Notes:**

---

#### 7. Industry and Spot Definitions
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What's the actual data model for an industry spot?
- Can multiple cars share one spot?
- How is spot capacity determined?
- How specific are commodity matchings? (coal -> any hopper, or coal -> open-top hopper only?)
- Do industries have operating hours or service schedules that affect availability?

**Notes:**

---

#### 8. Train Consist Building Rules
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Beyond geographic routing, what other constraints exist?
- Car order in consist (hazmat rules, reefer placement, weight distribution)?
- Maximum train length limits?
- Locomotive power calculations?
- Or is it purely geographic and everything else is operator judgment?

**Notes:**

---

#### 9. Multi-Session Continuity
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- For permanent layouts: state just persists, right?
- For modular layouts: if layout is disassembled, what happens to car positions?
- Do cars "go into storage" between shows?
- How does state transfer when the same modules are reconfigured differently?

**Notes:**

---

#### 10. Role-Specific Workflows
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- **Yardmaster:** What does a yardmaster actually DO minute-by-minute? Building consists manually or system suggests? Authority to override routing?
- **Dispatcher:** What information do dispatchers need that conductors don't? Controlling track warrants or just coordinating? Relevant for all layouts or only multi-train?
- **Local Switcher vs Through Freight:** Fundamentally different workflows or same workflow with different scope?

**Notes:**

---

### REFINEMENT (Can Defer to Detailed Design)

#### 11. "Checking In Cars" Workflow
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What's the actual UI/process for this?
- What happens if someone checks in a car that's already in the system?
- Can multiple people check in cars simultaneously?
- What validation occurs?

**Notes:**

---

#### 12. Session State Tracking
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- How granular is position tracking? (exact track? just location? "somewhere in this town"?)
- When an operator says "I spotted this car" - what actually gets recorded?
- What happens when reality diverges (operator says car is at Industry A but system thinks Industry B)?
- Is there an "undo" mechanism?

**Notes:**

---

#### 13. Train Assignment and Handoff
**Status:** [x] Partially Answered
**Session:** 2025-12-07 (Phase 1)
**PRD Section:** TBD

**Questions to Answer:**
- How does a car get assigned to a train?
- **ANSWERED:** Cars CANNOT be on multiple trains' manifests simultaneously (prevents deadlocks)
- When operator abandons a train, what happens to cars "in transit"?
- Can another operator just pick up that train's manifest and continue?

**Notes:** Deadlock constraint established; handoff mechanics still need detail

---

#### 14. Missing or Misdelivered Cars
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- When a car is physically missing, what should happen in system?
- Mark as "lost"? Remove from session? Leave in wrong location?
- How is it recovered later?

**Notes:**

---

#### 15. Cars Going "Out of Game"
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- When owner takes their cars home, how is this handled?
- Does system need to track "borrowed" vs "permanent" rolling stock?
- Can you query "show me only cars that are actually here today"?

**Notes:**

---

#### 16. Layout Changes Mid-Session
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Unlikely but: what if a module is removed mid-session?
- What happens to cars that were on that module?
- Can system detect this and flag affected operations?

**Notes:**

---

#### 17. Scale Operational Differences
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Do HO/N/O scales affect ROG operations?
- Or is it purely physical handling (smaller cars harder to manipulate)?
- Do different scales have different operational conventions in the community?

**Notes:**

---

#### 18. Capacity Constraints
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- How does system know track is "full"?
- Is it car count? Physical length? Visual crowding?
- What happens when operator tries to spot a car but track is full?

**Notes:**

---

#### 19. Session End State
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What defines a "natural breakpoint"?
- Specific trains completed? All assigned work done? Time limit? Operator exhaustion?
- Can a session "save" mid-train (train halfway through route)?
- What needs to be documented for next operator to continue?

**Notes:**

---

#### 20. Success Metrics
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Beyond "work completed," are there other success measures?
- Time efficiency? Number of moves? Errors made?
- Or purely intrinsic satisfaction?

**Notes:**

---

#### 21. Interchange Between Railroads
**Status:** [ ] Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- In modular settings with multiple "railroads" (different clubs?), how does interchange work?
- Is this just staging by another name or different mechanics?
- Do cars need routing info across "foreign" railroads?

**Notes:**

---

### PLATFORM AND DEPLOYMENT CONTEXT

#### 22. Technical Platform
**Status:** [x] Complete
**Session:** 2025-12-07 (Phase 2)
**PRD Section:** TBD

**Questions to Answer:**
- Where does ROG run? (web browser, native app, tablet-optimized?)
- Online vs offline operation - critical for train shows with unreliable WiFi
- Multiple device sync - can conductor use phone while yardmaster uses tablet?
- Does ROG need to work when internet is completely unavailable?

**Decisions Made:**
- **Platform:** Progressive Web App (PWA) - installs from browser, no app store required
- **Backend:** Cloud-hosted (Azure or cost-effective alternative)
- **Interface:** Mobile-first for operators (phones), desktop for session setup (keyboard/mouse)
- **Offline capability:** Tolerates 30+ second connection drops, queues changes for sync
- **Connectivity indicator:** Proactive, unobtrusive (small icon showing online/offline/syncing)
- **UX requirement:** Clear onboarding instructions for PWA install (older demographic consideration)
- **Multi-device:** Real-time collaboration, server-authoritative with optimistic sync

**Notes:** PWA verified on iPhone (Squoosh test app). Install process requires explicit instructions but experience is app-like once installed.

---

#### 23. Integration and Compatibility
**Status:** [x] Complete
**Session:** 2025-12-07 (Phase 2)
**PRD Section:** TBD

**Questions to Answer:**
- Can ROG import existing data from JMRI or other systems?
- Does ROG need to export data (for printing, archiving, analysis)?
- Is there value in being compatible with existing standards/formats?

**Decisions Made:**
- **MVP Scope:** Game and player stat tracking (session metrics, operator metrics, system health)
- **Future features (not MVP):**
  - JMRI import (architect to allow, but defer implementation)
  - Post-session export (JSON/CSV for records/analysis)
  - Layout/roster sharing (community feature)
- **Paper export:** REMOVED from MVP scope - disaster recovery scenario acceptable without paper backup
- **Rationale:** Paper backup changes gameplay dynamics (static vs evolving world), adds complexity without clear MVP value

**Notes:** Stat tracking enables future gamification (leaderboards, performance metrics). Post-session reporting provides session quality insights.

---

#### 24. Project Scope and Audience
**Status:** [x] Complete
**Session:** 2025-12-07 (Phase 2)
**PRD Section:** TBD

**Questions to Answer:**
- Is ROG for your personal use, your club, or the broader community?
- Open source vs proprietary?
- This affects decisions about generalization vs. solving your specific needs

**Decisions Made:**
- **Audience:** Community-wide - any club, operator, layout owner can use
- **Development:** Open development (public GitHub repository)
- **Commercial model:** Freemium - free for single operator, paid for multi-operator sessions
- **License:** Decision deferred until implementation phase
- **Deployment:** Commercial deployment required to recoup hosting/development costs

**Notes:** Avoiding self-hosted model to prevent "every club becomes sysadmin" barrier to adoption. Cloud-based reduces friction.

---

#### 25. Physical Artifacts and Backup
**Status:** [x] Complete
**Session:** 2025-12-07 (Phase 2)
**PRD Section:** TBD

**Questions to Answer:**
- Is ROG purely digital or are there hybrid scenarios (some paper, some digital)?
- Do operators need printed backup in case devices fail?
- QR codes on modules to identify them to the system?

**Decisions Made:**
- **MVP:** Pure digital (no paper dependency)
- **Offline capability:** PWA service workers provide offline tolerance
- **Disaster recovery:** If all devices fail catastrophically, session disruption is acceptable for MVP
- **Future consideration:** Paper backup for disaster recovery (noted that this changes gameplay from evolving world to static list)
- **Session state:** If devices fail but players were already in offline cached mode, they can continue

**Notes:** Offline cached state provides inherent backup if connectivity lost during active session. Complete device failure is edge case acceptable for V1.

---

#### 26. User Authentication and Authority
**Status:** [x] Complete
**Session:** 2025-12-07 (Phase 2)
**PRD Section:** TBD

**Questions to Answer:**
- Who can modify layout configuration?
- Who can check in cars?
- Who can change car positions/states?
- Or is it "trust everyone" since it's a collaborative game?

**Decisions Made:**
- **Authentication:** User accounts (email-based)
- **Ownership model:** Club/subscription ownership
- **Session master role:** Assignable role (not just subscription owner), has "god mode" privileges
- **Role switching:** Session master can switch to "Operator" profile during session, retain admin access
- **Operator permissions:** Can only modify cars "owned" by their assigned train
- **Admin capabilities:**
  - Override car positions (fix reality divergence)
  - Reassign cars between trains
  - Mark cars as "out of game"
  - End/freeze session
  - View all operators, connectivity status, system health
- **Conflict resolution:** Admin changes are authoritative (fixing reality), player syncs that conflict are rejected with notification
- **Tracking:** System logs admin overrides, surfaces conflicts to session master console

**Notes:** Car ownership model prevents most conflicts inherently. Admin authority handles edge cases (missing cars, owner took equipment home, etc.).

---

## Session Log

_Track which sessions worked on which questions_

### Session: 2025-12-07 - Platform Architecture
**Questions Addressed:**
- Q22: Technical Platform ([x] Complete)
- Q23: Integration and Compatibility ([x] Complete)
- Q24: Project Scope and Audience ([x] Complete)
- Q25: Physical Artifacts and Backup ([x] Complete)
- Q26: User Authentication and Authority ([x] Complete)

**Decisions Made:**
- PWA architecture with cloud backend (no app store required)
- Offline-tolerant with optimistic sync, 30+ second drop tolerance
- Proactive connectivity indicator (unobtrusive status icon)
- Car ownership model prevents conflicts, admin authority resolves edge cases
- Stat tracking in MVP scope, paper export removed from MVP
- Freemium business model: free single operator, paid multi-operator
- Session master = assignable role with god mode, can switch to operator

**Next Steps:**
- Q1 & Q3: Layout Topology Representation + Geographic Routing Algorithm (THE core problem)

---

### Session: 2025-12-20 - Layout Topology Representation
**Questions Addressed:**
- Q1: Layout Topology Representation ([x] Complete)

**Decisions Made:**
- Comprehensive module data model: interfaces, turnouts, segments, industries, sequencing
- Layout connection model for module-to-module connections
- Turnouts: hand, facing_direction, facing/diverging/trailing points
- Sequencing includes track_segments for cost-based routing
- Capacity in car count (not physical length)
- Visual editor essential (hand-editing YAML impractical)
- Runarounds detectable from parallel tracks converging at both ends

**Module Examples Validated:**
- Al - Chesterfield (complex multi-track with industry sidings)
- Scott - Kennedy (crossover demonstrating directional transitions)
- Pure Oil (3-track ladder forming multi-module runaround)

**Next Steps:**
- Q3: Geographic Routing Algorithm (using topology model as foundation)

---

## PRD Document Status

**Location:** TBD (likely `docs/ROG-PRD.md`)
**Status:** Not Created
**Last Updated:** N/A

**Sections Completed:**
- None yet (answers being captured in progress tracker)

**Sections In Progress:**
- Platform architecture (Q22-26 complete, needs synthesis)
- Layout topology (Q1 complete, needs formal write-up)

**Sections Pending:**
- All other sections

---

## Notes and Insights

### Critical Path
1. ~~Platform questions (22-26)~~ COMPLETE ✓
2. ~~Layout Topology (Q1)~~ COMPLETE ✓
3. **Geographic Routing (Q3)** - NEXT PRIORITY
4. Car State Lifecycle (Q2) - needed after routing
5. Time modeling (Q4) - affects session progression
6. Then address Important/Refinement questions

### Dependencies Identified
- Geographic routing REQUIRES layout topology representation ✓
- Car state lifecycle affects session state tracking
- Role workflows depend on understanding core operations first
- Branch detection algorithm depends on topology graph (Q1 → Q3)

### Open Questions for User
_Questions that arise during PRD development that need user input_

**From 2025-12-20 session:**
- "A few details we should discuss more later" - what were these specifically?
- Session Master UX: What did Visio workflow look like?
- Three-way turnouts: How to model?
- Wye turnouts: Complex geometry handling?
- Multi-level layouts: How to represent staging above/below?

---

## Phase 2 Completion Criteria

Phase 2 is complete when:
- [ ] All CRITICAL questions (1-5) have been answered in PRD
  - [x] Q1: Layout Topology Representation
  - [ ] Q2: Car State Lifecycle
  - [ ] Q3: Geographic Routing Algorithm
  - [ ] Q4: Time Modeling
  - [ ] Q5: Staging Mechanics
- [ ] All IMPORTANT questions (6-10) have been answered or explicitly deferred with rationale
- [x] All PLATFORM questions (22-26) have been answered
- [ ] REFINEMENT questions (11-21) have been triaged (answer now vs defer to detailed design)
- [ ] PRD document is comprehensive enough for Phase 3 (Implementation Planning)
- [ ] User confirms PRD captures the vision for ROG

**Target:** Not rushing - quality over speed. Multiple sessions expected.

**Progress:** 6/26 questions answered (23% complete)
- 1/5 Critical questions complete
- 5/5 Platform questions complete
- Ready to tackle Q3 (Geographic Routing) next session
