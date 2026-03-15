---
name: shit-inteliq-triage
description: "SHIT Loop rubric for IntelIQ knowledge triage. Evaluates knowledge assets (tools, repos, articles, videos, libraries) against Brad's process reengineering philosophy using 7 weighted criteria (0-4 scale). Normalized to 0-10 for triage decisions: auto-discard <3, surface 7+, auto-keep 9+. Criteria: Process Decomposition, Tool-Task Intelligence, Conceptual Transferability, Quality Architecture, Systems Integration, Continuous Improvement, Community Signal. Use for IntelIQ pipeline scoring, GitHub Awesome evaluation, and knowledge curation."
---

# ©2026 Brad Scheller

# SHIT Loop Rubric — IntelIQ Knowledge Triage

## Metadata

- **Rubric Name:** shit-inteliq-triage
- **Domain:** IntelIQ Knowledge Pipeline
- **Task Type:** Knowledge asset triage — keep, review, or discard
- **Created:** 2026-03-15
- **Source:** Research-backed (GitHub Awesome archive analysis + Brad's process reengineering philosophy)
- **Master Rubric Alignment:** SHIT Loop framework (adapted for triage vs. production quality gate)
- **Scoring Model:** Flat weighted, 0-4 per criterion × weight (1-5), normalized to 0-10

## Purpose

Evaluate knowledge assets — tools, repositories, articles, videos, libraries, frameworks — for
inclusion in the IntelIQ Knowledge Hub. Unlike standard SHIT Loop rubrics that use a 96% pass/fail
gate for production deliverables, this rubric is a **triage instrument** that sorts incoming
knowledge into keep/review/discard buckets.

The rubric is grounded in Brad's core thesis:
> Take a complex human process → break it into discrete tasks → identify the best tools and
> knowledge for each task → apply quality rubrics → build a system that continuously improves.

Assets are evaluated not just on direct utility, but on whether the **concepts and ideas** behind
them can be transferred to ToolsIQ products or Brad's broader engineering practice.

## Prerequisites

- Knowledge asset URL, description, or reference (GitHub repo, article, video, tool)
- Optional: GitHub metadata (stars, language, license, topics, last commit date)
- Optional: Context about which IQ product(s) might benefit
- The IntelIQ relevance topics from `G:/knowledge-archive/config/sources.yaml`

## Scoring Formula

```
Raw Score      = Σ(Rating_i × Weight_i) for all 7 criteria
Max Possible   = Σ(4 × Weight_i) = (5+5+5+4+4+3+1) × 4 = 108
Normalized     = (Raw Score / 108) × 10    # yields 0-10 scale
```

## Triage Thresholds

| Normalized Score | Action | Description |
|-----------------|--------|-------------|
| **9.0 – 10.0** | **Auto-keep** | Immediately archive to Obsidian vault. No human review needed. |
| **7.0 – 8.9** | **Surface for review** | Present to Brad with scoring rationale. Human decides keep/discard. |
| **3.0 – 6.9** | **Tag and file** | Store metadata only. Available for search but not surfaced. |
| **0.0 – 2.9** | **Auto-discard** | Delete after staging TTL expires (7 days). Not worth storing. |

## Evaluation Matrix

| # | Criterion | Wt | 0 — Irrelevant | 1 — Marginal | 2 — Useful | 3 — Valuable | 4 — Essential |
|---|-----------|----|----|----|----|----|----|
| 1 | Process Decomposition | 5 | No process insight. Solves a point problem without acknowledging the broader workflow it sits in. | Acknowledges a multi-step process exists but doesn't decompose it — treats the process as a monolith. | Identifies 2-3 steps within a process and addresses them, but misses key phases or treats decomposition as incidental. | Clearly decomposes a complex process into discrete, named tasks. Each task is independently addressable and improvable. | Provides a complete process decomposition framework — maps the full workflow, identifies dependencies between tasks, and enables targeted improvement at each stage. |
| 2 | Tool-Task Intelligence | 5 | Generic tool with no task-specific insight. "Use AI for everything" mentality. A wrapper over an existing capability. | Targets a specific task but brings no insight about WHY this tool fits this task better than alternatives. | Good tool-task fit for its primary use case. Demonstrates understanding of the task's requirements and matches capabilities accordingly. | Strong tool-task alignment with clear rationale. Explains trade-offs vs. alternatives. Could inform tool selection for similar tasks in other domains. | Exceptional tool-task matching that reveals non-obvious insights about the task itself. The tool design teaches you something about what the task actually requires. |
| 3 | Conceptual Transferability | 5 | Concept is domain-locked. No transferable ideas — the value is entirely in the specific implementation for a specific use case. | One narrow idea might transfer, but it would require significant reimagining. The concept is tightly coupled to its original domain. | Core concept transfers to 1-2 other IQ products or domains with moderate adaptation. The idea is separable from the implementation. | Concept is broadly transferable across 3+ IQ products or domains. The underlying pattern (not just the tool) applies to diverse processes. | Universal pattern that redefines how you think about a class of problems. Transferable across virtually any IQ product. Changes your mental model. |
| 4 | Quality Architecture | 4 | No quality measurement. No feedback loop. Ship and forget. | Has basic error handling or validation, but no structured quality assessment. Quality is an afterthought. | Includes measurable quality criteria for its primary output. Has some form of feedback mechanism (logs, metrics, user ratings). | Built-in quality rubrics or scoring systems. Supports iterative improvement based on quality data. Quality is a first-class design concern. | Comprehensive quality architecture: multi-dimensional scoring, threshold-based gates, automated improvement suggestions, research-backed criteria. SHIT Loop-aligned. |
| 5 | Systems Integration | 4 | Standalone silo. No APIs, no data export, no way to connect with other tools. Requires copy-paste to use output. | Has basic import/export (CSV, JSON) but no live integrations. Can be manually wired into a pipeline with effort. | Standard integration points (REST API, webhooks, or plugin system). Can be incorporated into an automated pipeline. | Rich integration ecosystem. Multiple protocols (API, MCP, WebSocket, SDK). Designed to be a component in a larger system, not a standalone product. | Integration-first architecture. Treats connectivity as a core feature. Provides adapters, middleware patterns, or protocol bridges that make it a natural fit in any IQ product pipeline. |
| 6 | Continuous Improvement | 3 | Static tool. No learning, no adaptation. Works the same on day 1 and day 1,000. | Collects usage data but doesn't act on it. Improvement requires manual intervention and full reconfiguration. | Supports configuration-driven improvement — you can tune parameters based on results. Has versioning or A/B testing capability. | Built-in learning loops. Adapts behavior based on outcomes. Tracks what works and what doesn't. Supports systematic experimentation. | Self-improving system with feedback-driven optimization. Learns from its own outputs, surfaces improvement opportunities, and can evolve its approach autonomously. |
| 7 | Community & Maturity Signal | 1 | Abandoned or pre-alpha. No documentation, no community, no maintenance signal. Red flags (security issues, license problems). | Active but early. <100 stars, sparse docs, single maintainer, infrequent commits. May not survive 6 months. | Moderate community. 100-1K stars, decent docs, small but active contributor base. Likely to be maintained for 1+ year. | Strong community. 1K-10K stars, comprehensive docs, multiple active maintainers, regular releases. Enterprise-viable. | Thriving ecosystem. 10K+ stars, professional docs, corporate backing or large community, plugin/extension ecosystem. Industry standard or emerging standard. |

## Evaluation Process

### Step 1: Gather Context
Collect available information about the knowledge asset:
- URL, title, description
- GitHub metadata (if applicable): stars, language, license, topics, last commit
- Video/article content summary (if applicable)
- Which IQ products or processes it might relate to

### Step 2: Score Each Criterion
For each of the 7 criteria:
1. Read the level definitions (0-4) for that criterion
2. Determine which level best describes this asset
3. Assign the rating (0-4)
4. Note brief justification citing specific evidence

### Step 3: Calculate Normalized Score
```
Raw Score  = Σ(Rating × Weight)
Normalized = (Raw Score / 108) × 10
```

### Step 4: Apply Triage Decision
Use the triage thresholds table to determine action (auto-keep, surface, tag, or auto-discard).

### Step 5: Generate Scoring Rationale
For assets scoring 7+ (surfaced for review), produce a brief rationale:
- Which criteria scored highest and why
- Specific IQ products that would benefit
- If Conceptual Transferability scored 3+, describe the transferable concept
- Any caveats (license issues, maintenance risk, steep learning curve)

## Examples

### Example: High Score (Langfuse — LLM Observability)
| Criterion | Rating | Weighted |
|-----------|--------|----------|
| Process Decomposition | 3 | 15 |
| Tool-Task Intelligence | 4 | 20 |
| Conceptual Transferability | 3 | 15 |
| Quality Architecture | 4 | 16 |
| Systems Integration | 4 | 16 |
| Continuous Improvement | 3 | 9 |
| Community Signal | 3 | 3 |
| **Total** | | **94/108** |
| **Normalized** | | **8.7** |
| **Decision** | | **Surface for review** |

**Rationale:** Langfuse decomposes the LLM evaluation process into traces, spans, and scores —
directly aligning with quality architecture patterns. The observability-as-quality-gate concept
transfers to VoicesIQ (call quality scoring), CasesIQ (grading pipeline monitoring), and ExamIQ
(clinical recommendation auditing). Strong integration (OpenAI SDK, LangChain, custom SDKs).

### Example: Low Score (Random CSS Animation Library)
| Criterion | Rating | Weighted |
|-----------|--------|----------|
| Process Decomposition | 0 | 0 |
| Tool-Task Intelligence | 1 | 5 |
| Conceptual Transferability | 0 | 0 |
| Quality Architecture | 0 | 0 |
| Systems Integration | 1 | 4 |
| Continuous Improvement | 0 | 0 |
| Community Signal | 2 | 2 |
| **Total** | | **11/108** |
| **Normalized** | | **1.0** |
| **Decision** | | **Auto-discard** |

**Rationale:** Point utility with no process insight, no quality architecture, no transferable
concepts. Useful for animations but brings no engineering philosophy value.

## Integration with IntelIQ Pipeline

This rubric is applied at the **triage stage** of the IntelIQ pipeline:

```
Source → Catch → [shit-inteliq-triage scoring] → Route:
                    ├── 9+    → Auto-keep → Obsidian vault
                    ├── 7-8.9 → Surface → Brad reviews → keep/discard
                    ├── 3-6.9 → Tag → metadata only
                    └── <3    → Auto-discard → delete after 7 days
```

The rubric can be applied by:
- **AI scoring** (automated, using Claude/Gemini with this rubric as system prompt)
- **Human scoring** (Brad manually evaluating surfaced items)
- **Hybrid** (AI pre-scores, human reviews borderline cases)

## Source Health Feedback Loop

Rubric scores feed back into source health tracking:
- Sources whose items consistently score <3 get flagged for removal (20% keep-rate threshold)
- Sources whose items consistently score 7+ get marked as high-value
- Outbound links in auto-kept items suggest new sources worth following

## Notes

- **Conceptual Transferability is key.** A tool might score 0 on Process Decomposition for its
  own domain but 4 on Conceptual Transferability if the underlying idea applies elsewhere. Example:
  a music composition tool that uses rubric-based quality scoring — the music domain is irrelevant,
  but the quality architecture pattern transfers directly to SHIT Loop.
- **Community Signal has weight 1 intentionally.** Brad cares about ideas and concepts, not
  popularity contests. A 50-star repo with a brilliant decomposition pattern outranks a 50K-star
  wrapper.
- **AGPL is acceptable** for internal/dev tools. Only flag AGPL as a concern for customer-facing
  product dependencies.
