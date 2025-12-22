# ROG Project Guidelines

**Last Updated:** December 21, 2025
**Purpose:** Project framework, workflow phases, and operational protocols

---

## Quick Links

**For ROG Requirements:**
- [PRD/README.md](PRD/README.md) - Product Requirements Document index
- [PRD/overview.md](PRD/overview.md) - Problem statement, vision, scope
- [PRD/open-questions.md](PRD/open-questions.md) - All 26 questions and status

**For Current Work:**
- [planning/current-work.md](planning/current-work.md) - Session bootstrap
- [planning/phase2-progress.md](planning/phase2-progress.md) - Question-by-question tracking

**For Portfolio:**
- [portfolio/README.md](portfolio/README.md) - "The ROG Sessions" portfolio project

---

## Project Dual Purpose

**Railroad Operations Game (ROG)** serves two goals:
1. **Build functional application** - Railroad operations gaming system
2. **Document collaboration** - Human-AI workflow for portfolio piece

**These projects are independent:**
- ROG content self-contained in `PRD/` and `planning/`
- Portfolio content separate in `portfolio/`
- Portfolio never needed to understand ROG

---

## Workflow Phases

### Phase 0: Planning Setup [x] Complete
**Goal:** Establish operational framework, documentation structure
**Deliverables:** Project guidelines, git workflow, decision log structure

### Phase 1: Ideation [x] Complete
**Goal:** Domain knowledge building, problem crystallization
**Deliverables:**
- Domain research (railroad operations, layouts, existing systems)
- 26-question framework
- Problem statement (geographic routing failure)

### Phase 2: PRD Development (Current - 6/26 questions complete)
**Goal:** Answer all 26 questions, define requirements
**Deliverables:**
- Complete Product Requirements Document
- Data models, algorithms, workflows defined
- All decisions documented with rationale

**Progress:** See [planning/phase2-progress.md](planning/phase2-progress.md)

### Phase 3: Implementation Planning
**Goal:** Select tech stack, architecture, deployment strategy
**Deliverables:**
- Technology decisions
- System architecture
- Database schema
- API contracts
- Deployment plan

### Phase 4: Detailed Task Breakdown
**Goal:** Create prioritized implementation task list
**Deliverables:**
- Task breakdown with dependencies
- Critical path identified
- Milestones defined

### Phase 5: Implementation
**Goal:** Build ROG application
**Tool:** Claude Code CLI (primary)
**Deliverables:**
- Working application
- Tests
- Documentation
- Deployment

### Phase 6: Portfolio Content Synthesis
**Goal:** Transform collaboration sessions into portfolio piece
**Deliverables:**
- 30-minute video OR comprehensive blog post
- Demonstrates effective human-AI collaboration
- Shows techniques viewers can apply

---

## Operational Protocols

### Content Preservation Strategy

**Multi-layer approach:**
- Regular checkpointing after major decisions
- Phase gate reviews at end of each phase
- Session summaries documenting key outcomes
- All context in files before PR merge (PR merge = knowledge finalized)

### Session Continuity

**Starting a session:**
1. Read `planning/current-work.md` (session bootstrap)
2. Load Tier 1 context (see CLAUDE.md)
3. Proceed with current task

**Ending a session:**
1. Update `planning/current-work.md`
2. Update relevant PRD sections
3. Update progress tracker if questions answered
4. Optionally create session summary (portfolio content)

### Cross-Environment Sync

**When transferring between Claude.ai and Claude Code CLI:**
- Core files always loaded (CLAUDE.md, PRD/README.md, etc.)
- Phase-specific files based on current work
- Reference files on-demand only

See CLAUDE.md for complete context loading protocol.

### Decision Documentation

**Technical decisions:**
- WHAT we decided → relevant PRD section
- WHY we decided → PRD/decisions-log.md

**Process observations:**
- HOW we collaborated → portfolio/collaboration-patterns.md
- Key moments → portfolio/presentation-artifacts.md

---

## Success Criteria

### For ROG Application

Application is successful when:
- Operators can work geographic assignments without traversing entire layout
- Setup time reduced compared to paper systems
- Reality-first tracking handles fluid session participation
- System works reliably with 30+ second connectivity drops
- Session masters can configure modular layouts quickly
- All defined MVP features function as specified

**Critical validation:** User confirms "This solves the geographic routing problem."

### For Portfolio Piece ("The ROG Sessions")

Portfolio is successful when:
- Demonstrates effective human-AI collaboration patterns
- Shows how human guidance shapes AI output
- Includes decision pivots and critical moments
- Creates coherent narrative from collaboration sessions
- Provides value to audience (techniques they can apply)

**Portfolio success is independent of ROG success** - even if ROG fails technically, portfolio can document the collaboration process.

---

## Tool Ecosystem

**Claude.ai:** Planning, ideation, PRD development, complex discussions
**Claude Code CLI:** File operations, git workflow, quick questions, implementation (Phase 5)
**GitHub:** Version control, PR reviews, collaboration
**VSCode:** Code editing, file management

**Model:** Claude Sonnet 4.5

---

## File Organization Principles

**PRD (Source of Truth):**
- `docs/PRD/` - All requirements, decisions, models
- Self-contained, cross-referenced
- Technical rationale documented

**Planning Materials:**
- `docs/planning/` - Current work, progress, research, specs
- Session bootstrap in `current-work.md`
- Domain knowledge in `research/`

**Portfolio Content:**
- `docs/portfolio/` - Process documentation, patterns, narratives
- Never needed for ROG planning
- Duplicative by design

See CLAUDE.md for complete file organization guidelines.

---

## Context Management

ROG uses three-tier context loading optimized for LLM context windows:

**Tier 1 (CORE):** Always load - essential files
**Tier 2 (ACTIVE):** Load for current work - focused context
**Tier 3 (REFERENCE):** Load on-demand - comprehensive knowledge

See CLAUDE.md for complete context management strategy.

---

## Git Workflow

**Branch Strategy:**
- Never commit directly to main
- Topic branches: `user/tbdye/<NameOfBranch>`

**Before Committing:**
- Always ask for permission first
- Ensure documentation updated
- PR merge finalizes session knowledge

See CONTRIBUTING.md for complete git workflow.

---

## Related Documents

**Primary references:**
- [../CLAUDE.md](../CLAUDE.md) - Claude Code guidance, context management
- [../CONTRIBUTING.md](../CONTRIBUTING.md) - Git workflow, commit standards, PR requirements
- [PRD/README.md](PRD/README.md) - Product requirements index
- [planning/current-work.md](planning/current-work.md) - Session bootstrap

**For specific topics:**
- Platform: [PRD/platform.md](PRD/platform.md)
- Data models: [PRD/data-models.md](PRD/data-models.md)
- Questions: [PRD/open-questions.md](PRD/open-questions.md)
- Progress: [planning/phase2-progress.md](planning/phase2-progress.md)
