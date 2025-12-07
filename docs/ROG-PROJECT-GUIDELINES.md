# Railroad Operations Game (ROG) - Project Guidelines

**Last Updated:** December 6, 2024  
**Current Phase:** Pre-Ideation / Planning Setup

---

## Project Overview

**Project Name:** Railroad Operations Game (ROG)  
**Type:** Persistent backend application with web-based configuration interface

### Dual Purpose

This project has two equally important objectives:

1. **Primary:** Build a functional Railroad Operations Game application
2. **Secondary:** Document the AI-assisted development workflow for portfolio/content creation
   - Target output: Blog post OR ~30 minute video
   - Focus: Demonstrate human-AI collaboration in software development
   - Critical: Clearly distinguish human contributions from AI contributions

---

## Tool Ecosystem

| Tool | Purpose | Usage Phase |
|------|---------|-------------|
| **Claude.ai** | Planning, ideation, document creation, problem-solving | All phases, primary for planning |
| **Claude Code CLI** | Code implementation, file manipulation, debugging | Implementation phase |
| **GitHub** | Version control, source code repository | Implementation phase onward |
| **VSCode** | Code editing, local development | Implementation phase |
| **Windows PC** | Development environment | All phases |

### Model Selection
- **Claude Sonnet 4.5** - Default for planning and complex reasoning
- Adjust if specific needs arise

### Cross-Environment Sync Strategy

**Manual sync required** between Claude.ai and local filesystem/Code CLI:

**Important: Project Files vs. Chat Files**
- Upload files **to the Project** (not individual chats) for persistence
- Project files are accessible across all conversations in this project
- When Claude updates documents, download new version and re-upload to Project
- This avoids re-uploading in every new conversation

**Phases 1-4 (Planning - Claude.ai Primary):**
- Claude creates documents and provides download links (blue links in responses)
- **You must click links to view, then download files using download button**
- Files are NOT automatically in your local folder - manual download required
- Save downloaded files to local `Railroad Operations Game/` folder
- **Upload to PROJECT (not just chat)** so they persist across conversations
- Commit to Git for version control
- When documents are updated, download new version and re-upload to Project

**Phase 5 (Implementation - Local/Code CLI Primary):**
- All active work documents live in local folder
- Claude Code CLI works directly with local files
- Return to Claude.ai ONLY for strategic questions
- Upload relevant context when needed for strategic input

**Decision Log (Special Case):**
- Maintained locally by human in project folder
- Updated in either environment as notable moments occur
- Excerpts shared with Claude.ai when strategically relevant
- Full version uploaded to Claude.ai for Phase 6 synthesis

**Sync Best Practices:**
- Use Git as single source of truth
- Commit after each Claude.ai checkpoint
- Clear file naming (avoid version proliferation)
- When uploading to Claude.ai, note what changed since last version

---

## Workflow Phases

### Phase 0: Planning Setup ✓ (Current)
- Define project structure
- Establish guidelines
- Capture initial goals

### Phase 1: Ideation (Next)
- Share domain knowledge (images, context)
- Explore requirements and features
- Define target audience and use cases
- Establish scope boundaries

### Phase 2: PRD Development
- Select appropriate PRD template
- Fully document ROG concept
- Define success criteria
- Establish MVP vs. future enhancements

### Phase 3: Implementation Planning
- Define tech stack
- Design architecture
- Create deployment strategy
- Establish testing approach

### Phase 4: Detailed Task Breakdown
- Create prioritized task list
- Define implementation order
- Identify dependencies
- Set phase gates/milestones

### Phase 5: Implementation (Claude Code CLI)
- Progressive build and test
- Component-by-component development
- Regular commits with clear messages
- Mark key collaboration moments in decision log

### Phase 6: Portfolio Content Synthesis (Separate Project)
- Review all captured conversations and decision log
- Identify compelling collaboration examples
- Extract decision pivots and critical moments
- Build narrative showing human-AI collaboration patterns
- Create final deliverable (blog post or video)
- Highlight effective prompting and guidance techniques

---

## Documentation Standards

### File Organization
- **Markdown (.md)** for version-controlled documents (PRD, plans, architecture)
- **Clear naming convention:** Use descriptive names with dates when relevant
- **Version tracking:** Major revisions should be noted in documents

### Git Practices
- **Commit messages:** Clear, descriptive, showing what changed and why
- **Branches:** TBD based on implementation strategy
- **Meaningful commits:** Group related changes logically for portfolio clarity

### Content Capture for Portfolio
- All Claude.ai conversations automatically saved in project
- **Focus: Collaboration patterns, not raw technical output**
- **What to capture:**
  - How human guides the AI
  - How AI responds and adapts
  - Decision pivots and critical moments
  - Direction changes and why they happened
  - Examples of effective prompting techniques
  
**Key Insight:** This initial framing conversation is valuable portfolio content

**Challenge:** Organizing vast amounts of captured information into a coherent narrative later

**Solution Strategy:**
- Create a separate "Decision Log" document to mark key moments as they happen
- Tag conversations with phase markers
- Use Claude.ai's conversation search to retrieve specific topics
- Phase 6 will be a dedicated project to synthesize content into deliverable

---

## Session Handoff Protocol

### Starting Each Session
1. Review current phase in this document
2. Recall previous session's outcomes
3. Identify any blocking questions
4. Confirm next steps

### Ending Each Session
1. Update current phase if changed
2. Note any key decisions made
3. Identify next session's starting point
4. **Create session summary file**
5. **Checkpoint all updates to documents**
6. Save any new artifacts to project folder

---

## Operational Protocols

### Conversation Management

**Token Usage Monitoring:**
- Claude.ai has ~200k token context window (no auto-compacting)
- Token counter may not be visible in web UI (Claude tracks internally)
- **Committed warning thresholds:**
  - **80% (~152k tokens):** ⚠️ First warning - consider checkpointing soon
  - **90% (~171k tokens):** ⚠️ Second warning - checkpoint critical content now
  - **95% (~180.5k tokens):** 🛑 CRITICAL - MANDATORY STOP for full preservation
- At 95%, Claude will stop current work and preserve ALL context to documents/logs
- Plan checkpointing well before limits

**Content Capture Instructions:**
- **Explicit:** "Add this to [document]" / "Update the guidelines with..."
- **Implicit:** Claude will offer: "Should I log this?" / "Want me to update X?"
- **Default Behavior:** Claude is proactive but asks before updating (no auto-updates)

### Preventing Content Loss - Multi-Layer Safety

**Layer 1: Regular Checkpointing**
- After major discussion topics
- Claude asks: "Should we checkpoint progress to documents?"
- Downloads key decisions to files

**Layer 2: Phase Gate Reviews**
- End of each phase = comprehensive save
- Update ALL relevant documents
- Confirm no conversation-only content remains

**Layer 3: Session Summaries**
- Session end = create "Session-YYYY-MM-DD-summary.md"
- Captures: key decisions, next steps, log-worthy moments
- Quick reference for what happened this session

**Layer 4: Strategic Save Points**
- Before major pivots or complex discussions
- Human triggers: "Let's checkpoint before we dive into this"
- Claude creates snapshot of current state

### Claude's Proactive Behaviors

**Will automatically:**
- Monitor and warn about token usage
- Suggest checkpointing after significant decisions
- Offer to update documents when relevant info emerges
- Remind about syncing if too long between saves

**Will NOT automatically:**
- Update documents without confirmation
- Assume what goes in Decision Log vs. other docs
- Start new conversations (human controls this)

---

## Open Questions & Decisions Needed

### Technical Decisions (Address in Phases 1-3)
- [ ] Tech stack (backend language/framework) - **Open-minded, will be driven by requirements**
- [ ] Database selection
- [ ] Frontend framework
- [ ] Deployment target (local/cloud/self-hosted) - **To be determined with ROG requirements**
- [ ] Testing framework/strategy

### Scope Decisions (Address in Phases 1-2)
- [ ] Target audience definition
- [ ] MVP feature set - **Will be negotiated during planning**
- [ ] Future enhancement categories
- [ ] Performance/scale requirements

### Content Creation (Address Throughout + Phase 6)
- [ ] Specification for portfolio output (what makes "good" collaboration content?)
- [ ] Decision log format and tagging strategy
- [ ] Key milestone identification criteria
- [ ] Blog vs. video format decision
- [ ] Timeline for Phase 6 content synthesis project

---

## Success Criteria

### For ROG Application
- Application is **usable according to specifications defined in PRD**
- All defined features function as intended
- Meets quality and performance standards set during planning

### For Portfolio Piece
- **Separate project phase** to transform captured content into deliverable
- Demonstrates effective human-AI collaboration patterns
- Shows how humans guide LLMs to produce valuable work
- Includes examples of:
  - Decision pivots and direction changes
  - Critical moments that shaped the project
  - Effective prompting and guidance techniques
- Creates a coherent narrative from collaboration sessions
- Final format: 30-minute video OR comprehensive blog post
- Target audience: Developers/creators interested in AI-assisted workflows

---

## Notes & Context

### Project Folder
- Located at: `Railroad Operations Game/`
- Contains: Planning docs, architecture, task lists, and reference materials

### Key Context Documents
- This file (ROG-PROJECT-GUIDELINES.md)
- [Future: ROG-PRD.md]
- [Future: ROG-ARCHITECTURE.md]
- [Future: ROG-TASK-LIST.md]

### Important Reminders
- This is a **side project** - no rush, quality over speed
- Claude PRO subscription available - use features as needed
- Domain knowledge explanation (with images) coming in Phase 1

---

## Change Log

| Date | Change | Phase |
|------|--------|-------|
| 2024-12-06 | Initial guidelines document created | Phase 0 |
| 2024-12-06 | Updated with tech stack, deployment, scope clarifications | Phase 0 |
| 2024-12-06 | Refined success criteria and content capture strategy | Phase 0 |
| 2024-12-06 | Added cross-environment sync strategy | Phase 0 |
| 2024-12-06 | Added operational protocols for conversation management | Phase 0 |

