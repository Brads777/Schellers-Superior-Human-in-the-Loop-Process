# ©2026 G. Bradley Scheller / ToolsIQ LLC

# SHIT Loop — Design Specification

**Scheller's Superior Human In The Loop Process**

| Field | Value |
|-------|-------|
| Version | 1.0 |
| Date | March 21, 2026 |
| Status | Draft |
| Author | G. Bradley Scheller / ToolsIQ LLC |
| IP | TM filed, Patent Pending |
| License (rubric skills) | MIT (open-source) |
| License (orchestrator) | Proprietary (ToolsIQ LLC) |

---

## Table of Contents

1. [Framework Vision](#1-framework-vision)
2. [Pipeline Architecture](#2-pipeline-architecture)
3. [Rubric Schema](#3-rubric-schema)
4. [Interview Engine](#4-interview-engine)
5. [Guardrails](#5-guardrails)
6. [Evaluation Engine](#6-evaluation-engine)
7. [Research Pipeline](#7-research-pipeline)
8. [Orchestrator Agent](#8-orchestrator-agent)
9. [Rubric Skill Format](#9-rubric-skill-format)
10. [Consumer Integration Patterns](#10-consumer-integration-patterns)
11. [Open Source Strategy](#11-open-source-strategy)
12. [Build Sequence](#12-build-sequence)
13. [IP & Legal](#13-ip--legal)
14. [Open Questions](#14-open-questions)

---

## 1. Framework Vision

### The Problem with Generic Evaluation

Most AI-driven evaluation falls into one of two traps:

1. **The "rate 1-5" trap.** Vague ordinal scales with no specific criteria. What separates a 3 from a 4? Nobody knows. The rating is meaningless, unreproducible, and useless for improvement.

2. **The "one rubric fits all" trap.** A fixed evaluation template applied across wildly different output types. A commercial video and a landing page have fundamentally different success criteria, yet they get measured with the same generic checklist.

SHIT Loop rejects both. It creates **bespoke evaluation instruments** — rubrics with specific, research-backed criteria, explicit weight assignments, and concrete level definitions that tell you exactly what a 0, 1, 2, 3, or 4 looks like for *that* criterion in *that* domain.

### The 96% Quality Bar

The pass threshold is deliberately harsh. A 96% score requires near-excellence across every weighted criterion.

- **At 80%, mediocrity passes.** You can score 3/4 on everything and still pass. That's "above average," not excellent.
- **At 90%, weaknesses hide.** A single criterion at 2/4 can be compensated by others. Structural flaws survive.
- **At 96%, every criterion matters.** You cannot pass with any criterion at 2/4 unless compensated by multiple perfect scores on high-weight criteria. The math forces near-excellence everywhere.

The 96% bar is not about perfection — it is about making weaknesses mathematically impossible to ignore. When something fails, the improvement plan tells you exactly which criteria are dragging the score down, what level you're at, what the next level looks like, and what research says about how to get there.

#### Mathematical Illustration

For a rubric with 7 criteria all weighted at 3:

| All criteria at rating: | Percentage | Result |
|------------------------|------------|--------|
| All 4s | 100% | PASS |
| All 4s, one 3 | 96.4% | PASS |
| All 4s, two 3s | 92.9% | FAIL |
| All 3s | 75% | FAIL |

With non-uniform weights, a single 3 on a weight-5 criterion costs more than a 3 on a weight-1 criterion. This is intentional — the weight system ensures the most important criteria have the most scoring leverage.

### Framework-Level Positioning

SHIT Loop is not a product. It is an **engine** consumed by every product in the ToolsIQ ecosystem:

| Product | Rubric(s) | Use Case |
|---------|-----------|----------|
| DemoIQ | shit-commercial-development, shit-landing-page, shit-carousel-content | Quality gate before publishing demos |
| VoicesIQ | shit-voice-ai | Call quality scoring, agent persona evaluation |
| CasesIQ | shit-case-study (future) | Case study content quality |
| ExamIQ | shit-clinical-exam (future) | Exam content quality, clinical recommendation audit |
| IntelIQ | shit-inteliq-triage | Knowledge asset curation and triage |
| ContentHubIQ | shit-content-curation (future) | Content quality scoring for marketplace |

Individual rubric skills are **open-source**. The orchestrator agent is **proprietary**. This separation allows the community to contribute rubrics while ToolsIQ retains the interview/research/evaluation engine.

---

## 2. Pipeline Architecture

### 5-Step Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│                        SHIT Loop Pipeline                           │
│                                                                     │
│  ┌──────────┐   ┌──────────┐   ┌───────────┐   ┌───────────┐      │
│  │  Step 0   │──▷│  Step 1   │──▷│  Step 2    │──▷│  Step 3    │     │
│  │  Intent   │   │ Research  │   │ Synthesis  │   │ Interview  │     │
│  └──────────┘   └──────────┘   └───────────┘   └─────┬─────┘      │
│                                                        │            │
│                                                        ▽            │
│                                              ┌───────────────┐      │
│                                              │  Rubric Skill  │     │
│                                              │ (stored YAML)  │     │
│                                              └───────┬───────┘      │
│                                                        │            │
│                                  ┌─────────────────────▽──────┐     │
│                                  │         Step 4             │     │
│                                  │    Evaluation Loop         │     │
│                                  │                            │     │
│                                  │  Artifact + Rubric → Score │     │
│                                  │  ≥96% → PASS              │     │
│                                  │  <96% → Improvement Plan   │     │
│                                  │         → Revise → Rescore │     │
│                                  └────────────────────────────┘     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Data Flow Between Steps

| Step | Input | Process | Output |
|------|-------|---------|--------|
| **0. Intent** | User describes what to evaluate | Structured interview: what, who, why, exemplars | Intent document (YAML) |
| **1. Research** | Intent document | Gemini Deep Research + Perplexity → NotebookLM | Research corpus: synthesized report, source URLs, key findings |
| **2. Synthesis** | Research corpus + intent | Opus ↔ NotebookLM interaction: draft criteria from research | Draft criteria list with proposed weights + level definitions |
| **3. Interview** | Draft criteria + human | AI presents criteria, human refines: add/remove/reweight/redefine levels | Final rubric document (stored as a skill) |
| **4. Evaluate** | Artifact + rubric skill | Per-criterion scoring with evidence citation | Score report + PASS/FAIL + improvement plan (if FAIL) |

### Two Entry Points

**Entry A — New Rubric Creation (Steps 0-4):**
Full pipeline. Use when no existing rubric covers the domain. Produces a new rubric skill, then evaluates.

```
/shit-loop new "commercial video for dental AI product"
→ Intent → Research → Synthesis → Interview → Evaluate
```

**Entry B — Existing Rubric Evaluation (Step 4 only):**
Jump directly to evaluation. Use when a rubric skill already exists.

```
/shit-loop evaluate --rubric shit-commercial-development ./output/video.mp4
→ Evaluate (using existing rubric)
```

---

## 3. Rubric Schema

### YAML Frontmatter Definition

Every rubric is stored as a Markdown file with YAML frontmatter and structured body. The reference implementation is `skills/shit-inteliq-triage.md`.

```yaml
---
name: shit-{domain}                    # Unique identifier, kebab-case, shit- prefix
description: >                          # Multi-line — used for skill discovery
  SHIT Loop rubric for {domain}.
  Evaluates {output type} against {N} weighted criteria
  (0-4 scale). {Threshold behavior}. Criteria: {list}.
version: "1.0.0"                       # Semantic versioning
created: "2026-03-21"                  # ISO 8601
updated: "2026-03-21"                  # Last modification
author: "Brad Scheller"               # Rubric author
domain: "{domain name}"               # Human-readable domain
output_type: "{what is being scored}"  # e.g., "video commercial", "landing page"
threshold_mode: "pass-fail"            # "pass-fail" (96% gate) or "triage" (tiered routing)
pass_threshold: 96                     # For pass-fail mode
criteria_count: 7                      # Number of criteria
max_possible: 108                      # Pre-calculated: sum(4 × Weight_i)
tags:
  - quality-gate
  - commercial
  - video
---
```

### Threshold Modes

**pass-fail** (default): Binary. Score ≥ `pass_threshold` = PASS. Score < threshold = FAIL + improvement plan.

**triage**: Tiered routing based on score ranges. Used for intake/curation pipelines:

```yaml
threshold_mode: triage
triage_thresholds:
  - range: [9.0, 10.0]
    action: auto-keep
    description: "Immediately archive. No human review needed."
  - range: [7.0, 8.9]
    action: surface
    description: "Present for human review."
  - range: [3.0, 6.9]
    action: tag
    description: "Store metadata only."
  - range: [0.0, 2.9]
    action: discard
    description: "Delete after staging TTL."
```

### Criteria Body Structure

Each criterion in the rubric body:

```yaml
criteria:
  - id: 1
    name: "Criterion Name"
    description: "One-sentence: what this criterion measures."
    weight: 5                           # 1-5, importance multiplier
    levels:
      0:
        label: "Unacceptable"           # or domain-specific (e.g., "Irrelevant")
        description: >
          Specific, concrete description of what a 0 looks like
          for THIS criterion. References observable behaviors,
          qualities, or outcomes. NOT generic.
      1:
        label: "Below Average"
        description: >
          Specific description. Acknowledges partial effort but
          identifies what is missing or insufficient.
      2:
        label: "Average"
        description: >
          Meets basic expectations. Identifies what would
          elevate to the next level.
      3:
        label: "Above Average"
        description: >
          Clearly strong with identified qualities that
          distinguish from average.
      4:
        label: "Excellence"
        description: >
          Aspirational target that justifies a perfect score
          on this criterion. Specific and observable.
```

### Critical Constraint: Level Specificity

Level definitions MUST be **specific to the criterion**, never generic quality language.

**WRONG:**
```
4: "Excellent quality, exceeds expectations"
3: "Good quality, meets expectations"
```

**CORRECT:**
```
4: "Provides a complete process decomposition framework — maps the full
    workflow, identifies dependencies between tasks, and enables targeted
    improvement at each stage."
3: "Clearly decomposes a complex process into discrete, named tasks.
    Each task is independently addressable and improvable."
```

The specificity of level definitions is what makes SHIT Loop evaluations **reproducible**. Two independent evaluators reading the same level definitions should arrive at the same rating (within 1 level) for the same artifact.

### Guardrails Schema

Guardrails define things that **must not** appear in the output. They operate outside the positive scoring model — violations result in deductions or automatic failure regardless of how well the positive criteria score.

**Two tiers:**

```yaml
guardrails:
  hard:   # Any match = AUTO-FAIL regardless of score
    - id: g1
      name: "HIPAA Violation"
      description: "Contains PHI, patient names, or identifiable health data"
    - id: g2
      name: "Copyright Infringement"
      description: "Uses unlicensed copyrighted material (music, footage, images)"
    - id: g3
      name: "False Medical Claims"
      description: "Makes specific clinical promises or guarantees treatment outcomes"

  soft:   # Each match deducts a fixed percentage from the score
    - id: g4
      name: "Stock Footage Substitution"
      penalty: 3   # deducts 3% from total percentage
      description: "Uses stock footage where real product footage was available"
    - id: g5
      name: "Missing Captions"
      penalty: 3
      description: "No captions — fails accessibility and silent autoplay on social"
    - id: g6
      name: "Placeholder Content"
      penalty: 5
      description: "Contains lorem ipsum, TODO markers, or placeholder text/images"
    - id: g7
      name: "Competitor Brand Visible"
      penalty: 4
      description: "Shows competitor logos, UI, or identifiable branding"
```

**Hard guardrails** cannot be compensated. A HIPAA violation in an otherwise perfect commercial is still a fail. This is the right behavior — certain violations are categorically disqualifying.

**Soft guardrails** apply percentage penalties that stack. An output that would have scored 98% but triggers two 3% soft guardrails drops to 92% — FAIL.

### Scoring Formula (with Guardrails)

```
For a rubric with N criteria:

  Rating_i  = score assigned to criterion i (integer, 0-4)
  Weight_i  = weight of criterion i (integer, 1-5)

  Total Score     = Σ(Rating_i × Weight_i) for i = 1..N
  Total Possible  = Σ(4 × Weight_i) for i = 1..N
  Percentage      = (Total Score / Total Possible) × 100
```

**Pass/fail mode (with guardrails):**
```
IF any hard guardrail triggered:
    result = AUTO-FAIL (overrides score, cites specific guardrail)
ELSE:
    Soft Penalty     = Σ(penalty_i) for each triggered soft guardrail
    Final Percentage = Base Percentage - Soft Penalty
    IF Final Percentage ≥ 96:  result = PASS
    ELSE:                      result = FAIL → generate_improvement_plan()
```

The improvement plan for guardrail-triggered failures lists guardrail violations FIRST (before criteria gaps), since guardrail fixes are usually binary (remove the offending element) rather than iterative.

**Triage mode:**
```
Normalized Score = (Total Score / Total Possible) × 10   # 0-10 scale
Match against triage_thresholds (descending) → action
```

---

## 4. Interview Engine

The Interview Engine transforms a vague evaluation need into a precise, research-backed evaluation instrument through structured conversation.

### Phase 4.1: Intent Gathering

Questions asked one at a time:

1. **What are you evaluating?** — output type, specific artifact, domain context
2. **Who is the audience?** — primary viewer, decision-maker, platform
3. **What does "great" look like?** — exemplars, anti-exemplars, brand guidelines
4. **What is the business goal?** — conversion, awareness, education; success metrics; constraints
5. **Have you evaluated this before?** — existing checklists, past failures, criteria that matter most

**Output format:**
```yaml
intent:
  output_type: "30-second commercial video"
  domain: "dental AI SaaS product launch"
  audience:
    primary: "dental practice owners, 35-60 years old"
    platform: "Instagram Reels, TikTok, YouTube Shorts"
  quality_goals:
    - "Stop the scroll in first 2 seconds"
    - "Convey the core value prop in 15 seconds"
    - "Drive demo sign-up clicks"
  exemplars:
    - url: "https://example.com/great-saas-ad"
      notes: "Love the hook and pacing"
  anti_exemplars:
    - url: "https://example.com/bad-example"
      notes: "Too corporate, no hook"
  existing_criteria:
    - "Must include product demo footage"
    - "Brand colors must be visible"
  constraints:
    - "30 seconds max for social, 60 seconds for YouTube"
    - "Must work without sound (captions required)"
```

### Phase 4.2: Research Integration

The intent document is passed to the Research Pipeline (Section 6). Research returns:
- Synthesized best practices for the output type
- Industry benchmarks and standards
- Platform-specific requirements
- Common failure modes

The Interview Engine uses findings to suggest criteria the human may not have considered:

> "Based on research into high-performing social media ads, accessibility compliance (captions, color contrast) significantly impacts engagement metrics. Would you like to add an Accessibility criterion?"

### Phase 4.3: Criteria Co-Creation

**Process:**
1. AI proposes initial criteria list from: intent analysis + research findings + domain knowledge from existing rubric skills
2. Human reviews each criterion — accept, modify, reject, add new
3. For each accepted criterion, AI proposes a description and explains why it matters (citing research)

### Phase 4.4: Weight Assignment

For each accepted criterion, a guided weight assignment process:

```
AI: "Let's assign weights. Weight 5 = absolutely critical to success,
     weight 1 = nice-to-have quality signal.

     I'd suggest 'Narrative Hook' at weight 5. Research shows the first
     1.7 seconds determine 73% of ad engagement. Without a hook,
     nothing else matters.

     What weight would you assign? (1-5)"
```

**Weight calibration guide** presented to the human:

| Weight | Meaning | Heuristic |
|--------|---------|-----------|
| 5 | **Critical** — output fails without this | "If this criterion scores 0, is the output useless?" → Yes = 5 |
| 4 | **Important** — major quality driver | "Would a viewer/user notice if this was weak?" → Yes = 4 |
| 3 | **Standard** — expected professional quality | Default weight when nothing special applies |
| 2 | **Contributing** — adds polish but not structural | "Does this differentiate good from great, not bad from good?" → Yes = 2 |
| 1 | **Signal** — nice-to-have, tiebreaker quality | "Would you still ship if this was missing?" → Yes = 1 |

### Phase 4.5: Level Definition Authoring

For each criterion, AI drafts the 5-level definitions (0-4) based on research, then the human refines:

```
AI: "For 'Narrative Hook' (weight 5), here are my proposed levels:

     4 — Excellence: Opens with a pattern-interrupt that stops the scroll
         within 1.5 seconds. Emotional stakes are established immediately.
         The viewer feels compelled to watch the next 5 seconds. Uses
         proven hook archetypes: surprising statistic, provocative question,
         visual disruption, or identity statement.

     3 — Above Average: Strong opening that captures attention within
         2-3 seconds. Clear emotional or intellectual hook present.
         Viewer is curious but not compelled.

     2 — Average: Has an identifiable opening moment but doesn't
         differentiate from typical content. Could be any SaaS ad.
         Viewer might keep watching but isn't engaged.

     1 — Below Average: Starts with logo, company name, or generic
         establishing shot. No hook — relies on viewer's existing
         interest. Would lose 80%+ of social media viewers.

     0 — Unacceptable: No discernible opening strategy. Starts with
         irrelevant footage, silent black screen, or confusing imagery.
         100% scroll-past rate.

     Do these match your expectations? Would you modify any level?"
```

### Phase 4.6: Guardrail Definition

After positive criteria are defined, the Interview Engine asks about guardrails:

```
AI: "Now let's define guardrails — things that must NOT appear in the
     output. There are two types:

     Hard guardrails: Violations that automatically fail the output
     regardless of score. These are non-negotiable.

     Soft guardrails: Things that shouldn't be there — each one deducts
     a fixed percentage from the score.

     Based on the domain (dental AI product commercial), I'd suggest
     these hard guardrails:
     - HIPAA violation (patient data visible)
     - Copyright infringement (unlicensed music/footage)
     - False medical claims (guaranteed outcomes)

     And these soft guardrails:
     - Stock footage when real product footage exists (-3%)
     - Missing captions (-3%)
     - Competitor branding visible (-4%)

     What would you add, remove, or modify?"
```

**Output:** Complete rubric document is saved as a reusable skill file (see Section 9).

---

## 5. Guardrails

### Design Rationale

Positive criteria measure the **presence of quality**. But an output can score well on every positive criterion and still be unusable if it contains disqualifying flaws. A commercial with a perfect narrative hook, beautiful production quality, and compelling CTA is worthless if it contains a HIPAA violation.

Guardrails measure the **absence of harm**. They are a separate evaluation axis that cannot be compensated for by positive quality.

### Tier 1: Hard Guardrails (Auto-Fail)

Hard guardrails represent **categorical disqualifiers** — violations so severe that no amount of positive quality can compensate. They are binary: present or absent.

**Characteristics:**
- Triggering ANY hard guardrail = AUTO-FAIL regardless of score
- Cannot be overridden by high positive scores
- Evaluated BEFORE positive criteria (fail-fast)
- The improvement plan cites the specific guardrail and what must be removed/changed
- Re-evaluation is blocked until the guardrail violation is explicitly resolved

**Common hard guardrails by domain:**

| Domain | Hard Guardrails |
|--------|----------------|
| Healthcare/Dental | HIPAA violation, false medical claims, unlicensed patient imagery |
| Commercial video | Copyright infringement, competitor trademarks, defamatory content |
| Landing page | Misleading pricing, fake testimonials, broken critical CTAs |
| Voice AI | Recording without consent, impersonating real individuals, sharing PHI |
| Educational content | Plagiarism, factual inaccuracies presented as fact |

### Tier 2: Soft Guardrails (Penalty Deductions)

Soft guardrails represent **quality detractors** — things that shouldn't be present but aren't categorically disqualifying. Each triggered soft guardrail deducts a fixed percentage from the total score.

**Characteristics:**
- Each soft guardrail has a `penalty` value (integer, 1-10, representing % deducted)
- Penalties stack — multiple soft guardrail violations compound
- An output at 99% base score with 4% in soft guardrail penalties scores 95% → FAIL
- The improvement plan lists soft guardrail violations with their penalties
- Fixing soft guardrails is usually binary (remove the offending element)

**Penalty calibration guide:**

| Penalty | Severity | Example |
|---------|----------|---------|
| 1-2% | Minor — detectable flaw, doesn't undermine the output | Slight brand color inconsistency, non-optimal aspect ratio |
| 3-4% | Moderate — noticeable quality issue, should be fixed | Stock footage substitution, missing captions, audio level inconsistency |
| 5-7% | Significant — undermines credibility or professionalism | Placeholder content, visible debug/test artifacts, broken layout elements |
| 8-10% | Severe — nearly disqualifying but theoretically fixable | Wrong product shown, misleading but not false claims, major accessibility failure |

### Guardrails in the Score Report

The evaluation report presents guardrails separately from criteria:

```
═══════════════════════════════════════════════════
  SHIT Loop Score Report — shit-commercial-development
═══════════════════════════════════════════════════

  GUARDRAIL CHECK
  ─────────────────────────────────────
  Hard Guardrails:
    ✓ HIPAA Violation .................. CLEAR
    ✓ Copyright Infringement ........... CLEAR
    ✓ False Medical Claims ............. CLEAR

  Soft Guardrails:
    ✗ Stock Footage Substitution ....... TRIGGERED (-3%)
      Evidence: Segments 5-8 use generic stock footage
      of a dental office instead of actual VoicesIQ UI
    ✓ Missing Captions ................. CLEAR
    ✓ Competitor Brand Visible ......... CLEAR

  Soft Penalty Total: -3%
  ─────────────────────────────────────

  CRITERIA SCORING
  ─────────────────────────────────────
  # │ Criterion          │ Wt │ Rating │ Weighted │ Evidence
  1 │ Narrative Hook     │  5 │   4    │   20     │ Strong pattern-interrupt...
  2 │ Value Proposition  │  5 │   4    │   20     │ Clear dental AI pitch...
  ...
  ─────────────────────────────────────

  Base Percentage:  97.2%
  Soft Penalty:     -3.0%
  Final Percentage: 94.2%

  Result: ✗ FAIL (threshold: 96%)

  IMPROVEMENT PLAN (priority order):
  1. [GUARDRAIL] Stock Footage Substitution (-3%)
     → Replace segments 5-8 with real VoicesIQ product
       screenshots or screen recordings. This alone would
       bring the score to 97.2% → PASS.
  2. [CRITERION] Production Quality (rating 3 → target 4)
     → Audio levels inconsistent between segments...
═══════════════════════════════════════════════════
```

Guardrail fixes are listed FIRST in the improvement plan because they're usually the highest-impact changes (binary fix → immediate score recovery).

---

## 6. Evaluation Engine

### Input

The evaluation engine accepts:
1. **Artifact** — the thing being evaluated (file path, URL, or inline content)
2. **Rubric skill** — the rubric to evaluate against (loaded by name)

### Evaluation Process

**Step 1: Guardrail Scan (fail-fast)**
- Check each hard guardrail. If any triggered → AUTO-FAIL immediately with evidence.
- Check each soft guardrail. Record triggered violations and their penalties.
- This step runs BEFORE positive criteria to avoid wasting evaluation effort on disqualified outputs.

**Step 2: Per-Criterion Scoring**
For each criterion in the rubric:
1. Read all 5 level definitions (0-4)
2. Examine the artifact for evidence relevant to this criterion
3. Determine which level best matches — cite specific evidence
4. Assign rating (0-4)
5. Record: `{criterion, rating, weight, weighted_score, evidence}`

**Step 3: Score Calculation**
```
Base Percentage  = (Σ weighted_scores / max_possible) × 100
Soft Penalty     = Σ penalties from triggered soft guardrails
Final Percentage = Base Percentage - Soft Penalty
```

**Step 4: Result Determination**
```
IF any hard guardrail triggered:     AUTO-FAIL
ELIF Final Percentage ≥ 96:          PASS
ELSE:                                FAIL
```

**Step 5: Improvement Plan (if FAIL)**
Generate a prioritized improvement plan:
1. **Priority metric** = `impact_score` — how much fixing this item would improve the final percentage
   - For guardrails: `impact_score = penalty` (exact % recovery)
   - For criteria: `impact_score = weight × (4 - current_rating)` — proportional to how much room for improvement on high-weight criteria
2. Sort all items by `impact_score` descending
3. For each item: what's wrong (evidence), what "good" looks like (target level definition), what research says, estimated score impact

### Re-Evaluation Protocol

When revised work is submitted:
- Guardrails are re-checked from scratch (violations may have been introduced in revision)
- Only **changed criteria** are re-scored (stable criteria carry forward)
- Score report shows delta: `Previous: 94.2% → Current: 97.8% (+3.6%)`
- Evaluation history is preserved for audit trail

---

## 7. Research Pipeline

### Three-Source Architecture

```
┌─────────────┐    ┌─────────────┐
│   Gemini     │    │  Perplexity  │
│ Deep Research│    │  (targeted)  │
│  (broad)     │    │              │
└──────┬──────┘    └──────┬──────┘
       │                   │
       ▽                   ▽
┌─────────────────────────────────┐
│         NotebookLM              │
│  (synthesis hub, source mgmt)   │
└────────────┬────────────────────┘
             │
             ▽
┌─────────────────────────────────┐
│    Opus 4.6 ↔ NotebookLM       │
│    Draft criteria from research │
└─────────────────────────────────┘
```

**1. Gemini Deep Research** — broad landscape synthesis
- Query: "What are best practices, industry standards, and academic research on evaluating {output_type} in {domain}?"
- Output: Synthesized report (5-10 pages) + source URLs

**2. Perplexity** — targeted current-state queries (3-5 focused questions)
- Covers: current platform algorithms, recent benchmarks, competitor analysis, accessibility standards

**3. NotebookLM** — synthesis hub
- Load: Gemini report + Perplexity answers + exemplar URLs from intent
- Query for: recurring themes, conflicting advice, quantifiable benchmarks

**4. Opus ↔ NotebookLM** — criteria drafting
- Opus drafts criteria from NotebookLM synthesis with level definitions grounded in research

Research is optional. Skip with `--skip-research` for experienced users with strong domain expertise.

---

## 8. Orchestrator Agent (/shit-loop)

### Command Interface

```bash
/shit-loop new "description"           # Full pipeline: Intent → Research → Interview → Evaluate
/shit-loop new --skip-research "desc"  # Skip research phase
/shit-loop evaluate --rubric <name> <artifact>  # Evaluate against existing rubric
/shit-loop re-evaluate --session <id> <artifact> # Re-evaluate revised work
/shit-loop list                        # List available rubric skills
/shit-loop inspect <rubric-name>       # Show criteria, weights, guardrails
```

### State Management

| Data | Location |
|------|----------|
| Rubric skills | `E:/SHIT-Loop/skills/shit-{domain}.md` |
| Evaluation sessions | `E:/SHIT-Loop/sessions/{session-id}/` |
| Research corpus | `E:/SHIT-Loop/research/{session-id}/` |
| Intent documents | `E:/SHIT-Loop/sessions/{session-id}/intent.yaml` |

### Sub-Agent Delegation

| Sub-Agent | Model | Purpose |
|-----------|-------|---------|
| Research Agent | Gemini 3 Flash | Broad research synthesis |
| Perplexity Agent | Perplexity API | Targeted current-state queries |
| Interview Agent | Opus 4.6 | Human interview, criteria co-creation |
| Evaluation Agent | Opus 4.6 | Per-criterion scoring with evidence |
| Report Agent | Sonnet 4.6 | Score report generation |

---

## 9. Rubric Skill Format

### File Naming

`shit-{domain}.md` — examples: `shit-commercial-development.md`, `shit-landing-page.md`, `shit-voice-ai.md`

### File Structure

```markdown
---
(YAML frontmatter — see Section 3)
---

# ©2026 Brad Scheller

# SHIT Loop Rubric — {Domain Title}

## Metadata
## Purpose
## Prerequisites
## Guardrails (Hard + Soft tables)
## Scoring Formula
## Evaluation Matrix (full criterion × level table)
## Evaluation Process (step-by-step)
## Examples (scored high + low)
## Integration (consumer product pipeline)
## Notes (domain-specific caveats)
```

A blank template at `E:/SHIT-Loop/skills/_template.md` is auto-populated by the Interview Engine.

---

## 10. Consumer Integration Patterns

### How Products Call SHIT Loop

| Pattern | Example | Use Case |
|---------|---------|----------|
| **Direct invocation** | `/shit-loop evaluate --rubric shit-commercial-development ./output.mp4` | Manual evaluation |
| **Orchestrator delegation** | `/delegate evaluator "/shit-loop evaluate ..."` | Within unified-orchestrator |
| **Pipeline quality gate** | Stitch → evaluate → if PASS, export; if FAIL, revise | Automated pipeline (DemoIQ) |

### Product-Specific Integration

| Product | Rubric | Trigger Point | On FAIL |
|---------|--------|---------------|---------|
| DemoIQ | shit-commercial-development | After stitcher, before export | Revise storyboard, re-stitch |
| DemoIQ | shit-landing-page | After page generation | Edit page, regenerate |
| VoicesIQ | shit-voice-ai | Post-call QA (agent #19) | Flag for review, adjust prompts |
| IntelIQ | shit-inteliq-triage | On knowledge asset ingestion | Route per triage thresholds |
| Autopilot | Any rubric | Before marking task complete | Block completion, loop until 96% |

---

## 11. Open Source Strategy

### What's Open vs. Closed

| Component | License | Rationale |
|-----------|---------|-----------|
| Rubric skills (`skills/`) | MIT | Community contribution, wide adoption |
| Scoring formula & schema | MIT (in README) | Standard, reproducible |
| Orchestrator (`/shit-loop`) | Proprietary | Core IP — interview, research, evaluation engines |
| Research pipeline | Proprietary | NotebookLM/Gemini/Perplexity orchestration |

### Community Contribution

1. Fork → create `shit-{domain}.md` from `_template.md`
2. Define criteria, weights, levels, guardrails
3. Include ≥2 scored examples (1 high, 1 low)
4. Submit PR → Brad reviews for level specificity + weight rationale
5. Merge → available to all users

---

## 12. Build Sequence

| Phase | Scope | Exit Criteria |
|-------|-------|---------------|
| **1. Foundation** | Rubric schema + evaluation engine + `shit-commercial-development` | Can load a rubric, score an artifact, produce a formatted report |
| **2. Interview Engine** | Intent gathering, criteria co-creation, weight assignment, level authoring, guardrail definition | Can create a new rubric from scratch through conversation |
| **3. Research Pipeline** | Gemini + Perplexity + NotebookLM integration | Research findings feed into interview as suggested criteria |
| **4. Orchestrator** | `/shit-loop` commands, session state, sub-agent delegation | Full `new` and `evaluate` workflows work end-to-end |
| **5. First Rubric Skills** | shit-commercial-development, shit-landing-page, shit-strategic-presentation | 3 production rubrics, each tested against real artifacts |

---

## 13. IP & Legal

- **©2026 G. Bradley Scheller / ToolsIQ LLC. All rights reserved.**
- **SHIT Loop™** and **Scheller's Superior Human In The Loop Process™** are trademarks of ToolsIQ LLC.
- **Patent Pending** — U.S. Provisional Patent Application covering:
  - The 5-step pipeline (Intent → Research → Synthesis → Interview → Evaluate)
  - Research-backed rubric co-creation methodology
  - Two-tier guardrail system (hard auto-fail + soft penalty deductions)
  - 96% threshold quality gate with weighted, level-specific evaluation
- **Open source (MIT)**: Individual rubric skill files, scoring formula, schema documentation
- **Proprietary**: Orchestrator agent, interview engine, research pipeline, evaluation engine

---

## 14. Open Questions

### Schema
- Should soft guardrail penalties be capped (e.g., max 20% total deduction)?
- Should rubric skills support versioning with backward compatibility?
- Integer vs. float penalties for soft guardrails?

### Research Pipeline
- Which NotebookLM MCP server to use? (Plugin built, no server installed yet)
- Can Gemini Deep Research be called programmatically?
- Pre-defined vs. dynamically generated Perplexity queries per domain?

### Evaluation
- For video/audio artifacts, should evidence cite timestamps?
- Should re-evaluation be limited to N iterations to prevent infinite loops?
- Git-tracked evaluation history for audit trail?

### Business
- MIT vs. Apache 2.0 for rubric skills?
- Rubric marketplace launch timing?
- Pricing for premium research-backed rubric skills?

### Integration
- Is SHIT Loop the implementation beneath Autopilot's rubric evaluation?
- Should VoicesIQ Call QA agent (#19) migrate to a SHIT Loop rubric?
- Should guardrails be shareable across rubrics (e.g., HIPAA guardrail used by all healthcare rubrics)?