# Contributing to ROG

This document outlines the git workflow, branching strategy, and contribution standards for the Railroad Operations Game (ROG) project.

## Git Workflow

### Branch Protection

**Never commit directly to `main`.** All changes must go through pull requests.

### Branching Strategy

All development work happens on topic branches following this naming convention:

```
user/tbdye/<NameOfBranch>
```

**Examples:**
- `user/tbdye/initial-repo-structure`
- `user/tbdye/add-database-schema`
- `user/tbdye/fix-authentication-bug`

Branch names should be:
- Descriptive of the work being done
- Kebab-case (lowercase with hyphens)
- Concise but clear

### Creating a Branch

```bash
git checkout -b user/tbdye/<NameOfBranch>
```

## Commit Standards

### Commit Messages

Follow software engineering best practices for commit messages:

**Structure:**
```
Short summary (50 characters or less)

Detailed explanation of what changed and why (if needed).
Wrap at 72 characters.
```

**Guidelines:**
- Use imperative mood ("Add feature" not "Added feature")
- First line is a concise summary
- Leave blank line between summary and body
- Body explains what and why, not how (code shows how)
- Reference issues/PRs when relevant

**Good examples:**
```
Add session summary documentation structure

Create docs/sessions/ directory to organize session summaries
from both Claude.ai and Claude Code CLI interactions.
```

```
Fix database connection timeout

Increase timeout from 5s to 30s for initial connection.
Long timeout needed for cold starts in cloud environment.
```

**Bad examples:**
```
Updated files
```

```
Fixed bug
```

```
WIP
```

### Commit Frequency

- Commit logical units of work
- Each commit should represent a complete, working change
- Avoid "WIP" commits on shared branches
- Smaller, focused commits are better than large, monolithic ones

## Pull Request Standards

### Before Creating a PR

1. **Ensure all work is committed** on your topic branch
2. **Update documentation** - All relevant docs must be updated before the PR
3. **Test your changes** (once implementation begins)
4. **Push your branch** to the remote repository

### Creating a Pull Request

**Always ask before creating a pull request**, even if you have permission to proceed.

### PR Description Template

Every pull request must include:

#### Executive Summary
A concise overview of what this PR accomplishes (2-4 sentences).

#### Notable Changes
- Bulleted list of significant changes
- Focus on what changed, not implementation details

#### Why These Changes Were Made
Explain the reasoning and context:
- What problem does this solve?
- What requirement does this fulfill?
- What decision led to this approach?

#### How (for non-trivial changes)
For complex or non-obvious changes, explain the approach:
- What pattern or technique was used?
- Why was this approach chosen over alternatives?
- Any important implementation details

#### Testing
(Once implementation begins)
- How were changes verified?
- What test cases were added/modified?

#### Documentation Updates
- List documentation files updated in this PR
- Confirm all necessary docs are current

**Example PR Description:**

```markdown
## Executive Summary

Establish initial repository structure with clear separation between project
documentation, portfolio content, and session summaries. Create git workflow
standards for future contributions.

## Notable Changes

- Created `CONTRIBUTING.md` with git workflow and PR standards
- Reorganized docs into purpose-based directories (`portfolio/`, `sessions/`)
- Updated `CLAUDE.md` with documentation structure guidance
- Added `README.md` with project overview

## Why These Changes Were Made

ROG project requires clear separation between:
1. Human-readable project documentation
2. Claude operational guidance
3. Portfolio synthesis materials

Establishing structure now prevents confusion as documentation accumulates
across multiple Claude.ai and Code CLI sessions.

## How

Created three-tier documentation structure:
- Root level: Operational files (CLAUDE.md, CONTRIBUTING.md, README.md)
- docs/portfolio/: Content for Phase 6 portfolio synthesis
- docs/sessions/: Session summaries with date-descriptor naming

## Documentation Updates

- Created: CONTRIBUTING.md, README.md
- Updated: CLAUDE.md
- Moved: docs/ROG-DECISION-LOG.md → docs/portfolio/decision-log.md
- Renamed: docs/Session-2024-12-06-summary.md → docs/sessions/2024-12-06-initial-setup.md
```

### PR Review Process

1. Create PR from `user/tbdye/<NameOfBranch>` → `main`
2. Human (tbdye) reviews the PR
3. Human manually approves and merges
4. Delete topic branch after merge

## Session Workflow

### End-of-Session Requirements

**Before merging a PR, ensure:**

1. All documentation updates are committed
2. Session summary is created in `docs/sessions/`
3. Decision log is updated (if notable collaboration moments occurred)
4. All relevant context is captured in files

**PR merge represents finalization of session knowledge.** Nothing should exist only in conversation memory.

### Session Summary Creation

Each work session should produce a summary in `docs/sessions/`:

**Naming:** `YYYY-MM-DD-<descriptor>.md`

**Content:**
- What was accomplished
- Key decisions made
- Notable collaboration moments (reference decision log)
- Next steps
- Any blockers or open questions
- **Presentation Artifacts** (see below)

### Presentation Artifacts in Session Summaries

Session summaries should include a "Presentation Artifacts" section to capture material suitable for portfolio presentation (blog post, video, slides).

**What to Capture:**
- Direct quotes/excerpts of human instructions (when they show essential guidance patterns)
- Conversation excerpts that would make good visual artifacts
- Interaction style notes (prescriptive vs. collaborative approach and why)
- Key decisions with context about how they were reached
- Contrast with other sessions when relevant

**What NOT to Capture as Artifacts:**
- Long transcripts (extract key quotes instead)
- File structure diagrams or diffs (metadata only)
- Information better preserved as raw metadata

**Visual Artifacts (Images/Diagrams):**
- Store in `docs/portfolio/artifacts/` directory
- Reference from session summary using relative links
- Include descriptive filenames: `2024-12-06-repo-structure-diagram.png`
- Particularly important for images uploaded during Claude.ai ideation sessions

**Goal:** Preserve presentation-ready material that demonstrates effective human-AI collaboration patterns, not just project history.

### Multiple Sessions Per Day

When multiple sessions occur on the same day:
- Use descriptive suffixes: `2024-12-06-repo-structure.md`, `2024-12-06-ideation-01.md`
- Or add time if needed: `2024-12-06-morning-planning.md`

## Documentation Maintenance

### Source of Truth

**The GitHub repository is the source of truth** for all project documentation.

### Syncing with Claude.ai

When working in Claude.ai for planning phases:
1. Download updated files from Claude.ai
2. Save to local repository
3. Commit changes following standards above
4. Create PR for review

### Cross-Environment Context Transfer

After merging a PR:
1. Updated docs are now in repository (source of truth)
2. Upload relevant context to Claude.ai Project files (not just chat)
3. Reference PR or commit in Claude.ai if context needed

## Questions?

Refer to:
- `CLAUDE.md` for Claude Code operational guidance
- `docs/ROG-PROJECT-GUIDELINES.md` for overall project framework
- `docs/portfolio/decision-log.md` for collaboration patterns and decisions
