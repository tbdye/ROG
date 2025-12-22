# Collaboration Patterns - The ROG Sessions

**Project:** "The ROG Sessions" Portfolio Piece
**Purpose:** Document effective human-AI collaboration patterns observed during ROG development
**Last Updated:** December 21, 2025

---

## How to Use This Document

This captures **observable collaboration patterns** from the ROG project—techniques, approaches, and interaction styles that made the collaboration effective.

**This is portfolio content** - process observations, not technical decisions. Technical rationale lives in `../PRD/decisions-log.md`.

---

## Core Collaboration Patterns

### Pattern: Structured Feedback
- Human uses numbered lists for multi-point feedback
- Separates "definitive" from "to be determined"
- Effective for parallel thread management

**Example:** When reviewing initial guidelines, user provided 6 numbered responses mixing immediate decisions with "we'll come back to this."

---

### Pattern: Meta-Awareness
- Human thinking about the process while doing the process
- Recognition that "this initial framing" is valuable content
- Anticipating future challenges (content synthesis)

**Example:** User recognized initial setup conversation had portfolio value even before work began.

---

### Pattern: Appropriate Deferral
- Tech stack decisions deferred until requirements known
- Scope to be "negotiated" after understanding ROG
- Shows restraint - not jumping to solutions prematurely

**Example:** "I don't have any tech stack preference. I don't want to lean toward any particular framework right now because we don't have a clear idea of what we want to do."

---

### Pattern: Anticipatory Problem-Solving
- Asks "how will this fail?" before it fails
- Probes operational mechanics upfront
- Establishes preventive protocols rather than reactive fixes
- Thinks one step ahead: "how do we work?" before "what do we build?"

**Example:** Asked about token counter visibility, content loss prevention, file sync workflow BEFORE starting actual work.

---

### Pattern: Adaptive Interaction Style
- Collaborative exploration for planning and ideation
- Prescriptive direction for implementation and standards
- Matches interaction mode to task requirements
- Recognizes when to explore vs. when to direct
- Meta-awareness of collaboration approach itself

**Examples:**
- Phase 0 (Planning): "What should we consider?" "How might this fail?"
- Repo setup: "Here are the requirements" "Implement this structure"

---

### Pattern: Systematic Elimination
- Think aloud about options, exploring implications
- Work through friction points of each approach
- Arrive at conclusion through discovery, not declaration
- "Talk myself into/out of" approach reveals trade-offs
- Particularly effective for architectural decisions

**Example:** User explored self-hosted vs cloud-based by systematically identifying friction points:
> "I feel like I'm talking myself out of this approach."

Discovered the right answer through exploration rather than jumping to conclusion.

---

### Pattern: Empirical Assumption Testing
- Demand hands-on verification of critical claims
- "Let me test that myself before we proceed"
- Recognize cascading implications of foundational decisions
- Build on validated foundations, not assertions
- Particularly important for technology capabilities claims

**Example:** When AI described PWA functionality:
> "I need to find an example PWA app and install it on my iPhone to ensure what you're saying is actually reality. This is a pivotal decision and accidentally hallucinating this behavior invalidates other decisions."

---

### Pattern: Technical Insight Refinement
- Accept AI reasoning but refine with superior technical understanding
- Find simpler solutions by seeing core truth
- Example: "Car ownership prevents conflicts" vs complex conflict resolution
- User's domain expertise reshapes AI's generic approach

**Example:** AI proposed complex CRDT-style conflict resolution. User saw simpler truth: car ownership model prevents conflicts by design.

---

### Pattern: Research-First, Then Practical Validation
- Claude conducts comprehensive research
- Human validates against lived experience
- Research blind spots revealed (things research can't tell you)
- Synthesis creates complete picture

**Example:** Domain research provided vocabulary and context. User's experience revealed geographic routing failure and paper waybill pain points.

---

### Pattern: Explicit Assumption Testing
- Human requests challenge to assumptions
- Generate comprehensive uncertainty list
- Assess readiness based on question addressability
- Proceed with documented unknowns

**Example:**
> "Challenge this assumption by thinking deeply about what we do know, and evaluate if we can find gaps in understanding."

Generated 26 categories of uncertainties, validating foundation while revealing scope.

---

### Pattern: Problem Crystallization Through Failure Analysis
- "What did you try before?"
- "Why didn't it work?"
- Failure modes define requirements
- Specific problems better than vague goals

**Example:** Previous implementation failure (no topology awareness) became the core problem statement for ROG.

---

### Pattern: Strategic Checkpointing
- Not "are we done?" but "are we ready for next phase?"
- Balance thoroughness with forward momentum
- Document gaps while acknowledging foundation
- Prevent both premature advancement and analysis paralysis

---

### Pattern: Iterative Model Refinement Through Examples
- Start with concrete examples, not abstract theory
- Each example reveals new complexity
- Corrections lead to general principles
- User validation (hand-editing) reveals UX requirements

**Example:** Topology model validated through Kennedy (crossovers), Pure Oil (ladder), Chesterfield (complex multi-track).

---

### Pattern: Testing Edge Cases Reveals Truth
- Simple examples mask complexity
- Complex examples test understanding
- Impossible configurations reveal gaps
- Domain expertise essential for validation

**Example:** Crossover module revealed AI had both turnouts facing same direction (impossible). User's railroad knowledge caught the error.

---

### Pattern: Learning Through Failure
- User creating YAML by hand revealed model is sound but UX is critical
- 30 minutes + errors = proof visual editor is non-negotiable
- Discovery through doing beats theoretical analysis

**Example:**
> "I spent at least 30 minutes trying to write that Pure-Oil-Module.yaml, in notepad, and still got it wrong. It really sucks doing this by hand."

Immediate realization: visual editor is essential, not nice-to-have.

---

### Pattern: Physical Reality Constrains Model
- Acknowledged human limitations (left/right ambiguity)
- Visual aids solve spatial reasoning problems
- Convention > absolute correctness
- Model must work regardless of how humans describe it

**Example:** User struggled with left/right orientation, provided screen clip to clarify. Labels are arbitrary but necessary for communication.

---

## Session-Specific Observations

### Session 1 (Initial Setup - Phase 0)
**Interaction style:** Collaborative exploration
**Key pattern:** Anticipatory problem-solving
**Notable:** Entire session on "how to work together" before "what to build"

### Session 2 (Repo Structure)
**Interaction style:** Prescriptive direction
**Key pattern:** Adaptive interaction (matched mode to task)
**Notable:** User had clear requirements, provided explicit constraints

### Session 3 (Domain Research - Phase 1)
**Interaction style:** Research-first with practical validation
**Key pattern:** Systematic knowledge building
**Notable:** Three-domain research (real→model→existing systems)

### Session 4 (Platform Architecture - Phase 2)
**Interaction style:** Systematic elimination
**Key pattern:** Empirical testing (PWA validation)
**Notable:** User talked through options, discovered answer through exploration

### Session 5 (Topology Model - Phase 2)
**Interaction style:** Iterative refinement
**Key pattern:** Learning through examples and failure
**Notable:** Hand-editing YAML revealed UX requirements

---

## Meta-Patterns

### Knowledge Building Progression
1. Research establishes vocabulary
2. User validation adds practical reality
3. Examples test understanding
4. Failures reveal UX requirements
5. Edge cases expose model gaps

### Decision-Making Flow
1. Explore options systematically
2. Test critical assumptions empirically
3. Refine with domain expertise
4. Validate through doing
5. Document rationale

### Communication Adaptation
- Numbered lists for parallel topics
- Visual aids for spatial concepts
- Code/data for technical precision
- Narrative for process observations
- Questions for exploring unknowns

---

## Effective Prompting Techniques Observed

### Framing for Collaboration Quality
> "I want to capture how I guide you and how you respond. Seeing raw content isn't necessarily useful, but the process that humans use to effectively get good work from LLMs like yourself is."

Sets expectation for meta-awareness and process documentation.

### Challenging Assumptions
> "Challenge this assumption by thinking deeply about what we do know, and evaluate if we can find gaps in understanding."

Requests explicit assumption testing rather than accepting current state.

### Demanding Empirical Validation
> "I need to find an example PWA app and install it on my iPhone to ensure what you're saying is actually reality."

Recognizes AI can hallucinate capabilities, demands proof for pivotal decisions.

### Systematic Exploration
> "I feel like I'm talking myself out of this approach."

Thinks aloud, explores implications, discovers answer through reasoning process.

### Learning Through Doing
> "I spent at least 30 minutes trying to write that Pure-Oil-Module.yaml, in notepad, and still got it wrong."

Tests theoretical model with hands-on practice, discovers UX requirements.

---

## Anti-Patterns (What NOT to Do)

### Jumping to Solutions
User consistently deferred technical decisions until requirements understood. Avoided premature commitment.

### Accepting AI Output Uncritically
User tested PWA claim, caught token counter UI assumption, validated topology model through examples.

### Ignoring Process for Product
User recognized that process documentation (this project) has value independent of technical outcome.

### Batch Refinement
User provided feedback incrementally, tested assumptions continuously, rather than waiting for "complete" draft.

---

## Questions for Phase 6 (Portfolio Synthesis)

When creating final portfolio piece:

1. **Narrative Arc:** How do we structure the collaboration story?
2. **Key Moments:** Which patterns best illustrate effective collaboration?
3. **Audience Value:** What can viewers/readers apply to their own AI work?
4. **Format:** Which moments work better for video vs blog?
5. **Balance:** How much project context vs collaboration technique?

---

## Tags for Search

`#collaboration-patterns` `#human-ai-interaction` `#structured-feedback` `#meta-awareness` `#appropriate-deferral` `#anticipatory-problem-solving` `#adaptive-interaction` `#systematic-elimination` `#empirical-testing` `#technical-refinement` `#research-validation` `#assumption-testing` `#problem-crystallization` `#strategic-checkpointing` `#iterative-refinement` `#edge-case-testing` `#learning-through-failure` `#effective-prompting`

---

## Related Portfolio Documents

- [README.md](README.md) - Portfolio project overview
- [presentation-artifacts.md](presentation-artifacts.md) - Curated quotes and moments
- [sessions/](sessions/) - Chronological process narratives
