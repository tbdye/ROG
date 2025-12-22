# Platform Architecture Decisions

**Category:** Platform & Deployment
**Last Updated:** December 21, 2025

---

## PWA vs Native Apps vs Desktop Software
#platform #pwa #deployment #adoption #offline-sync #final #mvp-critical

**Decision:** Progressive Web App (PWA) with cloud backend
**Date:** 2025-12-07
**Status:** Final

**Context:**
Need deployment model that works on phones/tablets for operators and desktop for session master, with offline capability for unreliable WiFi at train shows.

**Alternatives Considered:**

1. **Self-hosted local server:**
   - Pros: No recurring costs, local control
   - Cons: WiFi AP setup at every show, SSL cert complexity, single device failure point
   - User explored this systematically, identified too much operational friction

2. **Native mobile apps:**
   - Pros: Best offline capability, platform integration
   - Cons: App store delays, separate iOS/Android development, update friction

3. **Desktop software (like JMRI):**
   - Pros: Familiar to existing users
   - Cons: Not mobile-friendly, dated approach, no multi-device support

**Rationale:**
- PWA provides offline capability via service workers
- No app store friction (install from browser)
- Cross-platform (iOS, Android, desktop) from single codebase
- Cloud backend enables real-time multi-device sync
- Reduces adoption barrier ("visit URL, install, done")

**Trade-offs:**
- Gained: Deployment simplicity, cross-platform, instant updates
- Lost: Slightly worse offline than native (but acceptable)

**User validation:** Tested Squoosh PWA on iPhone, confirmed functionality. Identified UX requirement: clear onboarding for older demographic.

**Related PRD Section:** [../platform.md](../platform.md#q22-technical-platform)

---

## Cloud-Based vs Self-Hosted Deployment
#deployment #cloud #adoption #usability #train-show #final #mvp-critical

**Decision:** Cloud-based SaaS deployment (no self-hosted option for MVP)
**Date:** 2025-12-07
**Status:** Final

**Context:**
Choosing between cloud hosting (recurring costs, internet dependency) vs self-hosted (setup complexity, local control).

**User's systematic elimination:**
User explored self-hosted by thinking aloud about friction points:
- WiFi AP setup at every show
- SSL certificate management (Let's Encrypt complexity)
- Single point of failure (host's device)
- "No internet for anyone while playing"

Concluded:
> "I feel like I'm talking myself out of this approach."

**Rationale:**
- "No club becomes sysadmin" principle
- Reduces adoption friction
- Reliable multi-device sync
- Centralized updates and maintenance

**Trade-offs:**
- Gained: Adoption simplicity, reliability, multi-device sync
- Lost: Recurring hosting costs, internet dependency (mitigated by offline tolerance)

**Related PRD Section:** [../platform.md](../platform.md#q22-technical-platform)

---

## Car Ownership Model for Conflict Prevention
#conflict-resolution #offline-sync #car-identity #ownership-model #real-time #final #mvp-critical

**Decision:** Cars "locked" to assigned train, preventing multi-train assignment
**Date:** 2025-12-07
**Status:** Final

**Context:**
Need to handle offline sync conflicts when multiple operators working simultaneously with unreliable connectivity.

**Alternatives Considered:**

1. **Complex CRDT-style conflict resolution:**
   - Pros: Theoretically handles arbitrary conflicts
   - Cons: Over-engineered, complex to implement, unclear UX for resolution

2. **"Trust everyone" model:**
   - Pros: Simple, works for small groups
   - Cons: Breaks with accidental conflicts, no resolution mechanism

3. **Always-online requirement (reject offline changes):**
   - Pros: Avoids conflicts entirely
   - Cons: Breaks at train shows (unreliable WiFi requirement)

**Rationale:**
User's technical insight simplified the problem:
> "According to the server, if Operator B's connection is 'offline' and they spot car #5678 at industry C, according to the game server, that car is still in operator B's consist until their client informs the server that the car was placed at industry C. There shouldn't be an opportunity for someone else to move #5678 if they're playing the same game."

Cars locked to trains by design. Most conflicts prevented inherently, not resolved reactively.

**Trade-offs:**
- Gained: Simple, predictable, matches gameplay model
- Lost: Some flexibility (but not needed in practice)

**Edge case handling:** Admin can override for reality divergence (missing cars, out-of-game equipment).

**Related PRD Section:** [../platform.md](../platform.md#car-ownership-model)

---

## No Paper Backup in MVP
#error-recovery #offline-sync #scope-reduction #final #post-mvp

**Decision:** Pure digital for MVP, no paper/PDF export
**Date:** 2025-12-07
**Status:** Final

**Context:**
Considering disaster recovery if all devices fail during session.

**Alternatives Considered:**

1. **Paper backup export:**
   - Pros: Ultimate disaster recovery
   - Cons: Changes gameplay dynamics (static list vs evolving world), adds complexity

2. **Hybrid paper/digital:**
   - Pros: Flexibility
   - Cons: Unclear when to use which, paper becomes crutch

**Rationale:**
User refined thinking on disaster recovery:
> "I don't think the session master would be able to reconstruct from memory/photos/whatever, but assuming the game was established and in play when everything went offline, then everyone should already be in offline cached mode."

Offline cache IS the backup. If devices fail mid-session, operators already have cached state. Complete catastrophic failure (all devices simultaneously) is acceptable V1 edge case.

**Trade-offs:**
- Gained: Simpler MVP scope, consistent digital experience
- Lost: Disaster recovery for edge case (acceptable)

**Future consideration:** Can add paper backup post-MVP if needed.

**Related PRD Section:** [../platform.md](../platform.md#q25-physical-artifacts-and-backup)

---

## Freemium Business Model
#business-model #adoption #cloud #final #mvp-critical

**Decision:** Free single operator, paid multi-operator sessions
**Date:** 2025-12-07
**Status:** Final

**Context:**
Need to recoup hosting costs while maximizing adoption.

**Alternatives Considered:**

1. **Fully free:**
   - Pros: Maximum adoption
   - Cons: Unsustainable hosting costs for multi-user sessions

2. **Paid-only:**
   - Pros: Immediate revenue
   - Cons: Adoption barrier, can't trial before buying

**Rationale:**
- Free tier removes barrier for trial/personal use
- Paid tier sustains hosting costs
- Revenue goal: Recoup costs, not profit-driven

**Trade-offs:**
- Gained: Sustainable model, low barrier to entry
- Lost: Some potential revenue from free users (acceptable)

**Related PRD Section:** [../platform.md](../platform.md#q24-project-scope-and-audience)
