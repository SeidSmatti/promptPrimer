---
type: data
description: Data extraction, scraping, ETL, cleaning, transformation, analysis, and reporting
workspace: data/
---

# Data — PromptGen Type Module

<overview>
Use this module when the deliverable is data itself — extracted, cleaned, transformed, analyzed, or reported. Covers web scraping, document extraction, API harvesting, ETL pipelines, dataset cleaning, statistical analysis, and data-driven reports. If substantial code is required but the output is data, use this type. If the output is a reusable tool or library, use `coding`. If the output is a stakeholder-facing strategic memo, use `business`.
</overview>

<domain_best_practices>
- **Schema-first.** Define target shape, field types, units, and required fields before extraction runs. Extraction without a schema produces unreconcilable junk.
- **Preserve provenance.** Every record carries its source identifier, extraction timestamp, and extraction method. Lost provenance = unverifiable data.
- **Sample before scaling.** Extract and validate a small sample manually before running at full volume. Surprise quirks surface cheaply at N=20.
- **Idempotent and resumable.** Pipelines must survive interruption and rerun without corrupting state.
- **Null over guess.** When a field cannot be extracted reliably, record null with a reason, never a fabricated value.
- **Validate at ingest and egress.** Type checks, range checks, required-field checks — at both ends of every pipeline stage.
- **Respect source policies.** robots.txt, ToS, rate limits, authentication, attribution requirements. Document exceptions explicitly.
- **Separate raw from derived.** Keep raw extractions immutable. Transformations produce new files.
- **Data quality > data quantity.** 10k clean records beat 100k dirty ones.
- **Explain assumptions in analysis.** Every statistical test carries its assumptions; if the assumptions don't hold, neither does the test.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (one-off extraction from a single source, small dataset)
```
<task-slug>/
  BRIEF.md             # What data, from where, why, acceptance criteria
  SCHEMA.md            # Target fields with types and definitions
  <agent-file>
  data/                # raw/ and clean/ subfolders created by the agent
```

## Tier 2 — Small (multi-source extraction or small pipeline with analysis)
```
<project-slug>/
  BRIEF.md             # Goal, downstream consumer, deliverable
  SOURCES.md           # Sources with access, quirks, policies
  SCHEMA.md            # Target schema with provenance fields
  PIPELINE.md          # Ordered steps with checkpoints
  QUALITY.md           # Validation rules and acceptance criteria
  ISSUELOG.md          # Known data issues and decisions
  <agent-file>
  data/
```

## Tier 3 — Medium/Large (production pipeline, multi-stage ETL, longitudinal analysis)
```
<project-slug>/
  BRIEF.md
  SOURCES.md
  SCHEMA.md            # Includes dimensional modeling where relevant
  PIPELINE.md          # Full DAG with idempotency and recovery plans
  QUALITY.md           # Validation + reconciliation + sampling strategy
  ANALYSIS.md          # Analytical approach, statistical methods, assumptions
  ISSUELOG.md
  <agent-file>
  data/
```

</tiers>

<artifact_specs>

**BRIEF.md**: project name, purpose, downstream consumer, deliverable format, acceptance criteria, out of scope.

**SOURCES.md**: for each source — URL/location, access method, authentication, rate limits, quirks, attribution, policy constraints, last-verified date.

**SCHEMA.md**: for each entity — field name, type, units, nullable, constraints, definition, example value. Include provenance fields (`source_id`, `extracted_at`, `extractor_version`).

**PIPELINE.md**: ordered stages (extract → validate → transform → load). Each stage: inputs, outputs, failure mode, retry policy, idempotency key. Explicit checkpoints.

**QUALITY.md**: validation rules per field, acceptance thresholds, sampling plan for manual spot-check, reconciliation procedure, what to do when quality fails.

**ANALYSIS.md** (Tier 3): question → method → data → assumptions → limitations. Document the stats or modeling approach before running it.

**ISSUELOG.md**: template with ID, date, source, description, decision made, impact on output. Starts empty.

**Nested agent-instruction file**: references the other docs; defines workspace layout (`data/raw/`, `data/clean/`, `data/derived/`); mandates reading SCHEMA.md before extracting, QUALITY.md before accepting output, ISSUELOG.md entry for every anomaly.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **Never fabricate field values.** Null + reason > guessed value.
- **Never silently coerce types.** A string where a number was expected is an error, not a cast.
- **Never skip provenance.** Every record gets source_id + extracted_at at minimum.
- **Never scrape without checking policy.** robots.txt and ToS first.
- **Never run at scale before sampling.** Validate N=20 manually first.
- **Never mutate raw data.** Transformations create new files.
- **Never claim extraction success on partial failure.** Report counts honestly (attempted / extracted / validated / accepted / rejected).
- **Never use a library function you haven't verified** handles the data's edge cases (unicode, timezones, nulls, encoding, huge values).
- **Never cite a statistical method whose assumptions haven't been checked** against the data.
- **Never compare incompatible units.** Unit fields exist for a reason.
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read SCHEMA.md and QUALITY.md before any extraction work
- Always write to `data/raw/` for extractions, never overwrite
- Log every unexpected data shape or failure to ISSUELOG.md
- Sample-validate before scaling up
- Report counts (attempted / extracted / validated / accepted / rejected) in every session summary
- Ask the user before making schema decisions that affect downstream consumers
</nested_agent_file_directives>
