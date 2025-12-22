# Railroad Operations Game (ROG)

A geographic-aware railroad operations management system with PWA interface and cloud backend.

## Project Status

**Current Phase:** Phase 2 - PRD Development (6/26 questions answered)

**Progress:**
- [x] Q1: Layout Topology Representation - Complete
- [x] Q22-26: Platform Architecture - Complete
- **Next:** Q3 - Geographic Routing Algorithm

## Project Overview

ROG serves a dual purpose:

1. **Primary Goal:** Build a functional railroad operations management application that solves the geographic routing problem
2. **Secondary Goal:** Document AI-assisted development workflow for portfolio piece ("The ROG Sessions")

**These projects are independent** - portfolio content not needed to understand ROG.

The project follows a structured 6-phase development approach, from ideation through implementation to portfolio content synthesis.

## Repository Structure

```
ROG/
├── CLAUDE.md                        # Claude Code guidance
├── CONTRIBUTING.md                  # Git workflow and PR standards
├── README.md                        # This file
├── ROG-PROJECT-GUIDELINES.md        # Project framework (lightweight)
│
└── docs/
    ├── PRD/                         # ★ SOURCE OF TRUTH FOR ROG
    │   ├── README.md                # PRD index
    │   ├── overview.md              # Problem statement, vision, scope
    │   ├── platform.md              # Platform architecture (Q22-26)
    │   ├── data-models.md           # Data models (Q1, Q6, Q7)
    │   ├── routing-algorithm.md     # Geographic routing (Q3)
    │   ├── user-workflows.md        # Role workflows (Q10)
    │   ├── car-lifecycle.md         # Car states (Q2)
    │   ├── open-questions.md        # All 26 questions, status
    │   └── decisions-log.md         # Technical rationale
    │
    ├── planning/                    # ROG PLANNING MATERIALS
    │   ├── current-work.md          # ★ SESSION BOOTSTRAP
    │   ├── phase2-progress.md       # Question tracking
    │   ├── research/                # Domain knowledge
    │   │   ├── domain-knowledge.md
    │   │   └── yardless-operations.md
    │   └── specifications/          # Detailed specs
    │       └── topology-data-model.md
    │
    └── portfolio/                   # "The ROG Sessions" Portfolio
        ├── README.md                # Portfolio project overview
        ├── collaboration-patterns.md # Synthesized patterns
        ├── presentation-artifacts.md # Curated quotes
        └── sessions/                # Process narratives
            ├── 2024-12-06-initial-setup.md
            ├── 2024-12-07-phase1-domain-research.md
            ├── 2024-12-07-platform-architecture.md
            └── 2025-12-20-topology-representation.md
```

## Getting Started

### For Contributors

1. Read `CONTRIBUTING.md` for git workflow and PR standards
2. Read `CLAUDE.md` for Claude Code operational guidance
3. Read `docs/PRD/README.md` for requirements index
4. Check `docs/planning/current-work.md` for current session focus

### For Understanding ROG

**Start with:**
- `docs/PRD/README.md` - Requirements index
- `docs/PRD/overview.md` - Problem statement and vision

**For specific topics:**
- Platform decisions: `docs/PRD/platform.md`
- Data models: `docs/PRD/data-models.md`
- All questions: `docs/PRD/open-questions.md`
- Current work: `docs/planning/current-work.md`

### For Portfolio Context

**"The ROG Sessions"** portfolio content lives in `docs/portfolio/`

- Process documentation (how we collaborated)
- Never needed to understand ROG planning
- Separate project, duplicative content

## Session Management

### Starting a Session

**Recommended phrasing:**
- **"Start ROG session"** or **"Resume ROG work"**

**What happens:**
1. Claude loads `docs/planning/current-work.md` (session bootstrap)
2. Claude tells you what Tier 1 + Tier 2 files are loaded
3. You can request additional files if needed

**Example:**
```
You: "Start ROG session"

Claude loads:
- Tier 1 (CORE): CLAUDE.md, PRD/README.md, PRD/overview.md
- Tier 2 (ACTIVE): PRD/data-models.md, planning/phase2-progress.md

Ready to work on Q3 - Geographic Routing Algorithm
```

### Preparing for Next Session

**Get homework questions:**
- **"What should I prepare for the next session?"**
- **"What questions do you have about [Question/Topic]?"**

**What happens:**
Claude identifies:
- Open questions needing your input
- Background information to gather
- Decisions to think about
- Context to prepare

**Example:**
```
You: "What should I prepare for Question 3?"

Claude responds with:
- Branch detection algorithm questions
- Complex routing scenarios to consider
- Examples to bring (Grain Elevator case)
- Definitions to clarify
```

**Benefit:** Come back with answers ready, accelerate collaboration.

### Ending a Session (Two Scenarios)

#### Scenario 1: Checkpoint (Pause and Resume Later)

**Use when:** Need to clear context window but will continue work later

**Recommended phrasing:**
- **"Checkpoint session"** or **"Pause and save state"**

**What happens:**
1. Update `docs/planning/current-work.md`:
   - What we just completed
   - What we're working on now
   - What's next when we resume
   - Any blockers
2. Save any in-progress decisions to PRD sections
3. Update `planning/phase2-progress.md` if questions answered
4. **All changes written to disk** (verify before ending)
5. Session can resume anytime with fresh context

**Critical:** All work must be in files before ending. Conversation-only content is lost.

#### Scenario 2: Complete (Finalize and Create PR)

**Use when:** Completed a task/question, ready to merge to main

**Recommended phrasing:**
- **"Complete session and prepare for PR"** or **"Wrap up for merge"**

**What happens:**
1. All checkpoint steps (above)
2. Update relevant PRD sections with final decisions
3. Add decision rationale to `docs/PRD/decisions/` (categorized)
4. Update `docs/PRD/open-questions.md` status
5. Optionally create session summary in `docs/portfolio/sessions/`
6. **Verify all files saved**
7. Create branch (if not exists)
8. Create PR for review
9. **After PR merge, no additional work** - session fully finalized

**Critical:** PR captures snapshot at commit time. Anything not in files is lost.

### Context Loading

This repository uses a **three-tier context loading strategy** optimized for LLM context windows.

**Query:** **"What context do I need?"** or **"What should I load for current work?"**

Response based on `docs/planning/current-work.md`:
- **Tier 1 (CORE):** Essential files, always load
- **Tier 2 (ACTIVE):** Current work context
- **Tier 3 (REFERENCE):** On-demand reference

See `CLAUDE.md` for complete details.

## Development Phases

1. **Phase 1: Ideation** [x] Complete - Domain research, 26-question framework
2. **Phase 2: PRD Development** (Current) - Answering questions, defining requirements
3. **Phase 3: Implementation Planning** - Tech stack, architecture, deployment
4. **Phase 4: Detailed Task Breakdown** - Task prioritization, dependencies
5. **Phase 5: Implementation** - Active development (Claude Code CLI primary)
6. **Phase 6: Portfolio Content Synthesis** - Blog post or video

## Technology Stack

**Status:** Platform architecture complete, implementation stack TBD in Phase 3

**Platform decisions (Q22-26):**
- Progressive Web App (PWA) with cloud backend
- Offline-tolerant (30+ second connection drops)
- Real-time multi-device collaboration
- Car ownership model prevents conflicts
- Session master role with admin authority

See `docs/PRD/platform.md` for complete details.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed information about:
- Branch naming conventions (`user/tbdye/<NameOfBranch>`)
- Commit message standards
- Pull request requirements
- Git workflow (never commit directly to main)

## Documentation

**ROG Requirements:**
- **PRD Index:** `docs/PRD/README.md`
- **Overview:** `docs/PRD/overview.md`
- **Open Questions:** `docs/PRD/open-questions.md`

**Project Framework:**
- **Guidelines:** `docs/ROG-PROJECT-GUIDELINES.md`
- **Current Work:** `docs/planning/current-work.md`
- **Progress:** `docs/planning/phase2-progress.md`

**Portfolio:**
- **Portfolio Overview:** `docs/portfolio/README.md`
- **Collaboration Patterns:** `docs/portfolio/collaboration-patterns.md`

## License

TBD (decision deferred to Phase 3 - Implementation Planning)
