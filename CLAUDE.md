# SHIT Loop — Sheller's Superior Human In The Loop Process

**©2026 G. Bradley Scheller / ToolsIQ LLC. All rights reserved.**
**SHIT Loop™ and Sheller's Superior Human In The Loop Process™ are trademarks of ToolsIQ LLC.**
**Patent Pending — U.S. Provisional Patent Application**

---

## Project Overview

**SHIT Loop** is a research-backed, rubric-based quality assurance engine that creates bespoke evaluation instruments for any type of output (commercials, presentations, landing pages, voice AI, code, etc.) and loops until a 96%+ threshold is achieved.

It lives at the **ToolsIQ framework level** — consumed by DemoIQ, VoicesIQ, CasesIQ, ExamIQ, and any other product as a quality gate.

## Architecture

**Agent** (`/shit-loop`) with individual **rubric skills** (open-source):

```
/shit-loop
  ├── Interview Engine (Opus 4.6)
  ├── Research Engine (Gemini Deep Research + Perplexity → NotebookLM)
  ├── Criteria Synthesizer (NotebookLM ↔ Opus interaction)
  ├── Evaluation Engine (scoring + improvement plan)
  └── Rubric Skills Library:
        ├── shit-commercial-development
        ├── shit-strategic-presentation
        ├── shit-voice-ai
        ├── shit-landing-page
        ├── shit-carousel-content
        ├── shit-pitch-deck
        └── ... (grows with every new use case)
```

## Full Pipeline (5 Steps)

### Step 0: Intent
"What are you trying to evaluate?" — understand the domain, the output type, any exemplars.

### Step 1: Research Phase (automated)
- Gemini Deep Research → synthesized report + source URLs
- Perplexity → second independent report + additional source URLs
- Both reports + all sources loaded into a NotebookLM notebook

### Step 2: Criteria Synthesis (automated)
- NotebookLM synthesis OR Opus ↔ NotebookLM interaction
- Output: Draft criteria with 5-level definitions, grounded in research
- AI suggests criteria the human may not have considered

### Step 3: Interview Phase (human-in-the-loop)
- AI presents research-backed criteria suggestions
- Human refines, adds, removes criteria
- Each criterion gets: weight (1-5), 5-level definitions (0-4):
  - 4: Excellence — specific description of what excellence looks like
  - 3: Above Average — specific description
  - 2: Average — specific description
  - 1: Below Average — specific description
  - 0: Unacceptable — specific description
- AI brings research to bear: "The literature suggests X is critical because..."
- Output: Final rubric stored as a reusable skill

### Step 4: Evaluation Loop
- Apply rubric to output — AI evaluates + cites evidence
- Score: Rating (0-4) × Weight (1-5) per criterion
- Percentage = Total Score / Total Possible × 100
- ≥96% → PASS
- <96% → FAIL:
  - Rank criteria by gap-to-perfect
  - For each failing criterion, cite research + level definition for target level
  - Prioritized improvement plan: "Do these N things to cross 96%"
- Revised work → re-evaluate → loop until 96%+

## Scoring Formula

```
Total Score = Σ(Rating_i × Weight_i) for all criteria
Total Possible = Σ(4 × Weight_i) for all criteria
Percentage = (Total Score / Total Possible) × 100
Pass Threshold = 96%
```

## Key Design Decisions

- **96% threshold** is deliberately harsh — forces near-perfection
- **5 levels (0-4)** with specific definitions — not generic "good/bad"
- **Weighted criteria** — not all criteria are equal
- **Research-backed** — rubrics grounded in Gemini + Perplexity + NotebookLM research
- **Improvement suggestions cite research** — not generic advice
- **Rubrics are skills** — individually installable, versionable, open-source
- **Framework-level** — not product-specific

## Connections to Other Projects

- **DemoIQ** (C:\Users\Brads\remix-of-voicesiq-landing-pro) — uses SHIT Loop as quality gate for commercials, landing pages, carousels
- **ProdDevIQ3** (github.com/Brads777/ProdDevIQ3) — test-prototype-SKILL.md has synthetic persona testing patterns
- **ProdDevIQ2** (github.com/Brads777/ProdDevIQ2) — LLM Council protocol for multi-model evaluation
- **MKT2700-ProdDevIQ** (github.com/Brads777/MKT2700-ProdDevIQ) — Phase 2 rubric creation, Phase 5 concept evaluation
- **NotebookLM Plugin** (~/.claude/plugins/notebooklm-research/) — research bridge for loading sources

## Tech Stack

- Claude Code skill/agent (primary interface)
- Opus 4.6 for interview + evaluation (quality reasoning)
- Gemini Deep Research API for synthesized research reports
- Perplexity API for independent second-source research
- NotebookLM for research corpus management + synthesis
- YAML for rubric storage format
- GitHub for open-source rubric skills distribution

## Related Memory

- Memory file: ~/.claude/projects/C--Users-Brads/memory/shit-loop-framework.md
- DemoIQ memory: ~/.claude/projects/C--Users-Brads/memory/demoiq-project.md
- Brad profile: ~/.claude/projects/C--Users-Brads/memory/brad-profile.md
