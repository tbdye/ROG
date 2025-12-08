# Railroad Operations Game (ROG)

A persistent backend application with web-based configuration interface for managing railroad operations.

## Project Status

**Current Phase:** Phase 2 - PRD Development (Active)

This project is in PRD development phase. Platform architecture decisions complete (PWA, cloud backend, offline-tolerant). Next: Geographic routing algorithm design.

## Project Overview

ROG (Railroad Operations Game) serves a dual purpose:

1. **Primary Goal:** Build a functional railroad operations management application
2. **Secondary Goal:** Document AI-assisted development workflow for portfolio/content creation

The project follows a structured 6-phase development approach, from ideation through implementation to portfolio content synthesis.

## Repository Structure

```
ROG/
├── CLAUDE.md                        # Guidance for Claude Code
├── CONTRIBUTING.md                  # Git workflow and PR standards
├── README.md                        # This file
│
└── docs/
    ├── ROG-PROJECT-GUIDELINES.md    # Project framework and workflow
    ├── phase2-prd-progress.md       # Phase 2 progress tracker
    ├── portfolio/                   # Portfolio synthesis materials
    │   ├── decision-log.md          # Collaboration narrative
    │   └── artifacts/               # Research reports and planning materials
    └── sessions/                    # Session summaries
        └── YYYY-MM-DD-descriptor.md # Individual session notes
```

## Getting Started

This project is currently in Phase 2 (PRD Development). Platform architecture has been established (PWA, cloud backend, offline-tolerant). Technical stack selection and implementation details will be defined during Phase 3 (Implementation Planning).

For contributors and AI assistants:
- Read `CONTRIBUTING.md` for git workflow and standards
- Read `CLAUDE.md` for Claude Code operational guidance
- Read `docs/ROG-PROJECT-GUIDELINES.md` for complete project context

## Working with Claude

This repository includes special commands and protocols for effective AI-assisted development:

### Special Queries

**"What files need syncing?"** - When working across Claude.ai and Claude Code CLI, use this query to get a layered response of which files need to be synced to maintain full context:
- **CORE**: Always-loaded files (CLAUDE.md, CONTRIBUTING.md, ROG-PROJECT-GUIDELINES.md, decision-log.md)
- **PHASE-SPECIFIC**: Current phase trackers and recent session summaries
- **REFERENCE**: Optional large files to load on-demand

See `CLAUDE.md` for full details on the layered context strategy.

### Pre-Commit Review Protocol

Before committing new documents, **ALWAYS** verify:
- File naming follows established conventions (see CLAUDE.md documentation structure)
- New directories are documented in README and CLAUDE.md
- Session summaries use `YYYY-MM-DD-descriptor.md` format
- Content completeness for current phase

**Standard query:** "Look through uncommitted changes and verify naming conventions match our documented standards."

## Development Phases

1. **Phase 1: Ideation** [x] Complete - Requirements exploration and scope definition
2. **Phase 2: PRD Development** (Current) - Product Requirements Document creation
3. **Phase 3: Implementation Planning** - Tech stack, architecture, deployment strategy
4. **Phase 4: Detailed Task Breakdown** - Task prioritization and dependencies
5. **Phase 5: Implementation** - Active development
6. **Phase 6: Portfolio Content Synthesis** - Create portfolio deliverable from collaboration narrative

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed information about:
- Branch naming conventions
- Commit message standards
- Pull request requirements
- Git workflow

## Documentation

- **Project Framework:** `docs/ROG-PROJECT-GUIDELINES.md`
- **Decision Log:** `docs/portfolio/decision-log.md`
- **Session Summaries:** `docs/sessions/`

## License

TBD