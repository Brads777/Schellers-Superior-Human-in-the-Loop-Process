# SHIT Loop — Backlog

## High Priority
- [ ] Build interview engine skill — intent gathering + criteria co-creation (Phase 2)
- [ ] Build research engine — Gemini Deep Research + Perplexity → NotebookLM pipeline
- [ ] Build criteria synthesizer — NotebookLM ↔ Opus interaction for draft criteria
- [ ] Build rubric skill template (`_template.md`) — boilerplate for creating new rubric skills quickly

## Medium Priority
- [ ] Build orchestrator agent (/shit-loop) — sequences research → interview → evaluate
- [ ] Create shit-strategic-presentation rubric skill
- [ ] Create shit-landing-page rubric skill
- [ ] Create shit-voice-ai rubric skill
- [ ] NotebookLM plugin integration — automated source loading

## Low Priority / Future
- [ ] Open-source rubric skills repo (separate from main repo?)
- [ ] SHIT Loop web UI (part of ToolsIQ platform)
- [ ] Rubric marketplace on ToolsIQ
- [ ] Community-contributed rubrics with review process
- [ ] Analytics dashboard — track quality improvement over iterations

## Completed (Recent)
- [x] **Evaluation engine skill written** — 971 lines, full pipeline: guardrail scan, per-criterion scoring, score calculation, improvement plan, re-evaluation, session state, report format → `skills/shit-loop-evaluate.md` (2026-03-21)
- [x] **`shit-commercial-development` rubric skill written** — 10 criteria, two-tier guardrails (3 hard + 4 soft), pass-fail 96%, scored examples, DemoIQ integration → `skills/shit-commercial-development.md` (2026-03-21)
- [x] **Score report generator** — included in evaluation engine as Phase 6 with 3 report formats (pass-fail, triage, auto-fail) (2026-03-21)
- [x] `shit-inteliq-triage` rubric skill — 7 criteria, triage thresholds, examples, pipeline integration (2026-03-15)
- [x] Rubric YAML schema defined by example — `skills/shit-inteliq-triage.md` serves as format reference (2026-03-15)
- [x] **Design spec written** — 849 lines, 14 sections, two-tier guardrails system → `docs/design-spec.md` (2026-03-21)
- [x] Concept defined in DemoIQ brainstorming session (March 12, 2026)
- [x] Architecture outlined: agent + rubric skills + research pipeline
- [x] Research pipeline refined: Gemini Deep Research + Perplexity → NotebookLM → Opus synthesis
- [x] GitHub repo created: Brads777/Schellers-Superior-Human-in-the-Loop-Process
- [x] Working directory: E:\SHIT-Loop
- [x] CLAUDE.md written with full pipeline documentation

## References
- **DemoIQ** — primary consumer (quality gate for commercials, landing pages)
- **ProdDevIQ3 test-prototype-SKILL.md** — synthetic persona testing patterns (50-100 personas, Likert scale)
- **ProdDevIQ2 llm-council-protocol.md** — multi-model evaluation framework
- **MKT2700-ProdDevIQ Phase 2** — rubric creation methodology (weighted 0-4 scale, same foundation)
- **NotebookLM Plugin** — ~/.claude/plugins/notebooklm-research/ (research bridge)
