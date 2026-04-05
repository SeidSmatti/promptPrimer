---
type: research
description: Academic research, literature reviews, systematic reviews, rigorous synthesis with citations
workspace: research/
---

# Research — promptPrimer Type Module

<overview>
Use this module when the work demands verifiable sources, rigorous synthesis, and citation discipline. Covers literature reviews, systematic reviews, meta-analyses, research proposals, academic papers, policy research, annotated bibliographies, and landscape studies where every claim must be defensible. If the output is opinion-driven or uncited, use `writing`. If it is market or strategy advice for decision-makers, use `business`.
</overview>

<domain_best_practices>
- **Methodology before findings.** Define the search strategy, inclusion/exclusion criteria, and analysis approach BEFORE reviewing sources. Otherwise confirmation bias runs the show.
- **Replicable search.** Document databases, queries, date ranges, and filters so another researcher could reproduce the corpus.
- **Zero fabricated citations.** Every citation has verifiable metadata: authors, title, venue, year, DOI or stable identifier. No exceptions, ever.
- **Distinguish source tiers.** Peer-reviewed journal > peer-reviewed conference > preprint > grey literature > opinion/blog. Every claim carries its tier.
- **Quote with location.** Direct quotes need page numbers (or section for digital). Paraphrases attribute too.
- **Seek disconfirming evidence.** A literature review that cites only supporting work is propaganda.
- **Calibrate confidence.** Report findings with uncertainty: "strong evidence", "suggestive", "contested", "untested".
- **Separate primary from secondary.** If you cite a claim, trace to the primary source where possible.
- **Note conflicts of interest.** Funding, affiliation, known biases.
- **Follow PRISMA (or equivalent)** for systematic reviews: flow diagram, exclusion reasons, risk-of-bias assessment.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (brief memo, annotated short bibliography, quick literature scan)
```
<task-slug>/
  QUESTION.md          # Research question, scope, significance
  SOURCES.md           # Bibliography with full metadata
  NOTES.md             # Per-source annotations and key quotes
  <agent-file>
  research/            # Full texts, extracted data
```

## Tier 2 — Small (literature review, research proposal, white paper)
```
<project-slug>/
  QUESTION.md
  METHOD.md            # Search strategy, inclusion criteria, analysis plan
  SOURCES.md           # Bibliography with tier classification
  VERIFICATION_LOG.md  # Per-source DOI/metadata verification ledger (hard gate)
  NOTES.md             # Annotations with page-anchored quotes
  SYNTHESIS.md         # Thematic synthesis, gaps, contradictions
  OUTPUT_PLAN.md       # Structure of final deliverable
  <agent-file>
  research/
```

## Tier 3 — Medium/Large (systematic review, meta-analysis, dissertation-scale work)
```
<project-slug>/
  QUESTION.md          # Includes PICO or equivalent framing
  PROTOCOL.md          # Pre-registered protocol (inclusion, exclusion, analysis)
  METHOD.md            # PRISMA-compliant search + screening + extraction
  SOURCES.md           # Full corpus with screening decisions
  VERIFICATION_LOG.md  # Per-source DOI/metadata verification ledger (hard gate)
  EXTRACTION.md        # Data extraction template and results
  NOTES.md
  SYNTHESIS.md         # Thematic and quantitative synthesis
  BIAS.md              # Risk-of-bias assessment per source
  OUTPUT_PLAN.md
  <agent-file>
  research/
```

</tiers>

<artifact_specs>

**QUESTION.md**: primary research question, sub-questions, scope (what's in and out), significance, definitions of key terms, target audience.

**METHOD.md**: databases/sources, search strings, date range, language filters, inclusion criteria, exclusion criteria, screening process, extraction fields, analysis approach, citation style.

**PROTOCOL.md** (Tier 3): pre-registered version of METHOD.md that does not change mid-study. Deviations logged explicitly with reasons.

**SOURCES.md**: full bibliography entries in the declared citation style. Each entry: full citation, DOI/URL, source tier, why included, status (cited / read / screened / excluded-with-reason), and a pointer to its VERIFICATION_LOG.md entry.

**VERIFICATION_LOG.md** (Tier 2+): per-source DOI and metadata verification ledger. This is the operational enforcement of the "zero fabricated citations" rule. Schema per source: source_id | DOI | author(s) | title | venue | year | verified_at | verifier_method | verified_against_url | status. The verification procedure is: (1) resolve the DOI on doi.org or crossref.org, (2) compare every metadata field character-for-character against the resolved record, (3) if any field disagrees, mark the source red and either correct or remove. No source may be cited in NOTES.md, SYNTHESIS.md, or OUTPUT_PLAN.md until its VERIFICATION_LOG.md row is green. This is a hard gate, not a soft suggestion.

**EXTRACTION.md** (Tier 3): per-source data extraction — study design, sample, intervention/variable, outcome, effect size, risk of bias.

**NOTES.md**: per-source structured notes — key claims with page numbers, direct quotes with page numbers, methodology summary, relevance, critique.

**SYNTHESIS.md**: themes that emerged, contradictions between sources, confidence levels for each finding, identified gaps, implications.

**BIAS.md** (Tier 3): risk-of-bias assessment per source using a named tool (Cochrane, ROBINS-I, or equivalent).

**OUTPUT_PLAN.md**: structure of final deliverable, section-by-section, with which sources support which claim.

**Nested agent-instruction file**: mandates never fabricating citations; requires verifying every citation against a real source before adding to SOURCES.md; requires page numbers for quotes; forbids overclaiming.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **Zero fabricated citations.** A citation without verifiable metadata is deleted, not guessed. Never invent authors, titles, venues, years, or DOIs.
- **No invented page numbers.** If the page isn't known, mark it unknown and flag for verification.
- **No paraphrase-as-quote.** Direct quotes must match the source character-for-character.
- **No overclaiming.** One study ≠ "research shows". Report what the evidence supports.
- **No cherry-picking.** Disconfirming sources must be included and addressed.
- **No citation laundering.** Don't cite a secondary source for a primary claim; find the original.
- **No post-hoc methodology.** Methods are defined before analysis, not adjusted to fit findings.
- **No confidence without calibration.** State uncertainty explicitly.
- **No silent scope drift.** If the research question shifts, document why in METHOD.md.
- **No conflation of correlation and causation.**
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Never add a citation to SOURCES.md without first adding a green VERIFICATION_LOG.md row confirming every metadata field against the resolved DOI
- Store or link the actual PDFs/full texts in `research/`
- Include page numbers with every quote and paraphrase
- Flag suspected fabricated citations from upstream sources immediately
- Update SYNTHESIS.md only after NOTES.md is complete for a source
- Ask before changing METHOD.md mid-review
- State confidence level for every finding
</nested_agent_file_directives>
