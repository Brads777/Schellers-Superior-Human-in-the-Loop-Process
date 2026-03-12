# SHIT Loop — Session Referral

**Created:** March 12, 2026, during DemoIQ brainstorming session
**Start here:** Read `CLAUDE.md` in the project root — it has the full architecture.

## What This Is

**SHIT Loop** (Sheller's Superior Human In The Loop Process) is a research-backed, rubric-based quality assurance engine. It creates bespoke evaluation rubrics for any output type and loops until 96%+ quality is achieved.

## Where We Left Off

The concept was fully designed during a DemoIQ brainstorming session. The architecture, pipeline, and scoring model are documented in `CLAUDE.md`. Nothing has been built yet — this repo needs its own brainstorming → spec → implementation cycle.

## Recommended Next Steps

1. **Run `/brainstorming`** to do a formal design spec for the SHIT Loop
2. Start by reading these files for context:
   - `CLAUDE.md` (this project — full pipeline docs)
   - `docs/BACKLOG.md` (task tracker)
3. Key design decisions to make:
   - Rubric YAML schema format
   - Interview engine UX (one question at a time, like ProdDevIQ skills)
   - Research engine API integration (Gemini Deep Research, Perplexity, NotebookLM)
   - How rubric skills are packaged and distributed
   - Evaluation engine output format

## Context From Origin Session

### The 5-Step Pipeline
```
Step 0: Intent → "What are you evaluating?"
Step 1: Research → Gemini Deep Research + Perplexity → NotebookLM
Step 2: Synthesis → NotebookLM ↔ Opus → draft criteria + level definitions
Step 3: Interview → Human refines criteria, weights, levels (research-informed)
Step 4: Evaluate → Score against rubric, loop until ≥96%
```

### Scoring Model
- Each criterion: weight (1-5), rated (0-4) with specific level definitions
- Score = Σ(Rating × Weight) / Σ(4 × Weight) × 100
- Pass = ≥96%, Fail = improvement plan with research-cited suggestions

### Key Architectural Decisions
- **Framework-level** — lives above any single product, consumed by DemoIQ, VoicesIQ, CasesIQ, etc.
- **Agent + Skills** — `/shit-loop` orchestrator agent + individual rubric skills (shit-commercial-development, etc.)
- **Open source** — rubric skills will be published as an open-source GitHub repo
- **NotebookLM integration** — research corpus informs both rubric creation AND improvement suggestions
- **Interview pattern** — one question at a time, AI suggests + human refines (same as ProdDevIQ3 skills)

### Relevant Existing Skills to Study
| Skill | Repo | Why |
|-------|------|-----|
| test-prototype-SKILL.md | ProdDevIQ3 | Synthetic persona testing, Likert scale, archetype interviews |
| build-gtm-SKILL.md | ProdDevIQ3 | Interview pattern (one Q at a time), evidence-based decisions |
| pitch-builder-SKILL.md | ProdDevIQ3/4 | Artifact ingestion, gap-filling interview, branded output |
| phase-2-rubric-creation.md | MKT2700-ProdDevIQ | Weighted 0-4 scale rubric — SAME FOUNDATION as SHIT Loop |
| phase-5-concept-evaluation.md | MKT2700-ProdDevIQ | LLM Council evaluation, Red Team debate |
| llm-council-protocol.md | ProdDevIQ2 | Multi-model evaluation framework |
| synthetic-persona-validation.md | ProdDevIQ2 | 100-200 persona generation and testing |

### Connection to DemoIQ
DemoIQ (C:\Users\Brads\remix-of-voicesiq-landing-pro) will consume SHIT Loop as a quality gate:
- `shit-commercial-development` rubric for video commercials
- `shit-landing-page` rubric for interactive web demos
- `shit-carousel-content` rubric for social media exports
- The first test case: evaluate the VoicesIQ V2 commercial

### Memory Files
- ~/.claude/projects/C--Users-Brads/memory/shit-loop-framework.md
- ~/.claude/projects/C--Users-Brads/memory/demoiq-project.md
- ~/.claude/projects/C--Users-Brads/memory/MEMORY.md (index)

## Repo Info
- **GitHub:** https://github.com/Brads777/Schellers-Superior-Human-in-the-Loop-Process
- **Local:** E:\SHIT-Loop
- **License:** TBD (will be open source)
