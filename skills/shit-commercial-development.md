---
name: shit-commercial-development
description: >
  SHIT Loop rubric for commercial video development.
  Evaluates video commercials and product demos against 10 weighted criteria
  (0-4 scale). Pass-fail mode with 96% threshold. Two-tier guardrails:
  hard (HIPAA, copyright, false medical claims) and soft (stock footage,
  missing captions, placeholder content, competitor branding).
  Criteria: Hook Strength, Problem-Solution Clarity, Visual Consistency,
  Pacing, CTA Effectiveness, Data Credibility, Technical Accuracy,
  Platform Optimization, Narration Quality, Conversion Pathway.
version: "1.0.0"
created: "2026-03-21"
updated: "2026-03-21"
author: "Brad Scheller"
domain: "Commercial Video Development"
output_type: "video commercial / product demo"
threshold_mode: "pass-fail"
pass_threshold: 96
criteria_count: 10
max_possible: 164
tags:
  - quality-gate
  - commercial
  - video
  - demo
  - demoiq
---

# ©2026 Brad Scheller

# SHIT Loop Rubric — Commercial Video Development

## Metadata

- **Rubric Name:** shit-commercial-development
- **Domain:** Commercial Video / Product Demo
- **Task Type:** Quality gate — pass or fail before publishing
- **Created:** 2026-03-21
- **Source:** Research-backed (social media ad performance studies, video marketing benchmarks, platform algorithm research, Brad's process reengineering philosophy)
- **Master Rubric Alignment:** SHIT Loop framework (pass-fail mode, 96% threshold)
- **Scoring Model:** Weighted 0-4 per criterion x weight (1-5), percentage against max possible (164)

## Purpose

Evaluate commercial videos and product demos before publication across any platform (social media, website, sales decks, trade show displays). This rubric ensures every published video meets a near-excellence bar across all dimensions that drive viewer engagement and conversion.

The 96% threshold means a video cannot pass with any significant weakness — a brilliant hook with a weak CTA still fails, and a technically perfect video with no emotional resonance still fails. This forces holistic quality.

This rubric is the primary quality gate for **DemoIQ** output and is used whenever Brad or the ToolsIQ team produces video content for any IQ product.

## Prerequisites

- Video file (local path or URL) or detailed storyboard/script for pre-production evaluation
- Target platform(s) identified (Instagram Reels, TikTok, YouTube, LinkedIn, website embed, trade show)
- Target audience defined (e.g., dental practice owners, 35-60, considering AI tools)
- Product/service being promoted is identified
- Brand guidelines available (colors, fonts, logo placement, tone of voice)

## Guardrails

### Hard Guardrails (Auto-Fail)

Any single hard guardrail violation results in **AUTO-FAIL** regardless of positive criteria scores. These are evaluated FIRST (fail-fast).

| ID | Guardrail | Description |
|----|-----------|-------------|
| G1 | **HIPAA Violation** | Contains protected health information (PHI), real patient names, identifiable health records, or clinical data not properly anonymized. Includes visible patient charts, appointment screens with real data, or dental images with patient identifiers. |
| G2 | **Copyright Infringement** | Uses unlicensed copyrighted material — music tracks without sync license, footage from other productions without permission, trademarked sound effects, or AI-generated content that replicates identifiable copyrighted works. |
| G3 | **False Medical Claims** | Makes specific clinical promises, guarantees treatment outcomes, claims FDA approval that doesn't exist, or states measurable health benefits without proper clinical evidence and disclaimers. Example: "Our AI guarantees 30% better patient outcomes." |

### Soft Guardrails (Penalty Deductions)

Each triggered soft guardrail deducts a fixed percentage from the final score. Penalties stack.

| ID | Guardrail | Penalty | Description |
|----|-----------|---------|-------------|
| G4 | **Stock Footage Substitution** | -3% | Uses generic stock footage (smiling actors in lab coats, generic office shots) where real product footage, actual UI recordings, or genuine practice footage was available and could have been captured. |
| G5 | **Missing Captions** | -3% | No burned-in captions or subtitle track. Fails accessibility requirements and loses viewers on silent autoplay (85% of social media video views are without sound). |
| G6 | **Placeholder Content** | -5% | Contains lorem ipsum, "TODO" markers, "[INSERT LOGO]" placeholders, default template text, test data in UI screenshots, or clearly unfinished visual elements. |
| G7 | **Competitor Branding Visible** | -4% | Shows competitor logos, UI, product names, or identifiable branding in any frame — including in comparison sections where the competitor is identified by name/logo rather than described generically. |

## Scoring Formula

```
For 10 criteria with weights:

  Rating_i  = score for criterion i (integer, 0-4)
  Weight_i  = weight of criterion i (integer, 1-5)

  Total Score     = sum(Rating_i x Weight_i) for i = 1..10
  Max Possible    = sum(4 x Weight_i) = 4 x (5+5+4+4+5+3+4+3+3+5) = 4 x 41 = 164
  Base Percentage = (Total Score / 164) x 100

Pass/Fail with guardrails:

  IF any hard guardrail triggered:
      result = AUTO-FAIL (cite specific guardrail + evidence)
  ELSE:
      Soft Penalty     = sum(penalty_i) for each triggered soft guardrail
      Final Percentage = Base Percentage - Soft Penalty
      IF Final Percentage >= 96:  result = PASS
      ELSE:                       result = FAIL -> generate improvement plan
```

## Evaluation Matrix

### Criterion 1: Hook Strength
**Weight: 5** | *Does the opening stop the scroll and compel the viewer to keep watching?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Dead Air** | No discernible opening strategy. Starts with a logo animation, silent black screen, generic establishing shot, or corporate intro card. A viewer encountering this on a social feed would scroll past before the content begins. Zero reason to stop and watch. |
| 1 | **Weak Opener** | Starts with company name, tagline, or a slow pan across a generic scene. The content eventually arrives but there is no pattern-interrupt, no tension, no curiosity gap. Relies entirely on the viewer's pre-existing interest in the product. Would lose 80%+ of social media viewers within 2 seconds. |
| 2 | **Recognizable Hook** | Has an identifiable opening moment — a question on screen, a bold visual, or an attention-getting sound — but it is generic and interchangeable with any other SaaS ad. The hook does not connect to the specific product or audience. Viewer might pause but is not compelled to watch further. |
| 3 | **Strong Hook** | Opens with a targeted pattern-interrupt that speaks directly to the audience's pain point or aspiration within the first 2 seconds. Creates a clear curiosity gap or emotional stake. The viewer feels addressed personally and wants to see what comes next. One proven hook archetype is employed effectively (surprising statistic, provocative question, visual disruption, or identity statement). |
| 4 | **Irresistible Hook** | Opens with a pattern-interrupt that stops the scroll within 1.5 seconds AND establishes emotional stakes immediately. The hook is inseparable from the product's core value proposition — it could only belong to this specific commercial. Uses a proven hook archetype in a novel way. The viewer feels compelled to watch the next 5 seconds, and the transition from hook to content is seamless. Industry benchmark: achieves >70% 3-second view rate on social platforms. |

### Criterion 2: Problem-Solution Clarity
**Weight: 5** | *Does the viewer understand the problem being solved and why this product solves it better?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **No Problem Stated** | The video never articulates what problem it solves. It shows features, logos, or abstract imagery without connecting to any user pain point. After watching, a viewer could not explain what the product does or why they would want it. |
| 1 | **Vague Problem** | A problem is implied but never explicitly stated. The viewer has to infer the pain point from context clues. The solution is presented without contrast to the current state. Example: showing the product UI without first showing what life looks like WITHOUT the product. |
| 2 | **Generic Problem-Solution** | The problem is stated clearly but in generic terms that apply to any competitor ("save time," "reduce costs," "work smarter"). The solution is presented but the unique mechanism — WHY this product solves it differently — is missing. A competitor could swap in their logo and the ad would still work. |
| 3 | **Clear and Differentiated** | The problem is articulated in the audience's own language with specific, relatable scenarios. The solution is presented with a clear mechanism of action — the viewer understands HOW the product solves the problem, not just THAT it does. The differentiation from alternatives is implicit but present. |
| 4 | **Compelling Transformation** | The problem is framed as an urgent, emotionally resonant pain using the audience's exact vocabulary. The solution is presented as a clear before-to-after transformation with a specific, unique mechanism that no competitor can claim. The viewer thinks: "This is built for ME, and I understand exactly how it works." The problem-solution arc is complete within the first 15 seconds for short-form, first 30 seconds for long-form. |

### Criterion 3: Visual Consistency
**Weight: 4** | *Is the visual presentation cohesive, branded, and professionally produced throughout?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Visually Incoherent** | No discernible visual system. Mixed resolutions, random fonts, clashing color palettes, inconsistent aspect ratios between segments. Looks like clips from different projects spliced together. No brand presence. |
| 1 | **Inconsistent but Identifiable** | A brand identity exists (logo appears, brand colors are present) but is applied inconsistently. Font sizes vary without hierarchy, transitions are mismatched (some cuts, some dissolves, some wipes), and color grading shifts between segments. The video looks like it was made by multiple people without a style guide. |
| 2 | **Adequate Consistency** | Brand colors, fonts, and logo placement are consistent throughout. Transitions are uniform. Color grading is stable. However, the visual system is basic — it follows a template rather than having a distinctive visual identity. Professional but generic. Would not stand out from other well-produced SaaS ads. |
| 3 | **Cohesive Visual System** | Every visual element (typography, color palette, iconography, motion graphics, transitions) follows a clear, deliberate design system. The color grading creates a mood that reinforces the message. UI screenshots are properly formatted (device frames, consistent zoom levels, clean backgrounds). The video is immediately recognizable as belonging to this brand without the logo visible. |
| 4 | **Distinctive Visual Identity** | The visual system is not just consistent — it is distinctive and memorable. Every frame could be a screenshot used in marketing materials. Motion graphics, transitions, and visual metaphors reinforce the narrative arc. UI demos are polished (smooth interactions, real data, appropriate pacing). The production quality signals credibility — this looks like a company that pays attention to detail. Color grading, lighting, and composition are intentional in every shot. |

### Criterion 4: Pacing
**Weight: 4** | *Does the video maintain engagement through deliberate rhythm and timing?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Unwatchable Pacing** | Either agonizingly slow (long static shots, dead space, unnecessary pauses) or chaotically fast (information overload, cuts every 0.5 seconds with no breathing room). The viewer either loses interest or cannot process the content. Drop-off would occur before the midpoint. |
| 1 | **Monotonous or Rushed** | Single tempo throughout — no acceleration or deceleration to match the emotional arc. Information-dense sections get the same timing as emotional beats. For short-form (60s or less): either crams too much in or stretches too little. The viewer's attention drifts by the second third. |
| 2 | **Functional Pacing** | Content moves at a reasonable speed and key messages have adequate screen time. No jarring jumps or dead spots. However, the pacing is uniform — there are no deliberate tempo changes to create tension, relief, or emphasis. The video is watchable but not engaging on a rhythmic level. |
| 3 | **Deliberate Rhythm** | Pacing varies intentionally to match the content arc: quick cuts for energy during the hook, slower tempo for the problem statement, accelerating through features, breathing room before the CTA. Each message has precisely enough screen time to be absorbed without overstaying. Key information appears for the optimal duration (3-5 seconds for text overlays, 1-2 seconds per feature in a montage). |
| 4 | **Masterful Cadence** | The video has a rhythmic signature — a deliberate tempo pattern that creates anticipation and satisfaction. Beat changes align with music/audio cues. The hook-to-CTA arc follows a tension curve: build, peak, release, action. Screen time allocation is precision-optimized: nothing lingers, nothing rushes. For short-form: achieves >65% average view duration. For long-form: maintains engagement through the final CTA without significant drop-off. The pacing itself is a narrative device. |

### Criterion 5: CTA Effectiveness
**Weight: 5** | *Does the call-to-action drive the desired viewer behavior?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **No CTA** | The video ends without any call-to-action. No instruction, no link, no next step. The viewer may have been engaged but is given nothing to do with that engagement. The commercial is a dead end — attention captured, then abandoned. |
| 1 | **Passive CTA** | A CTA exists but is buried, generic, or an afterthought. "Learn more at our website" flashed for 1 second at the end. No visual emphasis, no urgency, no specificity. The CTA does not match the platform — e.g., "visit our website" on TikTok where links are not clickable in-feed. |
| 2 | **Standard CTA** | Clear, visible call-to-action that tells the viewer what to do next: "Book a demo," "Sign up free," "Learn more." Displayed with adequate duration and visual treatment. However, the CTA lacks urgency, social proof, or a compelling reason to act NOW rather than later. It is correct but not persuasive. |
| 3 | **Compelling CTA** | The CTA is specific, urgent, and directly connected to the value proposition established in the video. It reduces friction ("Free 14-day trial, no credit card") and creates mild urgency or exclusivity. The CTA appears with strong visual treatment — contrasting colors, motion, or a dedicated end card. It is platform-appropriate (swipe up on Stories, link in bio reference on Reels, direct URL on YouTube). |
| 4 | **Conversion-Optimized CTA** | The CTA is the natural, inevitable conclusion of the video's narrative arc — the viewer arrives at it with momentum. It combines specificity + urgency + social proof + friction reduction. Example: "Join 500+ dental practices using VoicesIQ — start your free trial in 60 seconds." The CTA appears at the emotional peak AND is reinforced in the end card. Multiple response channels are provided where platform allows. The CTA has been A/B tested or follows proven direct-response patterns for the target platform. |

### Criterion 6: Data Credibility
**Weight: 3** | *Are claims, statistics, and social proof accurate, sourced, and believable?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Fabricated or Unsourced Claims** | Makes specific numerical claims ("saves 40% of staff time") with no source, no basis, and no disclaimer. Uses fake testimonials, invented statistics, or presents aspirational projections as established facts. Undermines trust and creates legal liability. |
| 1 | **Vague Claims** | Uses directional claims ("save time," "increase efficiency," "better outcomes") without any quantification or evidence. No social proof, no testimonials, no case studies, no data points. The viewer has no reason to believe the claims beyond the company's own assertion. |
| 2 | **Basic Social Proof** | Includes some form of credibility signal — a testimonial quote, a customer count, a time-in-business claim — but the evidence is thin or unverifiable. Data points lack specificity or sourcing. Example: "Trusted by dental professionals" without naming any. |
| 3 | **Credible Evidence** | Claims are specific and supported by identifiable evidence: named testimonials with titles, verifiable metrics from pilots or case studies, third-party validation, or properly cited statistics. Data points include context (sample size, time frame, conditions). Claims are conservative rather than hyperbolic. |
| 4 | **Authoritative Proof** | Every claim is backed by independently verifiable evidence. Testimonials are from named individuals with verifiable credentials. Statistics cite specific studies, pilot programs with named participants, or third-party audits. Social proof is layered: customer count + named logos + specific outcome metrics + independent validation. The credibility signals are strong enough that a skeptical viewer would find them convincing. All claims include appropriate disclaimers where required. |

### Criterion 7: Technical Accuracy
**Weight: 4** | *Does the video accurately represent the product's capabilities and user experience?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Misleading Representation** | Shows capabilities the product does not have, demonstrates features that don't exist, or depicts a user experience dramatically different from reality. The product shown in the video is effectively fictional. Would generate customer complaints and refund requests. |
| 1 | **Significant Embellishment** | The product is real but the video exaggerates its capabilities — showing instant results that actually take minutes, depicting seamless workflows that actually require manual steps, or hiding known limitations. A new customer would feel misled during onboarding. |
| 2 | **Broadly Accurate** | The product's core capabilities are represented accurately, but the demo is idealized: perfect data, no edge cases, no loading times, no error states. The video shows the best-case scenario without acknowledging that real usage varies. Not misleading, but not fully honest either. |
| 3 | **Faithful Representation** | Product capabilities are shown accurately with real or realistic data. The demo flow represents actual user experience with appropriate acknowledgment of complexity (e.g., setup steps are mentioned even if not shown in detail). Limitations or conditions are stated rather than hidden. A new customer's first experience would match what the video promised. |
| 4 | **Transparent and Precise** | Every product claim is demonstrably true. UI recordings use real data (properly anonymized). Feature demonstrations could be replicated by a new user. Speed of interactions is representative of actual performance. Where the product requires setup, configuration, or has limitations, these are acknowledged transparently. The video builds trust by showing the product honestly — its strengths are compelling enough without embellishment. |

### Criterion 8: Platform Optimization
**Weight: 3** | *Is the video optimized for its target distribution platform(s)?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Platform-Ignorant** | A single video is produced with no consideration for where it will be shown. Wrong aspect ratio for the platform (horizontal video on TikTok, vertical on YouTube desktop), wrong duration (3-minute video for Instagram Reels), no platform-specific elements. |
| 1 | **Wrong Format** | The target platform is acknowledged but the video violates key platform norms. Examples: no captions on a platform where 85% watch without sound, landscape video on a vertical-first platform, exceeds platform duration limits, includes elements that get cropped by platform UI (text behind like/share buttons). |
| 2 | **Basic Compliance** | Meets the platform's technical requirements: correct aspect ratio, within duration limits, appropriate resolution. Captions included if needed. However, the video does not exploit platform-specific features or behaviors (no hooks optimized for autoplay, no use of platform-native interactions like polls or swipe-up). |
| 3 | **Platform-Aware** | Optimized for the target platform's algorithm and user behavior: safe zones respected (no content behind UI elements), hook designed for autoplay context, duration optimized for the platform's engagement curve (e.g., 15-30s for TikTok, 30-60s for Reels, 1-3min for YouTube). Text overlays are sized for mobile viewing. Thumbnail (if applicable) is designed, not auto-generated. |
| 4 | **Platform-Native** | The video feels like it was born on the target platform — it exploits platform-specific behaviors, trends, and features. For multi-platform distribution, separate cuts exist with platform-specific optimizations (not just re-cropped). The hook is optimized for autoplay + silent viewing. Duration hits the platform's algorithmic sweet spot. Engagement patterns follow platform best practices (e.g., pattern interrupt cadence on TikTok, chapter markers on YouTube). The video would perform in the platform's recommendation algorithm, not just on the brand's existing audience. |

### Criterion 9: Narration Quality
**Weight: 3** | *Does the voice, music, and audio design support the message and maintain engagement?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **Audio Absent or Harmful** | No narration, no music, no audio design — or audio that actively hurts the video: distorted voice, copyright-infringing music, jarring volume changes, echo/reverb from poor recording environment. The audio makes the video unwatchable with sound on. |
| 1 | **Amateur Audio** | Narration exists but is poorly delivered: monotone reading, unnatural pacing, incorrect pronunciation of product names or industry terms, or text-to-speech with no personality. Music is generic royalty-free that does not match the mood. Audio levels are inconsistent — narration competes with music. |
| 2 | **Competent Audio** | Clear narration with correct pronunciation and adequate delivery. Music is appropriate for the mood and properly mixed (narration sits above music). No technical audio issues. However, the narration is informational rather than persuasive — it describes what is on screen without adding emotional dimension. The audio is forgettable. |
| 3 | **Engaging Audio** | Narration is delivered with personality, appropriate emotion, and conversational tone that matches the brand. Voice talent (human or AI) sounds natural and authoritative. Music creates emotional progression that supports the narrative arc. Sound effects are used sparingly and purposefully (UI clicks, notification sounds, transitions). The audio experience enhances the video's persuasiveness. |
| 4 | **Broadcast-Quality Audio** | Every audio element is intentional and polished. Narration delivery is compelling — the voice conveys confidence, empathy, and expertise appropriate to the audience. Music is custom or carefully selected to create an emotional signature. Sound design creates atmosphere: subtle ambient layers, precision-timed stings, and audio transitions that mirror visual transitions. Audio levels are professionally mastered with proper dynamic range. The audio alone (without visuals) would communicate the brand's personality and the product's value. |

### Criterion 10: Conversion Pathway
**Weight: 5** | *Does the video create a clear, friction-free path from viewer to customer?*

| Level | Label | Description |
|-------|-------|-------------|
| 0 | **No Pathway** | The video exists in isolation. No link, no QR code, no mention of where to go, no landing page, no tracking. Even a viewer who loved the video has no actionable next step. Marketing budget spent on awareness with zero conversion infrastructure. |
| 1 | **Broken Pathway** | A pathway is mentioned but it is broken or high-friction: the URL in the video does not work, the landing page does not match the offer in the video, the sign-up process requires excessive steps, or the tracking pixels are not firing. The intent to convert exists but the execution fails. |
| 2 | **Basic Pathway** | The video points to a functional landing page or sign-up flow. The link works, the page loads, the form submits. However, there is no continuity between the video's message and the landing page — the viewer arrives and has to re-orient. No UTM parameters, no retargeting pixels, no attribution tracking. |
| 3 | **Connected Pathway** | The video's CTA connects to a landing page that continues the same narrative: same language, same visual identity, same offer. The viewer's journey feels seamless. UTM parameters are in place for attribution. The landing page is optimized for the traffic source (mobile-first for social, fast-loading, minimal form fields). Retargeting is configured for non-converters. |
| 4 | **Engineered Funnel** | The video is one component of a designed conversion system. The landing page mirrors the video's exact messaging and visual language. Multiple conversion points exist (immediate sign-up, email capture for nurture, calendar booking for high-intent). Attribution tracking provides full-funnel visibility. A/B testing infrastructure is in place. The entire path from first view to conversion is measured, optimized, and friction-minimized. Non-converters enter a retargeting sequence that references the video content. The pathway converts at or above industry benchmarks for the platform and audience segment. |

## Evaluation Process

### Step 1: Guardrail Scan (Fail-Fast)

Before scoring any positive criteria:

1. **Hard guardrails (G1-G3):** Watch/review the entire artifact looking for HIPAA violations, copyright infringement, and false medical claims. If ANY hard guardrail is triggered, stop immediately — result is AUTO-FAIL. Cite the specific guardrail, the timestamp or location of the violation, and what must be changed.

2. **Soft guardrails (G4-G7):** Scan for stock footage substitution, missing captions, placeholder content, and competitor branding. Record each triggered violation with evidence and its percentage penalty.

### Step 2: Score Each Criterion

For each of the 10 criteria:
1. Read all 5 level definitions (0-4) for that criterion
2. Watch/examine the artifact for evidence relevant to this criterion
3. Determine which level best matches — cite specific evidence (timestamps for video, locations for other formats)
4. Assign rating (0-4)
5. Calculate weighted score: Rating x Weight

### Step 3: Calculate Final Score

```
Total Score     = sum of all (Rating_i x Weight_i)
Base Percentage = (Total Score / 164) x 100
Soft Penalty    = sum of triggered soft guardrail penalties
Final Percentage = Base Percentage - Soft Penalty
```

### Step 4: Determine Result

```
IF any hard guardrail triggered:     AUTO-FAIL (cite guardrail)
ELIF Final Percentage >= 96:         PASS
ELSE:                                FAIL -> generate improvement plan
```

### Step 5: Generate Improvement Plan (if FAIL)

Priority-ordered list of improvements, sorted by impact score:
- **Guardrail fixes first** (binary fix -> immediate score recovery)
- **Criteria gaps second**, sorted by: weight x (4 - current_rating) descending
- For each item: what is wrong (evidence), what "good" looks like (target level definition), estimated score impact

## Examples

### Example: High Score — VoicesIQ Product Demo (YouTube)

**Guardrail Check:**
- G1 HIPAA Violation: CLEAR
- G2 Copyright Infringement: CLEAR
- G3 False Medical Claims: CLEAR
- G4 Stock Footage: CLEAR
- G5 Missing Captions: CLEAR
- G6 Placeholder Content: CLEAR
- G7 Competitor Branding: CLEAR

**Soft Penalty Total: 0%**

| # | Criterion | Wt | Rating | Weighted | Evidence |
|---|-----------|-----|--------|----------|----------|
| 1 | Hook Strength | 5 | 4 | 20 | Opens with "Your front desk is losing $47,000 a year in missed calls" — specific, quantified, targeted to practice owners. Pattern-interrupt within 1.2 seconds. |
| 2 | Problem-Solution Clarity | 5 | 4 | 20 | Before/after contrast: phone ringing with no one to answer vs. VoicesIQ answering with Sam's voice. Clear mechanism shown: AI answers, books appointment, confirms via text. |
| 3 | Visual Consistency | 4 | 4 | 16 | Navy/gold brand palette throughout. UI recordings use device frames. Consistent typography with clear hierarchy. Every frame is brand-identifiable. |
| 4 | Pacing | 4 | 4 | 16 | Hook (2s), problem (5s), solution demo (15s), social proof (5s), CTA (3s). Tempo accelerates through features, pauses for testimonial. Music beat-matched to cuts. |
| 5 | CTA Effectiveness | 5 | 3 | 15 | "Start your free trial at voicesiq.com" — clear and specific, but lacks urgency and social proof in the CTA itself. Good visual treatment on end card. |
| 6 | Data Credibility | 3 | 4 | 12 | "$47K" figure cited from dental practice management study. Named testimonial from Dr. Sarah Chen, DDS, Ashburn VA. "500+ practices" customer count. |
| 7 | Technical Accuracy | 4 | 4 | 16 | UI recordings are actual product with real (anonymized) data. Call flow demo is reproducible. Setup time mentioned ("live in 48 hours"). |
| 8 | Platform Optimization | 3 | 3 | 9 | YouTube-optimized: 16:9, custom thumbnail, chapter markers. However, no separate cuts for vertical platforms. |
| 9 | Narration Quality | 3 | 4 | 12 | Professional VO with conversational dental-industry tone. Music builds tension in problem section, resolves during solution. UI sound effects are subtle and purposeful. |
| 10 | Conversion Pathway | 5 | 3 | 15 | Links to landing page with matching message and visual identity. UTM tracking in place. But no retargeting configured and only one conversion point (trial sign-up). |

| Metric | Value |
|--------|-------|
| **Total Score** | **151 / 164** |
| **Base Percentage** | **92.1%** |
| **Soft Penalty** | **0%** |
| **Final Percentage** | **92.1%** |
| **Result** | **FAIL (threshold: 96%)** |

**Improvement Plan:**
1. **CTA Effectiveness** (rating 3 → target 4, impact: 5 x 1 = 5 points = +3.0%)
   - Current: Clear CTA but no urgency or social proof within the CTA itself.
   - Target: Add urgency + social proof: "Join 500+ practices — start free, cancel anytime." Reinforce at emotional peak, not just end card.
2. **Platform Optimization** (rating 3 → target 4, impact: 3 x 1 = 3 points = +1.8%)
   - Current: YouTube-only format.
   - Target: Create 9:16 vertical cut for TikTok/Reels with platform-specific hooks and safe zones.
3. **Conversion Pathway** (rating 3 → target 4, impact: 5 x 1 = 5 points = +3.0%)
   - Current: Single conversion point, no retargeting.
   - Target: Add email capture for nurture sequence, configure retargeting pixels, A/B test landing page variants.

*Fixing all three items would bring the score to 164/164 = 100% → PASS.*

### Example: Low Score — Hastily Produced Trade Show Clip

**Guardrail Check:**
- G1 HIPAA Violation: CLEAR
- G2 Copyright Infringement: CLEAR
- G3 False Medical Claims: **TRIGGERED** — "VoicesIQ guarantees 100% appointment booking accuracy"
- **Result: AUTO-FAIL (hard guardrail G3)**

*Evaluation stops at guardrail check. No positive criteria scored.*

**Even if scored, soft guardrails would have applied:**
- G5 Missing Captions: TRIGGERED (-3%)
- G6 Placeholder Content: TRIGGERED (-5%) — "[YOUR LOGO HERE]" visible at 0:22

**Improvement Plan:**
1. **[HARD GUARDRAIL] G3: False Medical Claims** — Remove "guarantees 100% appointment booking accuracy." Replace with verifiable claim: "Answers 100% of calls — no more missed opportunities." Accuracy claims about AI systems require documented evidence and disclaimers.
2. **[SOFT GUARDRAIL] G6: Placeholder Content (-5%)** — Replace all placeholder elements with finalized branding and content.
3. **[SOFT GUARDRAIL] G5: Missing Captions (-3%)** — Add burned-in captions for accessibility and silent autoplay.

*Hard guardrail must be resolved before re-evaluation. Soft guardrails should be fixed simultaneously.*

## Integration with DemoIQ

This rubric is the primary quality gate in the DemoIQ pipeline:

```
Storyboard → Content Capture → Video Forge (Veo3) → Demo Stitcher
    → [shit-commercial-development evaluation]
        |── PASS (≥96%) → Platform Export → Publish
        |── FAIL (<96%) → Improvement Plan → Revise Storyboard
                         → Re-stitch → Re-evaluate (loop until 96%)
```

**DemoIQ consumption pattern:**
1. After the stitcher produces a final video, invoke:
   `/shit-loop evaluate --rubric shit-commercial-development ./output/final.mp4`
2. If FAIL: the improvement plan feeds back into the storyboard-builder skill for targeted revisions
3. Only criteria that were weak are re-evaluated — stable criteria carry forward
4. Maximum 3 evaluation iterations before escalating to human review

**Autopilot integration:**
When Autopilot delegates a video creation task, it uses this rubric as the completion gate:
- Task is not marked "done" until `shit-commercial-development` evaluation returns PASS
- The autopilot improvement loop reads the improvement plan and re-delegates specific fixes

## Notes

- **Hook Strength and CTA Effectiveness are both weight 5** because they bookend the viewer's journey. A video without a hook is never watched; a video without a CTA is watched but wasted.
- **Conversion Pathway is weight 5** because the commercial's purpose is conversion, not just awareness. A beautiful video that does not convert is a failed commercial.
- **Data Credibility is weight 3** because not every commercial needs heavy data — some products sell on emotion, demonstration, or social proof. But when claims ARE made, they must be credible.
- **Narration Quality is weight 3** because many effective social media commercials work entirely without narration (text overlays + music). The weight reflects that narration enhances but is not essential.
- **For pre-production evaluation** (storyboard/script phase), criteria like Visual Consistency and Narration Quality should be scored against the planned approach, not actual execution. Note this in the score report.
- **Platform Optimization scoring depends on declared target platforms.** A video targeting only YouTube is not penalized for lacking vertical cuts. But a video targeting TikTok that is 16:9 without safe zones will score low.
