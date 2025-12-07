# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Context

**Railroad Operations Game (ROG)** is a persistent backend application with web-based configuration interface, currently in planning/ideation phase. This project serves dual purposes:
1. Build a functional Railroad Operations Game application
2. Document AI-assisted development workflow for portfolio/content creation

**Current Phase:** Phase 1 - Ideation (implementation has not begun)

## Project Workflow Phases

The project follows a structured 6-phase approach:

1. **Phase 1: Ideation** (Current) - Requirements exploration, scope definition
2. **Phase 2: PRD Development** - Product Requirements Document creation
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
│
└── docs/
    ├── ROG-PROJECT-GUIDELINES.md       # Project framework
    ├── portfolio/                      # Portfolio content (Phase 6)
    │   └── decision-log.md             # Collaboration narrative
    └── sessions/                       # Session summaries
        └── YYYY-MM-DD-descriptor.md    # Individual session notes
```

**Primary References:**
- `CONTRIBUTING.md` - **Read this first** for git workflow, branching, and PR standards
- `docs/ROG-PROJECT-GUIDELINES.md` - Complete project framework, workflow phases, operational protocols
- `docs/portfolio/decision-log.md` - Collaboration narrative tracking key decisions and patterns
- `docs/sessions/` - Session summaries from Claude.ai and Claude Code CLI interactions

**Read these files first** to understand:
- The dual-purpose nature of this project (functional app + portfolio documentation)
- How decisions should be captured for future portfolio synthesis
- The cross-environment sync strategy between Claude.ai and Claude Code CLI
- Operational protocols for checkpointing and content preservation

## Development Commands

**Note:** Implementation has not yet begun. Tech stack decisions will be made in Phase 3 based on requirements defined in Phases 1-2.

Once implementation begins, standard commands will be documented here for:
- Build/compilation
- Running tests (unit, integration)
- Linting and formatting
- Local development server
- Deployment

## Architecture Notes

**Status:** Architecture not yet defined. Will be determined in Phase 3 (Implementation Planning).

**Planned Characteristics:**
- Persistent backend application
- Web-based configuration interface
- Target audience, deployment model, and technical decisions deferred until requirements are clear

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

## Important Operational Notes

**Decision Logging:**
When working on this project, notable collaboration moments should be captured in `docs/portfolio/decision-log.md`. Focus on:
- How human guidance shapes AI output
- Pivots in approach or direction
- Critical decisions impacting the project
- Problem-solving patterns

**Session Summaries:**
Create a summary for each work session in `docs/sessions/`:
- Naming: `YYYY-MM-DD-<descriptor>.md`
- Multiple sessions per day: use descriptive suffixes
- Content: accomplishments, decisions, next steps, blockers

**Checkpoint Strategy:**
This project uses multi-layer content preservation:
- Regular checkpointing after major decisions
- Phase gate reviews at end of each phase
- Session summaries documenting key outcomes
- All context must be in files before PR merge

**File Organization:**
- Use Markdown (.md) for documentation
- Clear, descriptive naming conventions
- Portfolio content lives in `docs/portfolio/`
- Session summaries live in `docs/sessions/`

## Portfolio Context

This project captures human-AI collaboration patterns for a future portfolio piece (blog post or video). When working on implementation:
- Mark key collaboration moments in decision log
- Focus on showing collaboration effectiveness, not just technical output
- Document decision pivots and critical moments
- Demonstrate effective prompting and guidance techniques

## Current Status

- **Phase 0: Planning Setup** ✅ Complete
- **Phase 1: Ideation** - In progress, no code written yet
- No technical stack selected
- No implementation files exist
- Repository contains only planning documentation and Git structure

When implementation begins (Phase 5), this file will be updated with concrete build commands, architecture details, and development workflows.
