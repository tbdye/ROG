# Railroad Operations Game (ROG) - Project Guidelines

**Last Updated:** December 7, 2024  
**Current Phase:** Phase 2 - PRD Development (Active - Platform Architecture Complete)

---

## Project Overview

**Project Name:** Railroad Operations Game (ROG)  
**Type:** Persistent backend application with web-based configuration interface

### Dual Purpose

This project has two equally important objectives:

1. **Primary:** Build a functional Railroad Operations Game application
2. **Secondary:** Document the AI-assisted development workflow for portfolio/content creation
   - **Project Name:** "The ROG Sessions"
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

**File Sync Query (Layered Context Strategy):**

At any point, ask Claude Code CLI: **"What files need syncing to Claude.ai?"**

The response provides three tiers:

**CORE (Always in Claude.ai Project, update if modified):**
- `CLAUDE.md`
- `CONTRIBUTING.md`
- `ROG-PROJECT-GUIDELINES.md`
- `docs/portfolio/decision-log.md`

These stay loaded permanently, updated when modified.

**PHASE-SPECIFIC (Current work, add/update as needed):**
- Active phase progress tracker (e.g., `docs/phase2-prd-progress.md`)
- Latest session summaries
- Draft documents under development (e.g., PRD)

These change frequently, swap in/out based on current phase.

**REFERENCE (Optional, load on-demand):**
- Large research reports (load if discussing specific domain topics)
- Older session summaries (unless reviewing collaboration patterns)
- Previous phase artifacts (if referencing past decisions)

This prevents uploading unnecessary large files while ensuring critical context is never missing.

---

## Workflow Phases

### Phase 0: Planning Setup [x] (Current)
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

### Phase 6: Portfolio Content Synthesis (Separate Project - "The ROG Sessions")
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
4. **Create session summary file** in `docs/sessions/YYYY-MM-DD-descriptor.md`
   - Use date-first naming format (e.g., `2024-12-07-phase1-domain-research.md`)
   - Descriptive suffix identifies session focus
   - Multiple sessions same day: use distinct descriptors
5. **Checkpoint all updates to documents**
6. Save any new artifacts to project folder

---

## Operational Protocols

### Conversation Management

**Token Usage Monitoring:**
- Claude.ai has ~200k token context window (no auto-compacting)
- Token counter may not be visible in web UI (Claude tracks internally)
- **Committed warning thresholds:**
  - **80% (~152k tokens):** [!] First warning - consider checkpointing soon
  - **90% (~171k tokens):** [!!] Second warning - checkpoint critical content now
  - **95% (~180.5k tokens):** [CRITICAL] MANDATORY STOP for full preservation
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
- [x] **Deployment target** - Cloud-based PWA (Progressive Web App), no app store required
- [x] **Hosting strategy** - Cloud backend (Azure or cost-effective alternative)
- [x] **Offline capability** - 30+ second drop tolerance, optimistic sync with car ownership model
- [x] **Multi-device sync** - Real-time collaboration, server-authoritative
- [ ] Tech stack (backend language/framework) - **Open-minded, will be driven by requirements**
- [ ] Database selection
- [ ] Frontend framework (React/Vue/Svelte for PWA)
- [ ] Testing framework/strategy

### Scope Decisions (Address in Phases 1-2)
- [x] **Target audience** - Community-wide: any club, operator, layout owner
- [x] **Business model** - Freemium: free single operator, paid multi-operator sessions
- [x] **License/openness** - Open development (public GitHub), license deferred to implementation
- [x] **Integration scope** - MVP includes stat tracking; JMRI import, export, sharing deferred
- [x] **Physical artifacts** - Pure digital for MVP, paper backup consideration for future
- [ ] MVP feature set - **Being defined in Phase 2 PRD**
- [ ] Future enhancement categories
- [ ] Performance/scale requirements

### Content Creation (Address Throughout + Phase 6)
- [ ] Specification for portfolio output (what makes "good" collaboration content?)
- [ ] Decision log format and tagging strategy
- [ ] Key milestone identification criteria
- [ ] Blog vs. video format decision
- [ ] Timeline for Phase 6 content synthesis project
- [x] **Name for portfolio/content project** - "The ROG Sessions"

### File Organization Standards

**Session Summaries** (`docs/sessions/`):
- Format: `YYYY-MM-DD-descriptor.md`
- Date-first for chronological sorting
- Descriptor identifies session focus
- Multiple sessions same day: use distinct descriptors
- Example: `2024-12-07-phase1-domain-research.md`

**Research Artifacts** (`docs/portfolio/artifacts/`):
- Comprehensive research reports generated during planning
- Domain knowledge documents
- Analysis of existing systems
- Not final portfolio content - working materials that inform it

**Portfolio Content** (`docs/portfolio/`):
- Decision log (ongoing collaboration narrative)
- Artifacts subdirectory for research materials
- Final synthesis happens in Phase 6

**Phase Progress Trackers** (`docs/`):
- Named by phase: `phase2-prd-progress.md`, `phase4-task-breakdown.md`
- Track completion of phase-specific work
- Provide quick status snapshots
- Archived when phase completes

### Phase 2 (PRD) Question Framework

**Generated December 7, 2024 during assumption testing**

These questions must be addressed during PRD development. Organized by priority and category.

#### CRITICAL (Must Address in PRD)

**1. Layout Topology Representation**
- What specific data describes a module? (dimensions, track endpoints, switch positions, facing/trailing orientation?)
- How are connections between modules represented?
- For FREE-MO vs NTRAK, what interface data differs?
- Do curves matter operationally or just physically?
- How is "this industry is on the West Branch" captured in the data model?

**2. Car State Lifecycle**
- What are ALL the states a car can be in? (staged, assigned, in-transit, spotted, loading, loaded, unloading, empty, available, out-of-game?)
- What triggers state transitions?
- Who can change car states? (system auto, session master, operators?)

**3. Geographic Routing Algorithm (THE CORE PROBLEM)**
- What's the actual algorithm for geographic routing?
- How does "build a train for the West Branch" work?
- What defines a "branch"? Explicit in data model or inferred from topology?
- How does system know which industries are "logically grouped"?
- Should routing prefer fewer trains with longer routes, or more trains with focused routes?

**4. Time Modeling**
- Is time discrete (session 1, session 2) or continuous?
- Do fast clocks affect ROG or just operator perception?
- How long does loading/unloading take? (fixed time? variable? operator-triggered?)
- Can a car be "loading" and unavailable for pickup?
- If multi-day sessions exist, how is time tracked between days?

**5. Staging Mechanics**
- Is staging just "off-layout storage" or does it represent specific other locations?
- When a train "goes to staging," where did it actually go (fictionally)?
- How do cars enter staging initially? (session master places them? system generates them?)
- Can staging be empty or must it always have trains ready?
- Difference between hidden staging vs visible interchange track?

#### IMPORTANT (Affects Design)

**6. Car Identity and Granularity**
- Duplicate cars exist - how should ROG handle this?
- Is "box car" sufficient granularity or do you need "50-foot insulated box car"?
- When checking in cars, what identifiers are required vs optional?
- Car ownership tracking - is this core data or metadata?

**7. Industry and Spot Definitions**
- What's the actual data model for an industry spot?
- Can multiple cars share one spot?
- How is spot capacity determined?
- How specific are commodity matchings? (coal -> any hopper, or coal -> open-top hopper only?)
- Do industries have operating hours or service schedules that affect availability?

**8. Train Consist Building Rules**
- Beyond geographic routing, what other constraints exist?
- Car order in consist (hazmat rules, reefer placement, weight distribution)?
- Maximum train length limits?
- Locomotive power calculations?
- Or is it purely geographic and everything else is operator judgment?

**9. Multi-Session Continuity**
- For permanent layouts: state just persists, right?
- For modular layouts: if layout is disassembled, what happens to car positions?
- Do cars "go into storage" between shows?
- How does state transfer when the same modules are reconfigured differently?

**10. Role-Specific Workflows**
- **Yardmaster:** What does a yardmaster actually DO minute-by-minute? Building consists manually or system suggests? Authority to override routing?
- **Dispatcher:** What information do dispatchers need that conductors don't? Controlling track warrants or just coordinating? Relevant for all layouts or only multi-train?
- **Local Switcher vs Through Freight:** Fundamentally different workflows or same workflow with different scope?

#### REFINEMENT (Can Defer to Detailed Design)

**11. "Checking In Cars" Workflow**
- What's the actual UI/process for this?
- What happens if someone checks in a car that's already in the system?
- Can multiple people check in cars simultaneously?
- What validation occurs?

**12. Session State Tracking**
- How granular is position tracking? (exact track? just location? "somewhere in this town"?)
- When an operator says "I spotted this car" - what actually gets recorded?
- What happens when reality diverges (operator says car is at Industry A but system thinks Industry B)?
- Is there an "undo" mechanism?

**13. Train Assignment and Handoff**
- How does a car get assigned to a train?
- **ANSWERED:** Cars CANNOT be on multiple trains' manifests simultaneously (prevents deadlocks)
- When operator abandons a train, what happens to cars "in transit"?
- Can another operator just pick up that train's manifest and continue?

**14. Missing or Misdelivered Cars**
- When a car is physically missing, what should happen in system?
- Mark as "lost"? Remove from session? Leave in wrong location?
- How is it recovered later?

**15. Cars Going "Out of Game"**
- When owner takes their cars home, how is this handled?
- Does system need to track "borrowed" vs "permanent" rolling stock?
- Can you query "show me only cars that are actually here today"?

**16. Layout Changes Mid-Session**
- Unlikely but: what if a module is removed mid-session?
- What happens to cars that were on that module?
- Can system detect this and flag affected operations?

**17. Scale Operational Differences**
- Do HO/N/O scales affect ROG operations?
- Or is it purely physical handling (smaller cars harder to manipulate)?
- Do different scales have different operational conventions in the community?

**18. Capacity Constraints**
- How does system know track is "full"?
- Is it car count? Physical length? Visual crowding?
- What happens when operator tries to spot a car but track is full?

**19. Session End State**
- What defines a "natural breakpoint"?
- Specific trains completed? All assigned work done? Time limit? Operator exhaustion?
- Can a session "save" mid-train (train halfway through route)?
- What needs to be documented for next operator to continue?

**20. Success Metrics**
- Beyond "work completed," are there other success measures?
- Time efficiency? Number of moves? Errors made?
- Or purely intrinsic satisfaction?

**21. Interchange Between Railroads**
- In modular settings with multiple "railroads" (different clubs?), how does interchange work?
- Is this just staging by another name or different mechanics?
- Do cars need routing info across "foreign" railroads?

#### PLATFORM AND DEPLOYMENT CONTEXT

**22. Technical Platform**
- Where does ROG run? (web browser, native app, tablet-optimized?)
- Online vs offline operation - critical for train shows with unreliable WiFi
- Multiple device sync - can conductor use phone while yardmaster uses tablet?
- Does ROG need to work when internet is completely unavailable?

**23. Integration and Compatibility**
- Can ROG import existing data from JMRI or other systems?
- Does ROG need to export data (for printing, archiving, analysis)?
- Is there value in being compatible with existing standards/formats?

**24. Project Scope and Audience**
- Is ROG for your personal use, your club, or the broader community?
- Open source vs proprietary?
- This affects decisions about generalization vs. solving your specific needs

**25. Physical Artifacts and Backup**
- Is ROG purely digital or are there hybrid scenarios (some paper, some digital)?
- Do operators need printed backup in case devices fail?
- QR codes on modules to identify them to the system?

**26. User Authentication and Authority**
- Who can modify layout configuration?
- Who can check in cars?
- Who can change car positions/states?
- Or is it "trust everyone" since it's a collaborative game?

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
| 2024-12-07 | Phase 1 completed: domain research, user validation, problem crystallization | Phase 1 |
| 2024-12-07 | Added comprehensive Phase 2 question framework (26 categories) | Phase 1 |
| 2024-12-07 | Updated current phase status to Phase 2 (PRD Development) | Phase 1 |
| 2024-12-07 | Platform architecture decisions (Q22-26): PWA, cloud backend, offline sync, freemium model | Phase 2 |
| 2024-12-07 | Established car ownership conflict prevention model and session master authority | Phase 2 |

