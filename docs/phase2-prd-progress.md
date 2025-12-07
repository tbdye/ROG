# Phase 2: PRD Development - Progress Tracker

**Status:** Not Started
**Started:** TBD
**Phase Goal:** Create comprehensive Product Requirements Document answering the 26 critical questions identified during Phase 1

---

## Progress Summary

**Overall Completion:** 0/26 questions addressed

- **Critical (Must Address in PRD):** 0/5 complete
- **Important (Affects Design):** 0/5 complete
- **Refinement (Can Defer to Detailed Design):** 0/11 complete
- **Platform and Deployment Context:** 0/5 complete

---

## Question Status

### CRITICAL (Must Address in PRD)

#### 1. Layout Topology Representation
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What specific data describes a module? (dimensions, track endpoints, switch positions, facing/trailing orientation?)
- How are connections between modules represented?
- For FREE-MO vs NTRAK, what interface data differs?
- Do curves matter operationally or just physically?
- How is "this industry is on the West Branch" captured in the data model?

**Notes:** This is foundational for geographic routing

---

#### 2. Car State Lifecycle
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What are ALL the states a car can be in? (staged, assigned, in-transit, spotted, loading, loaded, unloading, empty, available, out-of-game?)
- What triggers state transitions?
- Who can change car states? (system auto, session master, operators?)

**Notes:**

---

#### 3. Geographic Routing Algorithm (THE CORE PROBLEM)
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What's the actual algorithm for geographic routing?
- How does "build a train for the West Branch" work?
- What defines a "branch"? Explicit in data model or inferred from topology?
- How does system know which industries are "logically grouped"?
- Should routing prefer fewer trains with longer routes, or more trains with focused routes?

**Notes:** This is THE problem ROG must solve - previous implementation failed here

---

#### 4. Time Modeling
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- What's the actual data model for an industry spot?
- Can multiple cars share one spot?
- How is spot capacity determined?
- How specific are commodity matchings? (coal → any hopper, or coal → open-top hopper only?)
- Do industries have operating hours or service schedules that affect availability?

**Notes:**

---

#### 8. Train Consist Building Rules
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
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
**Status:** ✅ Partially Answered
**Session:** 2024-12-07 (Phase 1)
**PRD Section:** TBD

**Questions to Answer:**
- How does a car get assigned to a train?
- **ANSWERED:** Cars CANNOT be on multiple trains' manifests simultaneously (prevents deadlocks)
- When operator abandons a train, what happens to cars "in transit"?
- Can another operator just pick up that train's manifest and continue?

**Notes:** Deadlock constraint established; handoff mechanics still need detail

---

#### 14. Missing or Misdelivered Cars
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- When a car is physically missing, what should happen in system?
- Mark as "lost"? Remove from session? Leave in wrong location?
- How is it recovered later?

**Notes:**

---

#### 15. Cars Going "Out of Game"
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- When owner takes their cars home, how is this handled?
- Does system need to track "borrowed" vs "permanent" rolling stock?
- Can you query "show me only cars that are actually here today"?

**Notes:**

---

#### 16. Layout Changes Mid-Session
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Unlikely but: what if a module is removed mid-session?
- What happens to cars that were on that module?
- Can system detect this and flag affected operations?

**Notes:**

---

#### 17. Scale Operational Differences
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Do HO/N/O scales affect ROG operations?
- Or is it purely physical handling (smaller cars harder to manipulate)?
- Do different scales have different operational conventions in the community?

**Notes:**

---

#### 18. Capacity Constraints
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- How does system know track is "full"?
- Is it car count? Physical length? Visual crowding?
- What happens when operator tries to spot a car but track is full?

**Notes:**

---

#### 19. Session End State
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Beyond "work completed," are there other success measures?
- Time efficiency? Number of moves? Errors made?
- Or purely intrinsic satisfaction?

**Notes:**

---

#### 21. Interchange Between Railroads
**Status:** ⬜ Not Started
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
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Where does ROG run? (web browser, native app, tablet-optimized?)
- Online vs offline operation - critical for train shows with unreliable WiFi
- Multiple device sync - can conductor use phone while yardmaster uses tablet?
- Does ROG need to work when internet is completely unavailable?

**Notes:** Offline capability may be critical constraint

---

#### 23. Integration and Compatibility
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Can ROG import existing data from JMRI or other systems?
- Does ROG need to export data (for printing, archiving, analysis)?
- Is there value in being compatible with existing standards/formats?

**Notes:**

---

#### 24. Project Scope and Audience
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Is ROG for your personal use, your club, or the broader community?
- Open source vs proprietary?
- This affects decisions about generalization vs. solving your specific needs

**Notes:**

---

#### 25. Physical Artifacts and Backup
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Is ROG purely digital or are there hybrid scenarios (some paper, some digital)?
- Do operators need printed backup in case devices fail?
- QR codes on modules to identify them to the system?

**Notes:**

---

#### 26. User Authentication and Authority
**Status:** ⬜ Not Started
**Session:** TBD
**PRD Section:** TBD

**Questions to Answer:**
- Who can modify layout configuration?
- Who can check in cars?
- Who can change car positions/states?
- Or is it "trust everyone" since it's a collaborative game?

**Notes:**

---

## Session Log

_Track which sessions worked on which questions_

### Session: TBD
**Questions Addressed:**
- TBD

**Decisions Made:**
- TBD

**Next Steps:**
- TBD

---

## PRD Document Status

**Location:** TBD (likely `docs/ROG-PRD.md`)
**Status:** Not Created
**Last Updated:** N/A

**Sections Completed:**
- None yet

**Sections In Progress:**
- None yet

**Sections Pending:**
- All sections

---

## Notes and Insights

### Critical Path
1. Start with Questions 3 (Geographic Routing) and 1 (Layout Topology) - these are interconnected and foundational
2. Then address Question 2 (Car State Lifecycle) - needed for operational mechanics
3. Platform questions (22-26) should be addressed early to constrain design space
4. Refinement questions (11-21) can be addressed as details emerge

### Dependencies Identified
- Geographic routing requires layout topology representation
- Car state lifecycle affects session state tracking
- Role workflows depend on understanding core operations first

### Open Questions for User
_Questions that arise during PRD development that need user input_

- None yet

---

## Phase 2 Completion Criteria

Phase 2 is complete when:
- [ ] All CRITICAL questions (1-5) have been answered in PRD
- [ ] All IMPORTANT questions (6-10) have been answered or explicitly deferred with rationale
- [ ] All PLATFORM questions (22-26) have been answered
- [ ] REFINEMENT questions (11-21) have been triaged (answer now vs defer to detailed design)
- [ ] PRD document is comprehensive enough for Phase 3 (Implementation Planning)
- [ ] User confirms PRD captures the vision for ROG

**Target:** Not rushing - quality over speed. Multiple sessions expected.
