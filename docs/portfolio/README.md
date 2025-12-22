# "The ROG Sessions" - Portfolio Project

**Project Type:** Portfolio content creation
**Source Material:** ROG (Railroad Operations Game) development collaboration
**Target Deliverable:** 30-minute video OR comprehensive blog post
**Target Audience:** Developers/creators interested in AI-assisted workflows
**Last Updated:** December 21, 2025

---

## What is This?

"The ROG Sessions" is a **separate project** that documents the human-AI collaboration process during ROG development. This is **portfolio content**, not ROG planning.

**ROG project goal:** Build a functional railroad operations game
**Portfolio project goal:** Document HOW we built it together (process, not product)

---

## Portfolio Content Structure

```
portfolio/
├── README.md                        # This file - portfolio project overview
├── collaboration-patterns.md        # Observed patterns across sessions
├── presentation-artifacts.md        # Curated quotes, moments for blog/video
└── sessions/                        # Chronological session narratives
    ├── 2025-12-06-initial-setup.md
    ├── 2025-12-06-repo-structure.md
    ├── 2025-12-07-phase1-domain-research.md
    ├── 2025-12-07-platform-architecture.md
    ├── 2025-12-20-topology-representation.md
    └── 2025-12-21-documentation-reorganization.md
```

---

## Purpose & Focus

### What We're Capturing

**Focus on:**
- How human guidance shapes AI output
- Decision pivots and direction changes
- Critical moments that shaped the project
- Effective prompting and guidance techniques
- Problem-solving collaboration patterns

**NOT about:**
- Technical tutorial on railroad operations
- ROG feature documentation
- Code implementation details
- Screen recordings or raw output

### The Collaboration Story

**Key insight from initial framing:**
> "I want to capture how I guide you and how you respond. Seeing raw content isn't necessarily useful, but the process that humans use to effectively get good work from LLMs like yourself is."

This project documents **collaboration technique**, not technical output.

---

## Content Types

### Collaboration Patterns
**File:** [collaboration-patterns.md](collaboration-patterns.md)

Synthesized patterns observed across sessions:
- Structured feedback
- Meta-awareness
- Systematic elimination
- Empirical testing
- Iterative refinement
- Adaptive interaction styles

### Presentation Artifacts
**File:** [presentation-artifacts.md](presentation-artifacts.md)

Curated quotes and moments for final deliverable:
- Direct instructions showing guidance style
- Breakthrough moments
- Problem-solving examples
- Technique demonstrations

### Session Narratives
**Directory:** [sessions/](sessions/)

Chronological process documentation:
- What we accomplished
- How we collaborated
- Key decisions and pivots
- Patterns observed in this session
- Contrasts with other sessions

---

## Portfolio Content is Duplicative

**Important principle:** Portfolio content duplicates ROG planning where necessary, but the two are independent.

**ROG content should NEVER require portfolio content** to understand technical decisions. All ROG context lives in `../PRD/` and `../planning/`.

**Portfolio content can reference ROG** to provide project context, but focuses on process not product.

---

## Phase 6: Portfolio Synthesis

**When:** After ROG implementation (Phase 5) complete

**Goal:** Transform session narratives and patterns into cohesive portfolio deliverable

**Format Decision:** 30-minute video OR comprehensive blog post (TBD in Phase 6)

**Synthesis Tasks:**
1. Review all session narratives
2. Extract most compelling collaboration examples
3. Build narrative arc showing human-AI collaboration evolution
4. Create final deliverable demonstrating effective AI-assisted workflows

---

## Success Criteria

Portfolio piece is successful when:
- Demonstrates effective human-AI collaboration patterns
- Shows how human guidance shapes AI output
- Includes decision pivots and critical moments
- Creates coherent narrative from collaboration sessions
- Provides value to audience (developers/creators)
- Demonstrates techniques viewers can apply to their own AI work

**Portfolio success is independent of ROG success** - even if ROG fails technically, portfolio can document the collaboration process.

---

## Never Needed for ROG Planning

**Critical principle:** You should never need to read portfolio content to understand ROG.

All ROG context lives in:
- `../PRD/` - Requirements, decisions, models
- `../planning/` - Research, specifications, progress tracking
- `../../CLAUDE.md` - Operational guidance
- `../../CONTRIBUTING.md` - Git workflow

Portfolio content is **process documentation for a separate project.**

---

## Related Documents

**ROG Planning (Independent):**
- [../PRD/README.md](../PRD/README.md) - ROG requirements index
- [../planning/current-work.md](../planning/current-work.md) - Current session state

**Portfolio Content:**
- [collaboration-patterns.md](collaboration-patterns.md) - Synthesized patterns
- [presentation-artifacts.md](presentation-artifacts.md) - Curated moments
- [sessions/](sessions/) - Chronological narratives
