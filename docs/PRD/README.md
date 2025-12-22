# ROG Product Requirements Document - Index

**Last Updated:** December 21, 2025
**Project Phase:** Phase 2 - PRD Development (In Progress)

---

## What is This?

This directory contains the **source of truth** for the Railroad Operations Game (ROG) product requirements. All technical decisions, data models, algorithms, and rationale are documented here.

**To understand ROG, read these documents.** Session summaries, portfolio content, and process notes are optional supporting material.

---

## Quick Navigation

### Problem & Vision
- **[overview.md](overview.md)** - Problem statement (geographic routing), vision, target audience, MVP scope
  - Related research: [planning/research/domain-knowledge.md](../planning/research/domain-knowledge.md)
  - Related research: [planning/research/yardless-operations.md](../planning/research/yardless-operations.md)

### Platform & Technical Architecture
- **[platform.md](platform.md)** - PWA architecture, cloud backend, offline sync, auth model, session master role
  - Status: Questions 22-26 complete
  - Key decisions: Car ownership model, freemium business model, offline tolerance

### Data Models
- **[data-models.md](data-models.md)** - Layout topology model (Q1 complete), future models (cars, industries, trains)
  - Detailed spec: [planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md)
  - Status: Topology model validated, others pending

### Core Algorithms
- **[routing-algorithm.md](routing-algorithm.md)** - Geographic routing, branch detection, pathfinding (Q3 - in progress)
  - Prerequisites: Requires data-models.md topology understanding
  - Status: Next priority

### Operational Workflows
- **[user-workflows.md](user-workflows.md)** - Role-specific operations: session master, operator, yardmaster, dispatcher (Q10)
  - Status: Not started

- **[car-lifecycle.md](car-lifecycle.md)** - Car states, state transitions, ownership rules (Q2)
  - Status: Not started

### Open Questions
- **[open-questions.md](open-questions.md)** - All 26 questions from Phase 1, status, dependencies, progress
  - See also: [planning/phase2-progress.md](../planning/phase2-progress.md) for detailed tracking

### Decision Rationale
- **[decisions-index.md](decisions-index.md)** - Quick reference to find specific technical decisions
  - **[decisions/platform.md](decisions/platform.md)** - Platform architecture decisions
  - **[decisions/data-models.md](decisions/data-models.md)** - Data model decisions
  - **[decisions/process.md](decisions/process.md)** - Process and philosophy decisions
  - WHY we chose approaches (technical reasoning, alternatives considered)
  - Separate from WHAT we decided (captured in respective PRD sections)

---

## How to Use This PRD

### For Session Continuity
1. Read [planning/current-work.md](../planning/current-work.md) - What are we working on right now?
2. Load relevant PRD sections based on current work
3. Reference detailed specs as needed

### For Understanding ROG
1. Start with **overview.md** - Problem, vision, scope
2. Read **platform.md** - Architecture foundation
3. Read **data-models.md** - Core data structures
4. Read topic-specific sections as interested

### For Implementation (Phase 5)
1. PRD becomes frozen reference (requirements)
2. Create `docs/implementation/` with component-level specs
3. Load only PRD sections relevant to current component
4. See "Future Evolution" section below

---

## Document Organization Principles

### Each PRD Section Should:
- Be **self-contained** with clear cross-references
- Answer specific questions from the 26-question framework
- Document **what** we decided and **why** (rationale)
- Be 5-15k tokens (split if larger)
- Have clear "Status" and "Dependencies" sections

### Cross-References
Use relative links to connect related content:
```markdown
See [data-models.md](data-models.md) for topology representation
See [platform.md](platform.md#offline-sync) for offline sync constraints
See [../planning/specifications/topology-data-model.md](../planning/specifications/topology-data-model.md) for detailed spec
```

---

## Status Summary

**Completed Questions:** 6/26 (23%)

| Category | Complete | Total |
|----------|----------|-------|
| Critical (Must Address in PRD) | 1 | 5 |
| Important (Affects Design) | 0 | 5 |
| Refinement (Can Defer) | 0 | 11 |
| Platform and Deployment | 5 | 5 |

**Next Priority:** Q3 - Geographic Routing Algorithm

---

## Future Evolution

### Phase 3-4 (Planning)
PRD expands as we answer questions:
- Add sections for time-modeling.md, staging-mechanics.md, etc.
- Detailed specs grow in planning/specifications/
- Open questions resolved into PRD sections

### Phase 5 (Implementation)
```
docs/PRD/              # Frozen requirements (reference only)
docs/implementation/   # NEW - Component specs, API contracts, DB schema
├── architecture/
│   ├── overview.md
│   ├── backend-api.md
│   └── database-schema.md
├── components/
│   ├── topology-editor.md
│   ├── routing-engine.md
│   └── sync-manager.md
└── current-component.md  # What we're building RIGHT NOW
```

During implementation:
- Load PRD sections for WHAT (requirements)
- Load implementation docs for HOW (build specs)
- Component-focused context loading

---

## Context Loading Guide

See [../../CLAUDE.md](../../CLAUDE.md) for full context management protocol.

**Tier 1 (Always Load):**
- This file (PRD/README.md)
- overview.md
- planning/current-work.md

**Tier 2 (Active Work):**
- PRD sections relevant to current question
- Related specifications
- Phase progress tracker

**Tier 3 (Reference/On-Demand):**
- Research reports
- Completed PRD sections not relevant to current work
- Portfolio content (never needed for ROG planning)

---

## Contributing to the PRD

When adding or updating PRD content:

1. **Update this index** if creating new sections
2. **Cross-reference** related content
3. **Document rationale** in decisions-log.md if choosing between alternatives
4. **Keep sections focused** - split if exceeding ~15k tokens
5. **Update status** in open-questions.md when answering questions

---

## Questions?

- For workflow/process: See [../../CONTRIBUTING.md](../../CONTRIBUTING.md)
- For Claude guidance: See [../../CLAUDE.md](../../CLAUDE.md)
- For project framework: See [../ROG-PROJECT-GUIDELINES.md](../ROG-PROJECT-GUIDELINES.md)
