---
type: business
description: Strategy, market analysis, business plans, memos, pitch content, competitive analysis
workspace: deliverables/
---

# Business — promptPrimer Type Module

<overview>
Use this module for strategic, analytical, or decision-support work aimed at business stakeholders: strategy memos, business plans, market analyses, competitive landscapes, go-to-market plans, pitch narratives, investor updates, operational reviews. If the work requires peer-reviewed citations, use `research`. If it is customer-facing marketing prose, use `writing`.
</overview>

<domain_best_practices>
- **Pyramid principle.** Lead with the answer. Support it with grouped, MECE arguments. Details last.
- **Name the decision.** What decision does this enable, by whom, by when? If there's no decision, there's no point.
- **Audience frame determines format.** Exec gets a one-pager. Board gets a deck. Operator gets a runbook. Investor gets a narrative.
- **Assumptions are labeled as assumptions.** Fake precision ("$4.27B TAM") without sourcing is worse than honest ranges.
- **Risks are named, not buried.** Three biggest risks, each with a mitigation or an accepted tradeoff.
- **MECE categorization.** Options, segments, and drivers are mutually exclusive and collectively exhaustive.
- **Quantify when possible.** "Faster" is weaker than "12 min → 3 min per task × 40 reps/week".
- **Cite sources for external data.** Market stats, competitor pricing, regulatory facts — all sourced.
- **Jargon earns its place.** Every buzzword should make the communication more precise, not less.
- **Options before recommendations.** Show the shortlist considered, then the choice and why.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single memo, decision brief, short analysis)
```
<task-slug>/
  CONTEXT.md           # Situation, decision, stakeholder, deadline
  ANALYSIS.md          # Key findings with evidence
  RECOMMENDATION.md    # Recommendation + rationale + risks
  <agent-file>
  deliverables/
```

## Tier 2 — Small (business plan section, competitive analysis, go-to-market plan)
```
<project-slug>/
  CONTEXT.md
  RESEARCH_NOTES.md    # Inputs, data, sources
  ANALYSIS.md          # Frameworks applied with evidence
  OPTIONS.md           # Alternatives considered with tradeoffs
  RECOMMENDATION.md
  DELIVERABLE_PLAN.md  # Final output structure
  <agent-file>
  deliverables/
```

## Tier 3 — Medium/Large (full business plan, multi-market strategy, due diligence)
```
<project-slug>/
  CONTEXT.md
  STAKEHOLDER_MAP.md   # Decision-makers, influencers, their frames
  RESEARCH_NOTES.md    # Structured inputs with citations
  ANALYSIS.md          # Multi-framework (SWOT / Porter / JTBD / etc.)
  FINANCIAL_MODEL.md   # Assumptions, sensitivity, scenarios
  OPTIONS.md
  RISK_REGISTER.md     # Full risk inventory with mitigations
  RECOMMENDATION.md
  DELIVERABLE_PLAN.md
  <agent-file>
  deliverables/
```

</tiers>

<artifact_specs>

**CONTEXT.md**: situation in 3–5 sentences, the specific decision being made, decision-maker, deadline, constraints, prior work.

**STAKEHOLDER_MAP.md** (Tier 3): named stakeholders with role, stake, frame (what they care about), and how to communicate with them.

**RESEARCH_NOTES.md**: structured inputs — market data, customer interviews, competitive info, internal data — each with source and date.

**ANALYSIS.md**: which frameworks apply and why (SWOT, Porter's Five Forces, JTBD, Blue Ocean, value chain — whichever fit), findings with evidence. Never framework for its own sake.

**FINANCIAL_MODEL.md** (Tier 3): assumptions (clearly flagged), revenue build, cost build, scenarios (conservative / base / aggressive), sensitivity to key variables.

**OPTIONS.md**: 2–4 shortlisted options, each with description, pros, cons, cost, risk, fit to constraints. No straw men.

**RISK_REGISTER.md** (Tier 3): risks categorized (market / execution / financial / regulatory / people), likelihood × impact, mitigation owner.

**RECOMMENDATION.md**: the recommendation as one sentence **on line 1, verbatim, before anything else** — no heading, no preamble, no hedge. Rationale in 3–5 bullets immediately below. Then top risks with mitigations, expected outcome, and the decision required. Enforce the **answer-first test**: if line 1 is anything other than the recommendation itself, the file fails and is rewritten.

Passing example (line 1): `Launch in the EU in Q3 2027, starting with Germany and the Netherlands only.`

Failing example (line 1): `## Recommendation` — because the recommendation is hidden behind a heading.

Failing example (line 1): `After considering the EU market opportunity and the associated risks, we recommend...` — because the answer is buried in preamble.

**DELIVERABLE_PLAN.md**: format (memo / deck / plan), length, section map, key visuals.

**Nested agent-instruction file**: mandates pyramid-principle structure; forbids fabricated market stats; requires citations for external data; enforces decision-oriented framing.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **Never fabricate market statistics or competitor data.** If it's not sourced, it's flagged as assumption or cut.
- **Never present a framework without findings.** SWOT with generic quadrants is noise.
- **Never bury the recommendation.** Answer first, evidence after.
- **Never offer false symmetry.** If option B is clearly worse, say so; don't pad a fake horse race.
- **Never confuse activity with outcome.** "Launched 5 initiatives" ≠ progress.
- **Never use ranges to hide that you don't know.** Say you don't know and what would resolve it.
- **Never skip risks to sell harder.** Unstated risks become surprises.
- **Never drown in buzzwords.** Define or delete every jargon term.
- **Never confuse correlation with causation** in analysis.
- **Never cite a TAM without showing its build.**
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read CONTEXT.md before any session to anchor on the decision at hand
- Cite every external data point in RESEARCH_NOTES.md
- Surface assumptions explicitly, never bury them
- Write in pyramid-principle order: answer → arguments → evidence
- Include top 3 risks with mitigations in every recommendation
- Stop and ask when the decision frame is ambiguous
</nested_agent_file_directives>
