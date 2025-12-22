# ROG - Overview & Problem Statement

**Last Updated:** December 21, 2025
**Status:** Problem statement defined, MVP scope in progress

---

## The Problem

Model railroad operations gaming transforms static layouts into interactive experiences where car movements have purpose. Current tools—paper-based car card systems and software like JMRI OperationsPro—face critical limitations:

### The Geographic Routing Failure

**The core problem ROG exists to solve:**

Previous operations systems lack **layout topology awareness**, making geographic routing impossible. Without understanding track connectivity and branch structure, systems generate random industry matchings that force operators to traverse entire layouts even when working "local" assignments.

**Real user experience (previous implementation):**
> "The generation algorithm had no awareness of layout configuration, so it was impossible to create trains that operated on a particular line, and operators were forced to go through the entire layout to work their train. This was a common complaint."

This breaks the short line operational model where crews work focused geographic areas (branches, towns, industrial districts).

### Secondary Pain Points

**Setup tedium (paper systems):**
- Build templates for hundreds of car cards
- Print, cut, manually write waybill information
- Physical placement and organization
- Pencil in car assignments based on available rolling stock
- All one-time use per session

**Learning curve (software systems):**
- JMRI requires extensive upfront data entry
- Documentation assumes prior knowledge
- Dated interfaces, steep complexity

**Modular layout challenges:**
- Track configurations change between setups
- Car rosters unknown until setup day
- Most software assumes permanent layouts
- Reconfiguration is difficult or impossible

**Real-time synchronization:**
- Physical reality diverges from software state (derailments, incomplete work)
- Current systems handle reconciliation poorly
- Rigid train-building workflows (all-or-nothing)

**Modern expectations gap:**
- Desktop-oriented with dated interfaces
- No mobile-first design
- No real-time multi-device collaboration
- No cloud synchronization

---

## The Vision

**ROG (Railroad Operations Game)** is a geographic-aware operations assistant that:

1. **Understands layout topology** - Models track connectivity, branches, junctions
2. **Routes cars geographically** - Enables "build a train for the West Branch"
3. **Adapts to reality** - Tracks actual car positions, handles fluid participation
4. **Works anywhere** - Modular setups, permanent layouts, changing configurations
5. **Modern experience** - PWA, mobile-first, offline-tolerant, real-time collaboration

**Core principle:** ROG observes and assists operations, doesn't prescribe them. The system tracks where cars actually are (reality-first), not where they theoretically should be.

---

## Target Audience

**Primary:** Community-wide—any club, operator, or layout owner can use ROG

**Specific user groups:**
- Modular railroad clubs (FREE-MO, NTrak) at train shows
- Permanent layout owners running operations sessions
- Individual operators participating in sessions
- Session masters setting up and managing sessions

**Typical session characteristics:**
- 3-12 operators (concurrent)
- 2-4 hour duration
- Mix of experience levels (some new to operations)
- Fluid participation (people come/go mid-session)
- Sometimes multi-day progressive sessions

**Technical context:**
- Train shows: unreliable WiFi, multiple devices
- Home layouts: stable connectivity
- Operator devices: mix of phones, tablets, laptops
- Age range: older demographic (clear UX essential)

---

## Scope Boundaries

### MVP (Phase 5 Implementation)

**In Scope:**
- Layout topology modeling (modules, connections, branches)
- Geographic routing algorithm (branch-aware car assignment)
- Car tracking and state management
- Train manifest generation (train-centric view)
- Switch list generation (location-centric view)
- Multi-operator real-time collaboration
- Offline tolerance (30+ second connection drops)
- Session master role with admin capabilities
- Basic stat tracking (session metrics, operator performance)
- PWA installation and usage

**Out of Scope (MVP):**
- JMRI import/export
- Post-session data export (JSON/CSV)
- Layout/roster sharing (community features)
- Paper backup/PDF export
- Advanced gamification (leaderboards, achievements)
- Integration with physical layout control systems
- Automated train scheduling/dispatching
- Fast clock integration

### Future Enhancements (Post-MVP)

**Potential additions:**
- JMRI compatibility (import car rosters, export sessions)
- Community layout library (share module definitions)
- Post-session analytics and reporting
- Gamification features (operator rankings, session achievements)
- Advanced scheduling (time-based operations, timetables)
- Integration with layout control (DCC, block detection)
- Paper backup for disaster recovery scenarios

---

## Success Criteria

### For ROG Application

The application is successful when:
- Operators can work geographic assignments without traversing entire layout
- Setup time reduced compared to paper systems
- Reality-first tracking handles fluid session participation
- System works reliably with 30+ second connectivity drops
- Session masters can configure modular layouts quickly
- All defined MVP features function as specified

**Critical validation:**
- User validates: "This solves the geographic routing problem my previous implementation failed at"
- Session flows naturally with reality-first tracking
- Operators can join/leave/switch trains fluidly
- Admin can resolve edge cases (missing cars, out-of-game equipment)

### For Portfolio Piece ("The ROG Sessions")

Separate project goal (Phase 6):
- Demonstrates effective human-AI collaboration patterns
- Shows how human guidance shapes AI output
- Includes decision pivots and critical moments
- Creates coherent narrative from collaboration sessions
- Target format: 30-minute video OR comprehensive blog post
- Target audience: Developers/creators interested in AI-assisted workflows

**Portfolio success is independent of ROG success** - even if ROG fails technically, portfolio can document the collaboration process.

---

## Business Model

**Freemium approach:**
- Free tier: Single operator mode (solo sessions)
- Paid tier: Multi-operator sessions (club use)
- Cloud-hosted deployment (avoid "every club becomes sysadmin")

**Revenue goal:**
Recoup hosting and development costs, not profit-driven.

**Development:**
- Open development (public GitHub repository)
- License decision deferred until implementation phase
- Community contributions possible post-launch

---

## Key Architectural Principles

### 1. Geographic Awareness is Core
Without topology understanding, ROG is just another broken operations system. Layout representation and routing algorithm are the differentiating features.

### 2. Reality-First Data Model
Track actual car positions, not theoretical. When operator says "I spotted car #5678 at Industry C," that's the truth—update the system to match reality.

### 3. Fluid Participation
Sessions are messy: people arrive late, leave early, abandon trains, pick up others' work. System must handle this gracefully.

### 4. Offline Tolerance
Train shows have unreliable WiFi. Operators must complete entire trains while offline, sync when reconnected.

### 5. Configuration Variability
Modular layouts never have the same configuration twice. System must handle unknown rosters and changing topology.

### 6. Car Ownership Prevents Conflicts
Cars are "locked" to assigned trains. Most sync conflicts prevented by design, not resolved reactively.

### 7. Admin Authority Fixes Reality
When reality diverges (missing car, wrong position), session master's changes are authoritative.

---

## The Core Challenge

From 26 questions identified in Phase 1, **one is paramount:**

**Q3: Geographic Routing Algorithm**
- How does "build a train for the West Branch" actually work?
- What's the algorithm for branch detection from topology?
- How do we balance shortest path vs operational cost vs complexity?
- How do we handle complex cases (Grain Elevator requires 2 reversals)?

This is THE problem ROG must solve. Platform decisions (PWA, offline sync) are enablers. Data models (topology) are prerequisites. But geographic routing is the reason ROG exists.

**Previous implementation failed here.** We will not.

---

## Related Documents

**Research & Background:**
- [../planning/research/domain-knowledge.md](../planning/research/domain-knowledge.md) - Model railroad operations domain
- [../planning/research/yardless-operations.md](../planning/research/yardless-operations.md) - Short line operations patterns

**Requirements Details:**
- [platform.md](platform.md) - Technical architecture
- [data-models.md](data-models.md) - Layout topology and future models
- [routing-algorithm.md](routing-algorithm.md) - Geographic routing design
- [open-questions.md](open-questions.md) - All 26 questions, status, dependencies

**Decision History:**
- [decisions-log.md](decisions-log.md) - Technical rationale for key decisions

**Progress Tracking:**
- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Question-by-question status
- [../planning/current-work.md](../planning/current-work.md) - Current session focus
