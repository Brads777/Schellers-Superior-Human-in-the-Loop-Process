---
name: shit-loop-evaluate
description: >
  SHIT Loop Evaluation Engine skill. Scores any artifact against a named
  rubric skill. Handles both threshold modes (pass-fail at 96%, triage
  with tiered routing). Performs fail-fast guardrail scanning, per-criterion
  scoring with evidence citation, score calculation with soft penalty
  deductions, result determination, and improvement plan generation.
  Supports first evaluation and re-evaluation of revised work.
  Commands: /shit-loop evaluate, /shit-loop re-evaluate, /shit-loop list,
  /shit-loop inspect.
version: "1.0.0"
created: "2026-03-21"
updated: "2026-03-21"
author: "Brad Scheller"
domain: "Evaluation Engine"
type: "engine"
tags:
  - evaluation
  - scoring
  - quality-gate
  - triage
  - engine
  - shit-loop
---

# ©2026 Brad Scheller

# SHIT Loop — Evaluation Engine

## Purpose

This skill is the **Step 4: Evaluation Loop** component of the SHIT Loop framework. It accepts an artifact and a rubric skill name, then executes the full evaluation pipeline:

1. Load and validate the rubric skill
2. Scan guardrails (fail-fast on hard guardrail violations)
3. Score each criterion against the artifact with evidence
4. Calculate the final score (base percentage minus soft guardrail penalties)
5. Determine PASS/FAIL (pass-fail mode) or routing action (triage mode)
6. Generate a prioritized improvement plan (if FAIL)
7. Output a formatted score report

This is the **proprietary engine** of the SHIT Loop framework. Individual rubric skills (MIT-licensed) define WHAT to evaluate. This engine defines HOW to evaluate.

## Commands

```
/shit-loop evaluate --rubric <rubric-name> <artifact>
/shit-loop re-evaluate --session <session-id> <artifact>
/shit-loop list
/shit-loop inspect <rubric-name>
```

## Prerequisites

- A rubric skill file exists at `E:/SHIT-Loop/skills/shit-{name}.md`
- An artifact to evaluate: file path, URL, storyboard document, or inline content
- For re-evaluation: a previous session ID from a prior evaluation

## Rubric Loading

### Step 1: Locate the Rubric

Given `--rubric <name>`, resolve the rubric file:

```
Rubric name: "shit-commercial-development"
File path:   E:/SHIT-Loop/skills/shit-commercial-development.md
```

If the name does not start with `shit-`, prepend it:
```
--rubric commercial-development  →  shit-commercial-development
--rubric shit-landing-page       →  shit-landing-page (already prefixed)
```

If the file does not exist, list available rubrics and abort:
```
ERROR: Rubric "shit-landing-page" not found.

Available rubrics:
  - shit-commercial-development (Commercial Video Development, pass-fail 96%)
  - shit-inteliq-triage (IntelIQ Knowledge Triage, triage mode)

Use /shit-loop list for details.
```

### Step 2: Parse the Rubric

Extract from the rubric file:

| Field | Source | Required |
|-------|--------|----------|
| `name` | YAML frontmatter | Yes |
| `threshold_mode` | YAML frontmatter | Yes |
| `pass_threshold` | YAML frontmatter | Yes (for pass-fail mode) |
| `triage_thresholds` | YAML frontmatter | Yes (for triage mode) |
| `criteria_count` | YAML frontmatter | Yes |
| `max_possible` | YAML frontmatter | Yes |
| `domain` | YAML frontmatter | Yes |
| `output_type` | YAML frontmatter | Yes |
| Hard guardrails | Guardrails section | If present |
| Soft guardrails | Guardrails section | If present |
| Criteria + level definitions | Evaluation Matrix section | Yes |

### Step 3: Validate the Rubric

Before proceeding, verify:

1. **Criteria count matches:** The number of criteria in the Evaluation Matrix equals `criteria_count` in frontmatter.
2. **Max possible is correct:** Recalculate `sum(4 x Weight_i)` and verify it matches `max_possible`. If mismatch, warn and use the recalculated value.
3. **Every criterion has all 5 levels (0-4) defined.** Abort if any level definition is missing.
4. **Every criterion has a weight between 1 and 5.** Abort if out of range.
5. **Soft guardrail penalties are between 1 and 10.** Warn if out of range.

Validation failures produce a clear error:
```
RUBRIC VALIDATION ERROR: shit-commercial-development
  - max_possible in frontmatter (160) does not match calculated (164).
    Weights: [5,5,4,4,5,3,4,3,3,5] → sum=41 → 4×41=164.
    Using calculated value: 164.
```

## Artifact Analysis

### Supported Artifact Types

| Type | How Analyzed | Evidence Format |
|------|-------------|-----------------|
| **Video file** (mp4, mov, webm) | Describe visual content, audio, pacing, text overlays, transitions frame-by-frame or segment-by-segment | Timestamps: `[0:03-0:07]` |
| **Image/screenshot** (png, jpg, svg) | Analyze visual composition, text content, layout, branding | Regions: `top-left quadrant`, `hero section` |
| **Web page** (URL or HTML file) | Analyze content, structure, design, interactions, performance | Sections: `hero`, `pricing table`, `footer CTA` |
| **Document** (md, pdf, docx) | Analyze content, structure, argument, evidence, clarity | Sections/pages: `Section 3`, `Page 7, para 2` |
| **Storyboard/script** (pre-production) | Analyze planned approach against criteria; score intent + plan quality | Scenes/beats: `Scene 3`, `Beat 5` |
| **Audio** (mp3, wav) | Analyze narration, music, sound design, pacing, clarity | Timestamps: `[1:22-1:35]` |
| **Code/config** (any source file) | Analyze architecture, patterns, quality, documentation | Files/functions: `src/index.ts:42`, `handleAuth()` |
| **Inline content** | Text provided directly in the evaluation request | Paragraphs/sections by content reference |

### Pre-Production Evaluation

When evaluating storyboards, scripts, or design documents (pre-production artifacts):

- Criteria about execution quality (visual consistency, narration quality, production values) are scored against the **plan**, not the result
- The score report notes: `[PRE-PRODUCTION EVALUATION — scores reflect planned approach, not final execution]`
- Criteria that cannot be meaningfully evaluated pre-production are scored N/A and excluded from the max possible calculation
- The N/A exclusion is noted in the score report with the adjusted max possible

## Evaluation Pipeline

### Phase 1: Guardrail Scan (Fail-Fast)

Guardrails are evaluated FIRST, before any positive criteria scoring. This prevents wasting evaluation effort on disqualified outputs.

#### 1A: Hard Guardrails

For each hard guardrail defined in the rubric:

1. Examine the entire artifact for evidence of the violation
2. If violation found:
   - Record: `{guardrail_id, guardrail_name, status: "TRIGGERED", evidence: "specific description with location"}`
   - Set `hard_fail = true`
3. If no violation found:
   - Record: `{guardrail_id, guardrail_name, status: "CLEAR"}`

**If any hard guardrail is triggered:**
- Result is **AUTO-FAIL** immediately
- Skip all positive criteria scoring
- Generate the score report with:
  - The triggered hard guardrail(s) with evidence
  - The improvement plan lists ONLY the guardrail fix(es) — nothing else matters until these are resolved
  - A clear statement: "Hard guardrail violation. Resolve the violation and re-submit for full evaluation."

#### 1B: Soft Guardrails

For each soft guardrail defined in the rubric:

1. Examine the artifact for evidence of the violation
2. If violation found:
   - Record: `{guardrail_id, guardrail_name, status: "TRIGGERED", penalty: N, evidence: "specific description with location"}`
   - Add penalty to `total_soft_penalty`
3. If no violation found:
   - Record: `{guardrail_id, guardrail_name, status: "CLEAR"}`

Calculate: `total_soft_penalty = sum(penalty_i)` for all triggered soft guardrails.

**Soft guardrails do not halt evaluation.** Positive criteria scoring proceeds. The penalties are applied to the final percentage.

### Phase 2: Per-Criterion Scoring

For each criterion in the rubric (in order):

#### 2A: Read Level Definitions

Read all 5 level definitions (0 through 4) for this criterion. Internalize what each level requires — the definitions are specific and observable, not generic quality language.

#### 2B: Examine the Artifact

Examine the artifact specifically for evidence relevant to this criterion. Look for:

- Direct evidence that the criterion is satisfied (positive signals)
- Evidence of weakness or absence (negative signals)
- Specific, citable details (timestamps, sections, elements, behaviors)

#### 2C: Determine Best-Fit Level

Compare the gathered evidence against each level definition (0-4). The rating is the **highest level whose definition is fully satisfied** by the evidence. Partial matches round DOWN, not up.

**Rating assignment rules:**

| Situation | Rating |
|-----------|--------|
| Evidence clearly matches a level definition | That level |
| Evidence falls between two levels | Lower level |
| Insufficient evidence to evaluate | 0 (with note: "insufficient evidence") |
| Criterion is N/A for this artifact type | Exclude from scoring (adjust max_possible) |

#### 2D: Record the Score

For each criterion, record:

```yaml
criterion:
  id: 1
  name: "Hook Strength"
  weight: 5
  rating: 4
  weighted_score: 20   # rating × weight
  level_label: "Irresistible Hook"
  evidence: >
    Opens at [0:00-0:02] with "Your front desk is losing $47,000 a year
    in missed calls" — specific dollar figure, directly addresses practice
    owner pain point. Pattern-interrupt achieved within 1.2 seconds.
    Transitions seamlessly into the product demo at [0:02].
```

### Phase 3: Score Calculation

#### 3A: Compute Base Percentage

```
total_score    = sum(weighted_score_i) for all scored criteria
max_possible   = sum(4 × weight_i) for all scored criteria
                 (excludes N/A criteria)
base_percentage = (total_score / max_possible) × 100
```

Round to 1 decimal place.

#### 3B: Apply Soft Guardrail Penalties

```
final_percentage = base_percentage - total_soft_penalty
```

The final percentage can go below 0% (theoretically) if enough soft guardrails are triggered. Display 0.0% as the floor.

#### 3C: Compute Gap Analysis

For each criterion, compute:

```
gap           = 4 - rating
impact_score  = weight × gap
potential_gain = (impact_score / max_possible) × 100   # percentage points recoverable
```

This drives the improvement plan priority ordering.

### Phase 4: Result Determination

#### Pass-Fail Mode (`threshold_mode: "pass-fail"`)

```
IF hard_fail:
    result = "AUTO-FAIL"
    reason = "Hard guardrail violation: {guardrail_name}"
ELIF final_percentage >= pass_threshold:
    result = "PASS"
ELSE:
    result = "FAIL"
    gap_to_pass = pass_threshold - final_percentage
```

#### Triage Mode (`threshold_mode: "triage"`)

```
normalized_score = (total_score / max_possible) × 10   # 0-10 scale
```

Match `normalized_score` against `triage_thresholds` (checked top-down, first matching range wins):

```
IF normalized_score >= 9.0:   action = "auto-keep"
ELIF normalized_score >= 7.0: action = "surface"
ELIF normalized_score >= 3.0: action = "tag"
ELSE:                         action = "discard"
```

### Phase 5: Improvement Plan Generation (If FAIL)

When the result is FAIL (or AUTO-FAIL), generate a prioritized improvement plan.

#### 5A: Collect Improvable Items

Two sources of improvement items:

1. **Guardrail fixes** — each triggered guardrail (hard or soft) becomes an improvement item
2. **Criteria gaps** — each criterion with rating < 4 becomes an improvement item

#### 5B: Calculate Impact Score

For each improvement item:

| Item Type | Impact Score | Rationale |
|-----------|-------------|-----------|
| Hard guardrail fix | `999` (always first) | Must be fixed before any other evaluation |
| Soft guardrail fix | `penalty` value | Direct percentage recovery |
| Criterion improvement | `weight × (4 - rating)` | Weighted gap to perfect |

#### 5C: Priority Sort

Sort all improvement items by `impact_score` descending. Within equal scores, sort by weight descending.

#### 5D: Generate Improvement Entries

For each item (in priority order):

**Guardrail fix format:**
```
1. [HARD GUARDRAIL] {guardrail_name}
   Status: TRIGGERED — {evidence}
   Fix: {specific action to resolve the violation}
   Impact: Resolves AUTO-FAIL → enables full evaluation
```

```
2. [SOFT GUARDRAIL] {guardrail_name} (-{penalty}%)
   Status: TRIGGERED — {evidence}
   Fix: {specific action to resolve the violation}
   Impact: +{penalty}% score recovery
```

**Criterion gap format:**
```
3. [CRITERION] {criterion_name} (rating {current} → target {target}, weight {weight})
   Current Level: {current_level_label} — {evidence of current state}
   Target Level: {target_level_label} — {target level definition from rubric}
   Fix: {specific, actionable steps to reach the target level}
   Impact: +{potential_gain}% ({impact_score} weighted points)
```

#### 5E: Minimum Path to Pass

After listing all improvements, calculate and display the **minimum subset** of fixes needed to reach the pass threshold:

```
MINIMUM PATH TO PASS (96%):
Current score: 92.1%
Gap: 3.9 percentage points

Fix these items to pass:
  1. CTA Effectiveness (3 → 4): +3.0%  → cumulative: 95.1%
  2. Conversion Pathway (3 → 4): +3.0% → cumulative: 98.1% ✓ PASS

Fixing items 1-2 would achieve 98.1% → PASS.
Remaining items are optional improvements toward 100%.
```

This greedy algorithm walks the priority-sorted list, accumulating score gains until the cumulative total exceeds the gap to pass. It stops at the first item that would push past the threshold.

### Phase 6: Score Report Output

The score report is the final output of the evaluation. It follows a fixed format.

## Score Report Format

### Pass-Fail Mode Report

```
═══════════════════════════════════════════════════════════════
  SHIT Loop Score Report
  Rubric: {rubric_name}
  Domain: {domain}
  Artifact: {artifact_path_or_description}
  Date: {YYYY-MM-DD HH:MM}
  Session: {session_id}
═══════════════════════════════════════════════════════════════

  GUARDRAIL CHECK
  ───────────────────────────────────────────────────────────

  Hard Guardrails:
    {✓|✗} {guardrail_name} .................. {CLEAR|TRIGGERED}
      {if TRIGGERED: Evidence: {evidence}}

  Soft Guardrails:
    {✓|✗} {guardrail_name} .................. {CLEAR|TRIGGERED (-N%)}
      {if TRIGGERED: Evidence: {evidence}}

  Soft Penalty Total: -{total_soft_penalty}%
  ───────────────────────────────────────────────────────────

  CRITERIA SCORING
  ───────────────────────────────────────────────────────────

  # | Criterion              | Wt | Rating | Level          | Weighted | Evidence
  ──┼────────────────────────┼────┼────────┼────────────────┼──────────┼─────────
  1 | {criterion_name}       | {w}|  {r}   | {level_label}  |  {w*r}   | {brief evidence}
  2 | ...                    |    |        |                |          |
  ...
  ──┼────────────────────────┼────┼────────┼────────────────┼──────────┼─────────
    | TOTAL                  |    |        |                | {total}  |
  ───────────────────────────────────────────────────────────

  SCORE SUMMARY
  ───────────────────────────────────────────────────────────

  Total Score:      {total_score} / {max_possible}
  Base Percentage:  {base_percentage}%
  Soft Penalty:     -{total_soft_penalty}%
  Final Percentage: {final_percentage}%
  Pass Threshold:   {pass_threshold}%

  Result: {✓ PASS | ✗ FAIL | ✗ AUTO-FAIL}
  ───────────────────────────────────────────────────────────

  {IF FAIL or AUTO-FAIL:}
  IMPROVEMENT PLAN (priority order)
  ───────────────────────────────────────────────────────────

  {numbered improvement entries — see Phase 5D format}

  MINIMUM PATH TO PASS ({pass_threshold}%)
  ───────────────────────────────────────────────────────────

  {minimum path calculation — see Phase 5E format}

═══════════════════════════════════════════════════════════════
```

### Triage Mode Report

```
═══════════════════════════════════════════════════════════════
  SHIT Loop Triage Report
  Rubric: {rubric_name}
  Domain: {domain}
  Asset: {artifact_description}
  Date: {YYYY-MM-DD HH:MM}
═══════════════════════════════════════════════════════════════

  CRITERIA SCORING
  ───────────────────────────────────────────────────────────

  # | Criterion              | Wt | Rating | Weighted | Evidence
  ──┼────────────────────────┼────┼────────┼──────────┼─────────
  1 | {criterion_name}       | {w}|  {r}   |  {w*r}   | {brief evidence}
  ...
  ──┼────────────────────────┼────┼────────┼──────────┼─────────
    | TOTAL                  |    |        | {total}  |
  ───────────────────────────────────────────────────────────

  TRIAGE RESULT
  ───────────────────────────────────────────────────────────

  Raw Score:        {total_score} / {max_possible}
  Normalized Score: {normalized_score} / 10.0
  Action:           {action} — {action_description}

  {IF action is "surface":}
  RATIONALE (for human review)
  ───────────────────────────────────────────────────────────

  Highest-scoring criteria:
    - {criterion_name}: {rating}/4 — {evidence}
    - ...

  Relevant IQ products: {list}
  Transferable concepts: {if Conceptual Transferability >= 3}
  Caveats: {license, maintenance, learning curve}

═══════════════════════════════════════════════════════════════
```

### AUTO-FAIL Report (Abbreviated)

When a hard guardrail triggers, the report is shortened — no criteria scores are shown because the evaluation was halted:

```
═══════════════════════════════════════════════════════════════
  SHIT Loop Score Report
  Rubric: {rubric_name}
  Domain: {domain}
  Artifact: {artifact_path_or_description}
  Date: {YYYY-MM-DD HH:MM}
  Session: {session_id}
═══════════════════════════════════════════════════════════════

  GUARDRAIL CHECK
  ───────────────────────────────────────────────────────────

  Hard Guardrails:
    ✗ {guardrail_name} .................. TRIGGERED
      Evidence: {specific evidence with location}

  *** EVALUATION HALTED — HARD GUARDRAIL VIOLATION ***

  Result: ✗ AUTO-FAIL

  REQUIRED FIX
  ───────────────────────────────────────────────────────────

  1. [HARD GUARDRAIL] {guardrail_name}
     Violation: {evidence}
     Required Action: {specific fix}

  Resolve the hard guardrail violation and re-submit for
  full evaluation. No positive criteria are scored until
  all hard guardrails are clear.

═══════════════════════════════════════════════════════════════
```

## Re-Evaluation Protocol

### When to Re-Evaluate

Re-evaluation is used when a previously-failed artifact has been revised and is submitted again. The command is:

```
/shit-loop re-evaluate --session <session-id> <revised-artifact>
```

### Session State

Each evaluation creates a session record:

```
E:/SHIT-Loop/sessions/{session-id}/
  ├── intent.yaml          # What was evaluated and why
  ├── evaluation-001.yaml  # First evaluation scores + evidence
  ├── evaluation-002.yaml  # Re-evaluation scores + evidence (if applicable)
  └── ...
```

The session ID is a timestamp-based identifier: `{rubric-name}-{YYYYMMDD-HHMMSS}`

Example: `shit-commercial-development-20260321-143022`

### Re-Evaluation Process

1. **Load previous session:** Read the most recent evaluation record
2. **Guardrail re-scan (full):** ALL guardrails are re-checked from scratch. Revisions may have introduced new violations.
3. **Criteria re-scoring (selective):**
   - Identify which criteria were below 4 in the previous evaluation (these are "improvement targets")
   - Also identify any criteria that might have been affected by the revisions (the evaluator uses judgment here)
   - Re-score those criteria against the revised artifact
   - Criteria that were previously at 4 and are not affected by the revision carry forward unchanged
4. **Score recalculation:** Use the new scores (mix of carried-forward and re-scored)
5. **Delta display:** The score report shows the change:

```
  SCORE COMPARISON
  ───────────────────────────────────────────────────────────

  # | Criterion              | Previous | Current | Delta
  ──┼────────────────────────┼──────────┼─────────┼──────
  1 | Hook Strength          |  4 (20)  |  4 (20) | —
  5 | CTA Effectiveness      |  3 (15)  |  4 (20) | +5 ↑
  10| Conversion Pathway     |  3 (15)  |  4 (20) | +5 ↑
  ──┼────────────────────────┼──────────┼─────────┼──────
    | TOTAL                  | 151/164  | 161/164 |

  Previous: 92.1%  →  Current: 98.2%  (+6.1%)
  Result:   FAIL   →  PASS ✓
```

### Iteration Limits

- **Maximum 3 evaluation iterations** per session before escalating to human review
- After 3 iterations that all result in FAIL, the report includes:

```
  *** ITERATION LIMIT REACHED (3/3) ***

  This artifact has been evaluated 3 times without achieving
  a passing score. Escalating to human review.

  Persistent weaknesses:
    - {criterion}: rated {rating} across all 3 iterations
    - {criterion}: improved from {r1} to {r2} but still below target

  Recommendation: The {criterion_name} criterion may require
  a fundamentally different approach rather than incremental
  revision. Consider revisiting the creative brief or strategy.
```

## /shit-loop list

Lists all available rubric skills with summary metadata.

**Output format:**

```
SHIT Loop — Available Rubrics
═══════════════════════════════════════════════════════════════

  Name                          | Domain                    | Mode       | Criteria | Threshold
  ──────────────────────────────┼───────────────────────────┼────────────┼──────────┼──────────
  shit-commercial-development   | Commercial Video Dev      | pass-fail  |    10    |    96%
  shit-inteliq-triage           | IntelIQ Knowledge Pipeline| triage     |     7    |    —

  Rubric directory: E:/SHIT-Loop/skills/
  Total: 2 rubrics

═══════════════════════════════════════════════════════════════
```

**Implementation:**
1. Scan `E:/SHIT-Loop/skills/shit-*.md` (exclude `_template.md` and this file)
2. Parse YAML frontmatter from each file
3. Extract: name, domain, threshold_mode, criteria_count, pass_threshold (or "—" for triage)
4. Display in table format, sorted alphabetically by name

## /shit-loop inspect <rubric-name>

Displays the full structure of a rubric without running an evaluation. Useful for understanding what will be measured before submitting an artifact.

**Output format:**

```
SHIT Loop — Rubric Inspection
═══════════════════════════════════════════════════════════════

  Rubric:       {name}
  Domain:       {domain}
  Output Type:  {output_type}
  Mode:         {threshold_mode} ({pass_threshold}%)
  Version:      {version}
  Author:       {author}
  Created:      {created}
  Updated:      {updated}

  GUARDRAILS
  ───────────────────────────────────────────────────────────

  Hard (auto-fail):
    G1: {name} — {description}
    G2: ...

  Soft (penalty deduction):
    G4: {name} (-{penalty}%) — {description}
    G5: ...

  CRITERIA
  ───────────────────────────────────────────────────────────

  1. {criterion_name} (weight: {weight})
     {criterion_description}
     Levels: 0={label} | 1={label} | 2={label} | 3={label} | 4={label}

  2. ...

  SCORING
  ───────────────────────────────────────────────────────────

  Max Possible: {max_possible} (sum of 4 × each weight)
  Weights: [{w1}, {w2}, ..., {wN}]
  Weight Sum: {sum}
  Pass requires: ≥{pass_threshold}% after guardrail penalties

  WEIGHT DISTRIBUTION
  ───────────────────────────────────────────────────────────

  Weight 5 (critical):  {list of criteria names}
  Weight 4 (important): {list of criteria names}
  Weight 3 (standard):  {list of criteria names}
  Weight 2 (contributing): {list of criteria names}
  Weight 1 (signal):    {list of criteria names}

  A single point lost on a weight-5 criterion costs
  {(1/max_possible)*100*5:.1f}% — equivalent to losing {ratio}
  points on a weight-1 criterion.

═══════════════════════════════════════════════════════════════
```

## Evaluator Conduct Rules

These rules govern how the evaluation engine (whether AI or human) must behave during scoring. They are non-negotiable.

### Rule 1: Evidence-Based Scoring Only

Every rating MUST cite specific, observable evidence from the artifact. Ratings without evidence are invalid.

**WRONG:**
```
Hook Strength: 4 — "Great hook, very engaging"
```

**CORRECT:**
```
Hook Strength: 4 — Opens at [0:00] with "$47,000 — that's what your
practice loses per year from missed calls." Specific dollar figure
creates immediate stakes. Pattern-interrupt within 1.2 seconds.
Transitions directly into product demo at [0:02] with no dead space.
```

### Rule 2: Level Definitions Are Binding

The rubric's level definitions are the scoring authority. The evaluator does not invent new criteria for what constitutes a 3 vs. a 4 — the rubric already defines this. The evaluator's job is to **match evidence to the closest level definition**.

If the evaluator believes the level definitions are wrong or incomplete, that is feedback for the rubric author — it does not change the current evaluation.

### Rule 3: Partial Matches Round Down

If evidence falls between two level definitions (e.g., clearly better than a 2 but doesn't fully satisfy the 3 definition), assign the lower rating. The 96% threshold is deliberately harsh. Rounding up defeats the purpose.

### Rule 4: No Halo Effect

A strong performance on one criterion must not bias the evaluation of other criteria. Each criterion is evaluated independently. A brilliant hook does not make a weak CTA score higher.

### Rule 5: Guardrails Are Binary

Guardrails are not judged on severity — they are either triggered or clear. A "minor" HIPAA violation is still a hard guardrail trigger. A "barely noticeable" placeholder is still a soft guardrail trigger.

### Rule 6: Improvement Plans Are Actionable

Every improvement plan entry must contain a specific, actionable fix — not a restatement of the criterion definition. The person reading the improvement plan should know exactly what to change.

**WRONG:**
```
CTA Effectiveness: Improve the CTA to be more compelling.
```

**CORRECT:**
```
CTA Effectiveness: Add urgency and social proof to the end card CTA.
Current: "Start your free trial at voicesiq.com"
Target: "Join 500+ dental practices — start free, cancel anytime.
Book a demo in 60 seconds." Also reinforce the CTA at the emotional
peak ([0:22]) before the end card, not just at the end.
```

### Rule 7: Consistency Across Evaluations

The same artifact evaluated twice with the same rubric should produce ratings within 1 level of each other on every criterion. If it does not, the level definitions need refinement (feedback to rubric author).

### Rule 8: Pre-Production Adjustments

When evaluating pre-production artifacts (storyboards, scripts, design docs):
- Score execution-dependent criteria (visual consistency, narration, pacing) against the **plan quality**, not imagined execution
- Note `[PRE-PRODUCTION]` next to any criterion scored this way
- Exclude criteria that genuinely cannot be evaluated pre-production (mark N/A)
- Adjust max_possible to exclude N/A criteria

## Session State Management

### Directory Structure

```
E:/SHIT-Loop/
  ├── skills/                    # Rubric skill files
  │   ├── shit-commercial-development.md
  │   ├── shit-inteliq-triage.md
  │   ├── shit-loop-evaluate.md  # This file (engine)
  │   └── _template.md           # Blank rubric template
  │
  ├── sessions/                  # Evaluation session records
  │   └── {session-id}/
  │       ├── intent.yaml        # What was evaluated
  │       ├── evaluation-001.yaml # First evaluation
  │       ├── evaluation-002.yaml # Re-evaluation (if any)
  │       └── report.md          # Human-readable score report
  │
  ├── docs/
  │   ├── design-spec.md         # Framework design specification
  │   └── BACKLOG.md             # Task tracker
  │
  └── CLAUDE.md                  # Project instructions
```

### Session Record Format

Each evaluation produces a YAML record:

```yaml
session_id: "shit-commercial-development-20260321-143022"
rubric: "shit-commercial-development"
artifact: "./output/voicesiq-demo-v2.mp4"
artifact_type: "video"
evaluator: "claude-opus-4.6"
timestamp: "2026-03-21T14:30:22Z"
iteration: 1

guardrails:
  hard:
    - id: G1
      name: "HIPAA Violation"
      status: "CLEAR"
    - id: G2
      name: "Copyright Infringement"
      status: "CLEAR"
    - id: G3
      name: "False Medical Claims"
      status: "CLEAR"
  soft:
    - id: G4
      name: "Stock Footage Substitution"
      status: "CLEAR"
    - id: G5
      name: "Missing Captions"
      status: "CLEAR"
    - id: G6
      name: "Placeholder Content"
      status: "CLEAR"
    - id: G7
      name: "Competitor Branding Visible"
      status: "CLEAR"
  total_soft_penalty: 0

criteria:
  - id: 1
    name: "Hook Strength"
    weight: 5
    rating: 4
    weighted_score: 20
    level_label: "Irresistible Hook"
    evidence: "Opens with '$47,000' statistic at [0:00-0:02]..."
  # ... all criteria

scores:
  total_score: 151
  max_possible: 164
  base_percentage: 92.1
  soft_penalty: 0
  final_percentage: 92.1

result:
  outcome: "FAIL"
  threshold: 96
  gap: 3.9

improvement_plan:
  - type: "criterion"
    name: "CTA Effectiveness"
    current_rating: 3
    target_rating: 4
    impact_score: 5
    potential_gain: 3.0
    fix: "Add urgency and social proof to end card CTA..."
  # ... sorted by impact_score descending

minimum_path_to_pass:
  items:
    - name: "CTA Effectiveness"
      gain: 3.0
      cumulative: 95.1
    - name: "Conversion Pathway"
      gain: 3.0
      cumulative: 98.1
  final_projected: 98.1
  passes: true
```

## Integration Patterns

### Direct CLI Invocation

```bash
# Evaluate a video against the commercial development rubric
/shit-loop evaluate --rubric shit-commercial-development ./output/voicesiq-demo.mp4

# Evaluate a knowledge asset for triage
/shit-loop evaluate --rubric shit-inteliq-triage "https://github.com/example/repo"

# Re-evaluate after revisions
/shit-loop re-evaluate --session shit-commercial-development-20260321-143022 ./output/voicesiq-demo-v2.mp4

# List available rubrics
/shit-loop list

# Inspect a rubric before evaluation
/shit-loop inspect shit-commercial-development
```

### Autopilot Integration

When Autopilot delegates a task that has a SHIT Loop quality gate:

```yaml
# Autopilot task with quality gate
task:
  name: "Create VoicesIQ product demo video"
  quality_gate:
    rubric: "shit-commercial-development"
    threshold: 96
    max_iterations: 3
    on_fail: "revise_and_resubmit"
    on_max_iterations: "escalate_to_human"
```

Autopilot behavior:
1. Execute the task (produce the artifact)
2. Invoke `/shit-loop evaluate --rubric {rubric} {artifact}`
3. If PASS: mark task complete
4. If FAIL: read improvement plan, revise artifact, re-evaluate
5. If 3 iterations fail: escalate to human with full evaluation history

### DemoIQ Pipeline Integration

```
Storyboard → Content Capture → Video Forge (Veo3) → Demo Stitcher
    → /shit-loop evaluate --rubric shit-commercial-development ./final.mp4
        |── PASS (≥96%) → Platform Export → Publish
        |── FAIL (<96%) → Improvement Plan → Revise Storyboard
                        → Re-stitch → Re-evaluate (loop until 96% or 3 iterations)
```

### Unified Orchestrator Delegation

```bash
/delegate evaluator "/shit-loop evaluate --rubric shit-commercial-development ./output.mp4"
```

The unified orchestrator delegates to an evaluator sub-agent, which:
1. Loads this evaluation engine skill
2. Loads the specified rubric skill
3. Executes the full evaluation pipeline
4. Returns the score report to the orchestrator

## Error Handling

| Error | Handling |
|-------|----------|
| Rubric file not found | List available rubrics, suggest closest match, abort |
| Rubric validation fails | Display specific validation errors, abort |
| Artifact not accessible | Display path/URL, verify existence, abort with instructions |
| Artifact type not supported | List supported types, suggest alternatives, abort |
| Session not found (re-evaluate) | List recent sessions, suggest closest match, abort |
| Iteration limit reached | Display escalation message, preserve all evaluation history |
| N/A criteria exceed 50% of total | Warn: "Over half the criteria are N/A. This rubric may not be appropriate for this artifact type. Consider using a different rubric." |

## Versioning and Compatibility

### Rubric Version Compatibility

The evaluation engine reads the rubric's `version` field but does not enforce version constraints. Rubrics are self-contained — the engine treats whatever it reads as the current truth.

If a rubric is updated between evaluation iterations in the same session, the re-evaluation uses the **current rubric version**, not the version from the original evaluation. This is logged:

```
NOTE: Rubric version changed between iterations.
  Iteration 1: shit-commercial-development v1.0.0
  Iteration 2: shit-commercial-development v1.1.0
  Criteria may have changed. Full re-evaluation performed.
```

### Engine Version

This evaluation engine is at version 1.0.0. Changes to the engine (scoring formula, report format, conduct rules) are versioned independently from individual rubrics.

## Notes

- **This file is the evaluation engine, not a rubric.** It does not contain criteria, weights, or level definitions. Those live in individual rubric skill files.
- **The engine is model-agnostic** in principle but is designed for Opus 4.6 as the primary evaluator model. The quality of evidence citation and level-matching improves with model capability.
- **Triage mode rubrics do not have guardrails** by convention. Guardrails are a pass-fail concept (blocking publication). Triage rubrics route assets to appropriate handling — even a low-scoring asset gets routed (to discard), not failed.
- **Session records are append-only.** Evaluation records are never modified after creation. This preserves the audit trail.
- **The 96% threshold is not configurable per-evaluation.** It is set in the rubric's frontmatter. To change the threshold, update the rubric. This prevents threshold-shopping during evaluation.
- **Soft guardrail penalty cap:** Not implemented in v1.0.0. Penalties stack without limit. This is an open design question (see design-spec.md Section 14). If a future version introduces a cap, it will be set in the rubric frontmatter as `soft_penalty_cap: 20`.
- **Pre-production evaluation** is valuable for catching strategic issues before production investment. A storyboard that fails on Problem-Solution Clarity will produce a video that fails on Problem-Solution Clarity — catching it early saves time and money.
