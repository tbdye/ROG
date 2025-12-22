# ROG - Platform & Technical Architecture

**Last Updated:** December 21, 2025
**Status:** Platform architecture complete (Q22-26)

---

## Overview

This document captures platform-level technical decisions that establish ROG's technical foundation: deployment model, device support, offline capabilities, authentication, and authority models.

**Questions Addressed:** Q22-26 from Phase 1 framework (all complete)

---

## Q22: Technical Platform

**Status:** ✓ Complete

### Decision: Progressive Web App (PWA) with Cloud Backend

**Architecture:**
- **Frontend:** Progressive Web App (PWA)
  - Installs from browser, no app store required
  - Service workers for offline capability
  - Mobile-first responsive design

- **Backend:** Cloud-hosted (Azure or cost-effective alternative)
  - RESTful API or GraphQL
  - Real-time collaboration via WebSockets
  - Server-authoritative state management

- **Deployment:** Cloud-based SaaS
  - No self-hosted option for MVP
  - Avoids "every club becomes sysadmin" barrier

### Device Support

**Operators (during session):**
- Mobile-first: phones and tablets
- Touch-optimized interfaces
- Minimal text entry (tap/swipe workflows)

**Session Master (setup/admin):**
- Desktop/laptop preferred (keyboard + mouse)
- Visual module editor needs larger screen
- Admin dashboard with detailed visibility

**Multi-device collaboration:**
- Real-time synchronization across all connected devices
- Operator can use phone while session master monitors on laptop
- Multiple operators each on their own devices

### Connectivity & Offline Tolerance

**Requirements:**
- Tolerates 30+ second connection drops without data loss
- Operators can complete entire train's work while offline
- No queue limit (if 50 cars spotted offline, sync all on reconnect)
- Proactive connectivity indicator (small unobtrusive icon)
  - States: Online, Offline, Syncing
  - Non-blocking, doesn't interrupt workflow

**Offline capabilities:**
- Service workers cache app shell and critical data
- IndexedDB for local state persistence
- Optimistic UI updates (immediate feedback, background sync)
- Queue all changes while offline, replay on reconnect

**Edge case handling:**
- Industry capacity conflicts from offline spotting → session master notified
- Admin state changes while players offline → admin authoritative, conflicts tracked

### Why PWA over Native Apps?

**Rationale:**

User initially explored self-hosted approach but systematically identified friction points:
- WiFi AP setup at every show
- SSL certificate management (Let's Encrypt complexity)
- Single point of failure (host's device)
- "No internet for anyone while playing"

**Cloud-based with PWA chosen because:**
- ✓ No app store submission/approval delays
- ✓ Instant updates (refresh browser)
- ✓ Cross-platform (iOS, Android, desktop)
- ✓ Offline capability via service workers
- ✓ Cloud backend enables multi-device sync
- ✓ Reduces adoption friction ("visit URL, install, done")

**Validation:**
User tested Squoosh PWA on iPhone, confirmed functionality.

**UX requirement identified:**
Clear onboarding instructions essential for older demographic (install process not obvious).

### Alternatives Considered

**Self-hosted (rejected):**
- Pros: No recurring costs, local control
- Cons: WiFi setup burden, SSL complexity, single device failure, adoption barrier

**Native mobile apps (rejected):**
- Pros: Better offline, platform integration
- Cons: App store delays, separate iOS/Android codebases, update friction

**Desktop software (rejected):**
- Pros: Familiar to JMRI users
- Cons: Not mobile-friendly, dated approach, no multi-device

---

## Q23: Integration and Compatibility

**Status:** ✓ Complete

### MVP Scope: Stat Tracking

**In scope for MVP:**
- **Session metrics:**
  - Cars spotted, trains completed, session duration
  - Work completion percentage
  - System health indicators

- **Operator metrics:**
  - Cars handled, moves completed, time active
  - Trains worked, efficiency measures

- **System health tracking:**
  - Sync conflicts count
  - Offline duration per operator
  - Average latency

**Purpose:**
- Enables future gamification (leaderboards, achievements)
- Provides session quality insights
- Debugging and system optimization

### Future Enhancements (NOT MVP)

**JMRI integration (deferred):**
- Import car rosters from JMRI OperationsPro
- Import layout definitions
- Architect to allow future integration, but don't implement for V1

**Post-session export (deferred):**
- JSON/CSV export for records and analysis
- Archive completed sessions
- External data analysis tools

**Layout/roster sharing (deferred):**
- Community layout library
- Share module definitions
- Collaborative layout building

### Paper Export Removed from Scope

**Decision:** NO paper backup/PDF export in MVP

**Rationale:**
- Disaster recovery scenario acceptable without paper
- Offline cached state provides inherent backup during session
- If all devices fail catastrophically, session disruption is acceptable for V1
- Paper backup changes gameplay dynamics (static list vs evolving world)

**User insight:**
> "If the game was established and in play when everything went offline, then everyone should already be in offline cached mode."

Offline cache already provides session continuity. Complete device failure is rare edge case.

### Alternatives Considered

**Include paper export in MVP (rejected):**
- Adds complexity without clear MVP value
- Changes operational dynamics (operators refer to static list instead of system)
- Disaster recovery is acceptable V1 limitation

---

## Q24: Project Scope and Audience

**Status:** ✓ Complete

### Decision: Community-Wide, Open Development, Freemium Model

**Target Audience:**
- Any club, operator, or layout owner can use ROG
- Not limited to specific club or personal use
- Generalized for community adoption

**Development Approach:**
- Open development (public GitHub repository)
- Community can observe progress
- Potential for community contributions post-launch

**Business Model:**
- **Freemium:** Free for single operator, paid for multi-operator sessions
- **Revenue goal:** Recoup hosting/development costs (not profit-driven)
- **License:** Decision deferred until implementation phase

**Deployment Model:**
- Commercial cloud deployment
- Avoid self-hosted to reduce adoption friction
- "No club becomes sysadmin" principle

### Rationale

**Why community-wide scope:**
- Solve problem for entire hobby community, not just one user
- Broader impact potential
- More interesting portfolio piece (building for community vs self)

**Why open development:**
- Transparency builds trust
- Community can validate approach
- Potential for contributions/feedback

**Why freemium model:**
- Free single-operator removes barrier for trial/personal use
- Paid multi-operator sustains hosting costs
- Aligns with "any club can use" vision

### Alternatives Considered

**Personal use only (rejected):**
- Solves user's problem but not community's
- Less compelling portfolio narrative

**Fully free (rejected):**
- Hosting costs for multi-user sessions substantial
- Unsustainable without revenue

**Paid-only (rejected):**
- Barrier to trial and adoption
- Conflicts with accessibility goal

---

## Q25: Physical Artifacts and Backup

**Status:** ✓ Complete

### Decision: Pure Digital for MVP

**MVP approach:**
- No paper dependency
- No hybrid scenarios (some paper, some digital)
- Pure digital operations with offline tolerance

**Disaster recovery:**
- PWA service workers provide offline tolerance
- If devices fail but operators were offline-cached, they can continue
- Complete device failure (all devices simultaneously) is acceptable edge case for V1

**Future consideration:**
- Paper backup for disaster recovery noted but not MVP
- Awareness that paper changes gameplay (static vs evolving world)

### Rationale

**Why pure digital:**
- Offline cached state provides inherent backup during active session
- Complete catastrophic failure (all devices fail) is rare edge case
- Paper backup adds complexity without clear V1 value
- Disaster recovery scenario is acceptable limitation

**User refinement:**
> "I don't think the session master would be able to reconstruct from memory/photos/whatever, but assuming the game was established and in play when everything went offline, then everyone should already be in offline cached mode."

Key insight: Offline mode IS the backup mechanism. If playing when internet fails, cached state continues operation.

### Alternatives Considered

**Hybrid paper/digital (rejected):**
- Adds complexity
- Unclear UX (when to use paper vs digital?)
- Paper becomes crutch instead of disaster recovery

**QR codes on modules (deferred):**
- Interesting for module identification
- Not essential for MVP
- Future enhancement possibility

---

## Q26: User Authentication and Authority

**Status:** ✓ Complete

### Decision: Email Auth, Session Master Role, Car Ownership Model

### Authentication

**User accounts:**
- Email-based authentication
- Simple signup/login flow
- No social login for MVP (add later if needed)

**Ownership model:**
- Club/subscription owns the "session space"
- Subscription owner can invite operators
- Operators join sessions via invite/code

### Session Master Role

**Dual profile model:**
- **Admin profile:** Setup, configuration, god mode (desktop)
- **Operator profile:** Regular player working a train (mobile)
- Can switch between profiles during session
- Retains admin access even when operating

**Admin capabilities:**
- Override car positions (fix reality divergence)
- Reassign cars between trains
- Mark cars "out of game" (owner took home)
- End/freeze session
- View all operators, connectivity status, system health
- Resolve capacity conflicts
- Authority over edge cases

**Session master assignment:**
- Role is assignable, not tied to subscription owner
- Subscription owner can delegate session master for specific session
- Enables trusted club members to run sessions

### Car Ownership Model (Conflict Prevention)

**Core principle:** Cars are "locked" to their assigned train.

**How it works:**
1. Session master assigns cars to trains (initial consist)
2. Operator accepts train assignment
3. Cars in that train's consist are "owned" by that operator
4. Other operators cannot modify those cars
5. When car is spotted/delivered, ownership releases or transfers

**Conflict prevention:**
According to server, if Operator B is offline and spots car #5678, that car remains in Operator B's consist until sync confirms delivery. No other operator can move #5678 because they can't access it.

**Most sync conflicts prevented by design** - no complex resolution needed.

### Admin Authority (Conflict Resolution)

**When conflicts occur:**
- Admin manually changes car state while operator offline
- Admin changes are authoritative (fixing reality)
- Operator's conflicting sync rejected with notification
- System logs conflict, surfaces to session master dashboard

**Examples:**
- Car physically missing → admin marks "out of game"
- Car misdelivered → admin corrects position
- Capacity conflict from offline spotting → admin resolves

**Tracking:**
- All admin overrides logged
- Conflicts surfaced to session master console
- Enables post-session review if needed

### Operator Permissions

**What operators CAN do:**
- Modify cars "owned" by their assigned train
- Accept/decline train assignments
- Spot cars at industries (if train owns car)
- Pull cars from industries (if assigned to pick up)
- Update train progress/status

**What operators CANNOT do:**
- Modify layout configuration
- Reassign cars to different trains
- Override other operators' cars
- Change session state
- View admin dashboard

### Why This Approach

**User's technical insight simplified the model:**

AI initially proposed complex conflict resolution (operational transforms, partition tolerance, CRDT-style merging).

User saw simpler truth:
> "According to the server, if Operator B's connection is 'offline' and they spot car #5678 at industry C, according to the game server, that car is still in operator B's consist until their client informs the server that the car was placed at industry C. There shouldn't be an opportunity for someone else to move #5678 if they're playing the same game."

**Car ownership prevents conflicts inherently.** Admin authority handles edge cases. Simple, effective, aligns with gameplay.

### Alternatives Considered

**"Trust everyone" model (rejected):**
- Works for small groups
- Breaks with accidental conflicts (two operators think they own car)
- No clear resolution mechanism

**Complex CRDT conflict resolution (rejected):**
- Over-engineered for this problem
- Car ownership model simpler and better

**No admin override (rejected):**
- Can't fix reality divergence
- Missing cars, out-of-game equipment have no resolution

---

## Related Decisions

### Time Modeling (Q4 - Not Yet Addressed)
Platform supports both:
- Discrete sessions (session 1, session 2)
- Continuous time with fast clocks (if needed)

Decision deferred until Q4 addressed.

### Multi-Session Continuity (Q9 - Not Yet Addressed)
Platform architecture supports:
- State persistence across sessions
- Multi-day progressive sessions
- Layout reconfiguration (modular)

Details deferred until Q9 addressed.

---

## Implementation Notes

### Technology Stack (To Be Determined in Phase 3)

**Frontend candidates:**
- React + PWA libraries
- Vue + PWA plugin
- Svelte + SvelteKit (PWA support)

**Backend candidates:**
- Node.js + Express/Fastify
- Python + FastAPI
- Go + Gin/Echo

**Database candidates:**
- PostgreSQL (relational for complex queries)
- MongoDB (document for flexibility)
- SQLite (embedded for simplicity)

**Real-time:**
- WebSockets (Socket.io, ws)
- Server-Sent Events (SSE)
- GraphQL subscriptions

**Hosting:**
- Azure (preferred for ecosystem)
- AWS, Google Cloud (alternatives)
- Fly.io, Render (cost-effective options)

**Decision criteria for Phase 3:**
- Team skill set
- Scalability requirements
- Cost efficiency
- Development velocity

---

## Open Questions (Deferred)

**Q10: Role-Specific Workflows**
- What information does each role need to see?
- Can operators see other players' online/offline status?
- What's the session master's minute-by-minute workflow?

**Q12: Session State Tracking**
- How granular is position tracking?
- What happens when reality diverges?
- Is there an "undo" mechanism?

**Q14: Missing or Misdelivered Cars**
- Detailed workflow for marking cars lost/found
- Recovery procedures

**Q15: Cars Going "Out of Game"**
- How to query "show only cars here today"
- Borrowed vs permanent rolling stock tracking

---

## Related Documents

- [overview.md](overview.md) - Problem statement, vision, scope
- [data-models.md](data-models.md) - Layout topology and data structures
- [user-workflows.md](user-workflows.md) - Role-specific operations (Q10)
- [decisions-log.md](decisions-log.md) - Rationale for platform decisions
- [../planning/phase2-progress.md](../planning/phase2-progress.md) - Question tracking
