# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context

**Railroad Operations Game (ROG)** is a persistent backend application with web-based configuration interface, currently in planning phase. This project serves dual purposes:
1. Build a functional Railroad Operations Game application
2. Document AI-assisted development workflow for portfolio/content creation ("The ROG Sessions")

**Current Phase:** Phase 2 - PRD Development (implementation has not begun)

## Project Workflow Phases

The project follows a structured 6-phase approach:

1. **Phase 1: Ideation** [x] Complete - Requirements exploration, scope definition
2. **Phase 2: PRD Development** (Current) - Product Requirements Document creation
3. **Phase 3: Implementation Planning** - Tech stack, architecture, deployment strategy
4. **Phase 4: Detailed Task Breakdown** - Prioritized task list with dependencies
5. **Phase 5: Implementation** - Active development (Claude Code CLI primary tool)
6. **Phase 6: Portfolio Content Synthesis** - Create blog/video from collaboration narrative

## Documentation Structure

```
ROG/
├── CLAUDE.md                           # This file - Claude Code guidance
├── CONTRIBUTING.md                     # Git workflow and PR standards
├── README.md                           # Project overview
├── ROG-PROJECT-GUIDELINES.md           # Project framework (lightweight)
│
└── docs/
    ├── PRD/                            # ★ SOURCE OF TRUTH FOR ROG
    │   ├── README.md                   # PRD index and navigation
    │   ├── overview.md                 # Problem statement, vision, scope
    │   ├── platform.md                 # Platform architecture (Q22-26)
    │   ├── data-models.md              # Layout topology, future models
    │   ├── routing-algorithm.md        # Geographic routing (Q3)
    │   ├── user-workflows.md           # Role-specific workflows (Q10)
    │   ├── car-lifecycle.md            # Car states and transitions (Q2)
    │   ├── open-questions.md           # All 26 questions, status
    │   └── decisions-log.md            # Technical decision rationale
    │
    ├── planning/                       # ROG PLANNING MATERIALS
    │   ├── current-work.md             # ★ SESSION BOOTSTRAP
    │   ├── phase2-progress.md          # Question-by-question tracking
    │   ├── research/                   # Domain knowledge
    │   │   ├── domain-knowledge.md
    │   │   └── yardless-operations.md
    │   └── specifications/             # Detailed technical specs
    │       └── topology-data-model.md
    │
    └── portfolio/                      # PORTFOLIO PROJECT ("The ROG Sessions")
        ├── README.md                   # Portfolio project overview
        ├── collaboration-patterns.md   # Synthesized patterns
        ├── presentation-artifacts.md   # Curated quotes, moments
        └── sessions/                   # Chronological process narratives
            ├── 2024-12-06-initial-setup.md
            ├── 2024-12-07-phase1-domain-research.md
            ├── 2024-12-07-platform-architecture.md
            └── 2025-12-20-topology-representation.md
```

## Context Management Strategy

ROG uses a **three-tier context loading approach** optimized for LLM context windows.

### Tier 1: CORE (Always Load - ~15-20k tokens)

**Load at session start:**
- `CLAUDE.md` (this file - operational guidance)
- `CONTRIBUTING.md` (git workflow)
- `docs/planning/current-work.md` (**session bootstrap**)
- `docs/PRD/README.md` (index - what's where)
- `docs/PRD/overview.md` (problem statement, vision, scope)

**Purpose:** Essential context for any ROG work.

### Tier 2: ACTIVE (Load for Current Work - ~30-50k tokens)

**Load based on docs/planning/current-work.md:**
- `docs/planning/phase2-progress.md` (question tracking)
- Relevant PRD sections for current question
- Related specifications if designing/validating

**Example: Working on Q3 (Geographic Routing)?**
- `docs/PRD/data-models.md` (need topology model)
- `docs/PRD/platform.md` (need offline sync constraints)
- `docs/planning/specifications/topology-data-model.md` (detailed spec)

**Purpose:** Focused context for active work without overload.

### Tier 3: REFERENCE (Pull On-Demand - load as needed)

**Load only if specific topic arises:**
- `docs/planning/research/` (domain knowledge, yardless operations)
- Detailed specifications (when designing/implementing)
- Completed PRD sections not relevant to current work
- Portfolio content (sessions, patterns) - **NEVER needed for ROG planning**

**Purpose:** Comprehensive knowledge available without context bloat.

### Context Loading Query

Ask: **"What context do I need?"** or **"What should I load for current work?"**

Claude Code CLI will respond based on `docs/planning/current-work.md` with tiered recommendations.

## Primary References

**To understand ROG:**
- Start with: `docs/PRD/README.md` (index)
- Problem statement: `docs/PRD/overview.md`
- Platform decisions: `docs/PRD/platform.md`
- Current work: `docs/planning/current-work.md`

**Portfolio content is INDEPENDENT:**
- `docs/portfolio/` contains process documentation for "The ROG Sessions"
- Portfolio content is duplicative - never needed to understand ROG
- All ROG context lives in `docs/PRD/` and `docs/planning/`

**For git workflow:**
- `CONTRIBUTING.md` - **Read this first** for branching, commits, PRs

**For project framework:**
- `ROG-PROJECT-GUIDELINES.md` - Workflow phases, operational protocols

## Development Commands

**Note:** Implementation has not yet begun. Tech stack decisions will be made in Phase 3 based on requirements defined in Phases 1-2.

Once implementation begins, standard commands will be documented here for:
- Build/compilation
- Running tests (unit, integration)
- Linting and formatting
- Local development server
- Deployment

## Architecture Notes

**Status:** Platform architecture complete (Q22-26), implementation stack TBD in Phase 3.

**Platform decisions:**
- Progressive Web App (PWA) with cloud backend
- Offline-tolerant (30+ second connection drops)
- Real-time multi-device collaboration
- Car ownership model prevents conflicts
- Session master role with admin authority

See `docs/PRD/platform.md` for complete details.

## Git Workflow (Critical)

**Branch Strategy:**
- **Never commit directly to main**
- All work happens on topic branches: `user/tbdye/<NameOfBranch>`
- Example: `user/tbdye/add-database-schema`

**Before Committing or Creating PRs:**
- **Always ask for permission first**, regardless of what other permissions state
- Ensure all documentation is updated before PR merge
- PR merge finalizes session knowledge - nothing should exist only in conversation

**Commit Messages:**
- Follow best practices (see `CONTRIBUTING.md`)
- Use imperative mood, clear and descriptive
- Explain what and why, not how

**Pull Request Requirements:**
Every PR must include:
- Executive summary (2-4 sentences)
- Notable changes (bulleted)
- Why changes were made
- How (for non-trivial changes)
- Documentation updates list

See `CONTRIBUTING.md` for complete details.

## Session Continuity

### Starting a Session

**User says:** "Start ROG session" or "Resume ROG work"

**Claude's session start checklist (complete in order):**

**Step 1: Read session bootstrap**
- Read `docs/planning/current-work.md`
- Extract current focus (which question/topic we're working on)

**Step 2: State current focus with summary**
- Output current focus clearly
- Provide 2-3 sentence summary of what this work entails
- Example:
  ```
  Current focus: Q3 - Geographic Routing Algorithm

  Summary: Design algorithm that uses topology model to enable branch-based
  routing, solving the geographic routing problem that previous implementation
  failed at.
  ```

**Step 3: Load Tier 1 (CORE) - Always load these**
- CLAUDE.md
- CONTRIBUTING.md (if git operations expected)
- docs/planning/current-work.md
- docs/PRD/README.md
- docs/PRD/overview.md

**Step 4: Recommend Tier 2 (ACTIVE) - Context-specific files**
Based on current focus, recommend relevant files:
- PRD sections for current topic
- Decision rationale for current topic (during planning phases)
- Related specifications
- Progress tracker

**Phase-specific loading:**
- **During Planning (Phase 2-4):** Load BOTH PRD section AND decision rationale
  - Example: PRD/data-models.md + PRD/decisions/data-models.md
- **During Implementation (Phase 5):** Load PRD section + implementation specs
  - Decision rationale only if questioning approach

**Step 5: Directory scan (Tier 3 awareness) - Smart file discovery**
Based on current work, intelligently scan directories for potentially related files:

**Pattern-based suggestions:**
- If current work mentions "car" → suggest: car-lifecycle.md, decisions about car ownership
- If current work mentions "operator" or "workflow" → suggest: user-workflows.md
- If current work mentions topology/layout → suggest: data-models.md, topology-data-model.md
- If current work mentions platform/sync/offline → suggest: platform.md, decisions/platform.md

**Example output for Q3 (Geographic Routing):**
```
Tier 3 scan - Related files available:
- docs/PRD/data-models.md (topology representation - dependency)
- docs/PRD/platform.md (offline sync constraints)
- docs/PRD/decisions/data-models.md (turnout directionality rationale)
- docs/planning/specifications/topology-data-model.md (detailed topology spec)
```

**General scan (all sessions):**
- List all files in docs/PRD/, docs/PRD/decisions/, docs/planning/specifications/
- Highlight which ones seem most related based on filename keywords

**Step 6: Confirm context loading**
- Ask: "Should I load Tier 2 recommendations now? (yes/no/customize)"
- Wait for user confirmation before proceeding

**Purpose of summary:** Forcing function to think about scope, reminds user what's relevant, helps identify missing context.

### Ending a Session

**Two types:** Checkpoint (pause) or Complete (finalize for PR)

#### Common Steps (Both Types)

**1. Update technical state:**
- `docs/planning/current-work.md`:
  - What we just completed
  - What we're working on now
  - What's next when we resume
  - Any blockers
- Update relevant PRD sections with decisions made
- Update `docs/planning/phase2-progress.md` if questions answered
- Update `docs/PRD/open-questions.md` status if applicable

**2. Portfolio content preservation (CRITICAL):**

**Stop and think:** Did meaningful collaboration occur worth preserving?

**Check against criteria:**
- Novel collaboration pattern observed?
- Significant pivot in approach or direction?
- User guidance shaped AI output meaningfully?
- Breakthrough moment or aha insight?
- Productive mistake/correction revealing process?
- Decision with interesting rationale?

**If YES to any:**
- Add entry to `docs/portfolio/collaboration-patterns.md` (if new pattern)
- Add quote/moment to `docs/portfolio/presentation-artifacts.md` (if portfolio-worthy)
- Note in current-work.md what should be captured in session summary

**Skip if:**
- Routine file operations only
- Mechanical updates only
- Standard git workflow only
- Simple clarifications only

**3. Verify all changes written to disk:**
```bash
git status  # Verify all work in files, nothing conversation-only
```

#### Checkpoint (Pause and Resume Later)

**User says:** "Checkpoint session" or "Pause and save state"

**Additional steps:**
- Confirm all changes saved
- Note resume point in current-work.md
- Can clear context and resume anytime

#### Complete (Finalize and Create PR)

**User says:** "Complete session and prepare for PR" or "Wrap up for merge"

**Additional steps:**
- All checkpoint steps above
- Add decision rationale to `docs/PRD/decisions/` (categorized)
- Create session summary in `docs/portfolio/sessions/YYYY-MM-DD-descriptor.md`
- **PR Pre-Flight Checklist** (see below)
- Create branch (if not exists)
- Ask user for permission to create PR
- After PR merge: Session fully finalized, no additional work

---

## PR Pre-Flight Checklist

**Before Creating PR - STOP AND THINK**

This checklist MUST be completed before creating any PR. Skipping steps risks losing context.

**Working Memory Check:**
- [ ] All work written to disk (nothing conversation-only)
- [ ] All decisions documented in PRD sections
- [ ] All rationale captured in decision logs
- [ ] Portfolio content preserved (if meaningful interactions occurred)

**File Updates:**
- [ ] `docs/planning/current-work.md` updated with session state
- [ ] Relevant PRD sections updated with decisions
- [ ] `docs/PRD/decisions/` updated with rationale (categorized)
- [ ] `docs/PRD/open-questions.md` status updated if questions answered
- [ ] `docs/planning/phase2-progress.md` updated if questions completed

**Portfolio Preservation:**
- [ ] Checked if meaningful collaboration occurred (see criteria above)
- [ ] Added to `collaboration-patterns.md` if new pattern observed
- [ ] Added to `presentation-artifacts.md` if portfolio-worthy moment
- [ ] Session summary created in `docs/portfolio/sessions/` (for Complete only)

**Verification:**
- [ ] Run `git status` - all changes visible
- [ ] Review `git diff` - changes match intentions
- [ ] No "TODO" or "FIXME" comments left unresolved
- [ ] Cross-references updated if file structure changed

**Only after ALL checks pass:**
- Create PR with proper description
- After PR merge, session is finalized (no additional work)

**Why this matters:** PR captures snapshot at commit time. Anything in conversation-only is permanently lost.

---

## Decision Documentation Workflow

**CRITICAL:** All decisions MUST be tagged and documented properly. This is CORE WORKFLOW, not optional.

### When Making a Decision

**Immediately upon reaching a decision:**

1. **Document in appropriate PRD section**
   - Update relevant PRD file (platform.md, data-models.md, etc.) with WHAT was decided
   - Include clear rationale for external readers

2. **Add decision entry to `docs/PRD/decisions/` category file**
   - Choose appropriate category: platform, data-models, process
   - Use standard decision template (see below)
   - **MUST include tags** (minimum 3: technical area, scope, status)
   - **MUST include date and status** fields

3. **Update `docs/PRD/decisions-index.md`**
   - Add decision entry with date, status, and tags
   - If introducing new tag, add to Tag Taxonomy section

### Decision Entry Template

```markdown
## Decision Name
#tag1 #tag2 #tag3 #technical-area #status #scope

**Decision:** Brief statement of what was decided
**Date:** YYYY-MM-DD
**Status:** Final | Tentative | Needs Validation

**Context:**
Why this decision was needed, what problem it solves

**Alternatives Considered:**
1. **Option A:**
   - Pros: ...
   - Cons: ...
2. **Option B:**
   - Pros: ...
   - Cons: ...

**Rationale:**
Why we chose this approach over alternatives

**Trade-offs:**
- Gained: ...
- Lost: ...

**Related PRD Section:** [link to relevant PRD file]
```

### Tag Selection Guide

**Every decision MUST have at least:**
- **1-2 technical area tags** (what aspect of system): `#topology`, `#offline-sync`, `#routing`, etc.
- **1 status tag**: `#final`, `#tentative`, or `#needs-validation`
- **1 scope tag**: `#mvp-critical`, `#post-mvp`, or `#scope-reduction`
- **0-3 context tags** (if relevant): `#train-show`, `#modular-layout`, `#permanent-layout`, etc.

**Common tag combinations:**
- Platform decisions: `#platform #deployment #adoption #final #mvp-critical`
- Data model decisions: `#data-model #topology #routing #final #mvp-critical`
- UX decisions: `#usability #adoption #editor #final #mvp-critical`
- Deferred features: `#scope-reduction #post-mvp #final`
- Unconfirmed decisions: `#needs-validation #tentative #mvp-critical`

**See `docs/PRD/decisions-index.md` for complete tag taxonomy.**

### Searching for Decisions

**By tag (recommended for multi-category search):**
```bash
# Find all offline-sync decisions
grep -r "#offline-sync" docs/PRD/decisions/

# Find all MVP-critical decisions
grep -r "#mvp-critical" docs/PRD/decisions/

# Find decisions about topology AND routing
grep -r "#topology" docs/PRD/decisions/ | grep "#routing"

# Find tentative decisions needing revisit
grep -r "#tentative" docs/PRD/decisions/
```

**By name (when you know specific decision):**
```bash
# Find specific decision
grep -n "Car Ownership Model" docs/PRD/decisions/platform.md
```

**By category (browse all platform, data-model, or process decisions):**
- Read `docs/PRD/decisions/platform.md`
- Read `docs/PRD/decisions/data-models.md`
- Read `docs/PRD/decisions/process.md`

### Maintaining Tags

**When updating a decision:**
1. Update status tag if certainty changes: `#tentative` → `#final`
2. Update scope tags if MVP boundary changes: `#post-mvp` → `#mvp-critical`
3. Add new technical tags if decision scope expands
4. Update decisions-index.md with new tags/status
5. Update Date field if decision substantially changes

**When introducing a new tag:**
1. Add to `docs/PRD/decisions-index.md` Tag Taxonomy section
2. Include clear description of when to use it
3. Consider if existing decisions should use this tag (retrospective tagging)

---

## File Organization Guidelines

### PRD Documents (ROG Source of Truth)

**Location:** `docs/PRD/`

**Guidelines:**
- Each section 5-15k tokens (split if larger)
- Self-contained with clear cross-references
- Document WHAT we decided
- Technical rationale in `decisions-log.md` (WHY)

**When to update:**
- Question answered → update relevant PRD section
- Decision made → update `decisions-log.md` with rationale
- Status changed → update `open-questions.md`

### Planning Materials

**Location:** `docs/planning/`

**Key files:**
- `current-work.md` - Always update at session end
- `phase2-progress.md` - Update when questions answered
- `research/` - Domain knowledge (reference only)
- `specifications/` - Detailed technical specs

### Portfolio Content

**Location:** `docs/portfolio/`

**Purpose:** Process documentation for "The ROG Sessions" portfolio project

**Key files:**
- `collaboration-patterns.md` - Synthesized patterns
- `presentation-artifacts.md` - Curated quotes
- `sessions/` - Chronological narratives

**Important:** Portfolio content is duplicative. Never needed for ROG planning.

## File Naming Conventions

**Session summaries:**
- Format: `YYYY-MM-DD-descriptor.md`
- Location: `docs/portfolio/sessions/`
- Date-first for chronological sorting
- Descriptive suffix for multi-session days

**PRD sections:**
- Descriptive names: `routing-algorithm.md`, `user-workflows.md`
- Location: `docs/PRD/`

**Specifications:**
- Descriptive names: `topology-data-model.md`
- Location: `docs/planning/specifications/`

**Before committing new documents:**
- Verify naming matches conventions
- Check correct directory placement
- Update indexes if creating new sections

## File Encoding Standards

**Unicode Encoding:**

To prevent encoding issues in git diffs and cross-platform compatibility, always use ASCII alternatives for status indicators:

**Status Indicators:**
- Use `[x]` instead of ✅ (checkmark)
- Use `[ ]` instead of ⬜ (empty box)
- Use `[OK]` instead of ✅ for healthy status
- Use `[!]` / `[!!]` / `[CRITICAL]` instead of ⚠️ / 🛑

**Arrows:**
- Use `->` instead of →
- Use `<-` instead of ←
- Use `<->` instead of ↔

**Rationale:** Unicode emoji can display inconsistently in git diffs and some text editors. ASCII alternatives render consistently everywhere.


## Current Status

- **Phase 0: Planning Setup** [x] Complete
- **Phase 1: Ideation** [x] Complete
- **Phase 2: PRD Development** - In progress (6/26 questions answered)
  - [x] Q1: Layout Topology Representation
  - [x] Q22-26: Platform Architecture
  - **Next:** Q3 - Geographic Routing Algorithm
- No technical stack selected yet (Phase 3)
- No implementation files exist (Phase 5)

## Portfolio Context

This project captures human-AI collaboration patterns for "The ROG Sessions" portfolio piece (blog post or video).

**Portfolio content location:** `docs/portfolio/`

**Portfolio is independent:** You should never need to read portfolio content to understand ROG planning. All ROG context lives in `docs/PRD/` and `docs/planning/`.

---

When implementation begins (Phase 5), this file will be updated with concrete build commands, architecture details, and development workflows.
