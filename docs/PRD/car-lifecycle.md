# ROG - Car Lifecycle & State Management

**Last Updated:** December 21, 2025
**Status:** Q2 - Not Started

---

## Overview

This document will define car state lifecycle: all possible states, state transitions, and authority for state changes.

**Question Addressed:** Q2 - Car State Lifecycle

---

## Questions to Answer

From Q2 framework:

1. What are ALL the states a car can be in?
   - staged, assigned, in-transit, spotted, loading, loaded, unloading, empty, available, out-of-game?
2. What triggers state transitions?
3. Who can change car states? (system auto, session master, operators?)

---

## Preliminary States

Potential states (to be validated in Q2):

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

---

## Related Decisions

**Car ownership model (Q26):**
- Cars locked to assigned train
- Prevents most state conflicts
- See [platform.md](platform.md#car-ownership-model)

**Reality-first principle:**
- Actual state trumps theoretical
- Operator actions authoritative for their assigned cars
- Admin can override to fix reality divergence

---

## Placeholder Content

**This section pending Q2 work.**

Content will include:
- Complete state diagram
- Transition triggers and rules
- Authority matrix (who can change what)
- Edge cases and error handling
- Integration with car ownership model

---

## Related Documents

- [platform.md](platform.md) - Car ownership and authority
- [data-models.md](data-models.md) - Car data model (Q6)
- [open-questions.md](open-questions.md) - Q2 details
