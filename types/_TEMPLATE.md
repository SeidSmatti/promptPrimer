---
type: <your_type_name>
description: <One-line description of what this type covers and the deliverable kind it produces>
workspace: <directory-name>/
---

# <Type Name> — promptPrimer Type Module

<!--
This file is a template for creating a new type module. See
`knowledge/new_type_workflow.md` for the full workflow including
the mandatory A/B testing step before contribution.

Rules when filling this in:
- Every practice, artifact spec, and failure mode must be CONCRETE and enforceable.
  "Write clearly" is not a practice; "commit to POV in CONCEPT.md, reject generic
  adjectives like 'innovative'" is a practice.
- Anchor domain best practices to NAMED established frameworks where possible
  (Diátaxis, Bloom's taxonomy, pyramid principle, PRISMA, backward design, etc.).
- Every tier's scaffold is 3–6 files. Tier 1 is the tiniest version; Tier 3 is
  medium-to-large. Do not force all types to have Tier 3 — if a domain rarely
  goes large, make Tier 3 optional and say so in the overview.
- Do NOT include STATE.md or autonomy/context-preservation blocks — those are
  applied universally by the orchestrator from knowledge/guardrails.md.
- Keep the file to ~100–140 lines total for consistency with existing modules.
-->

<overview>
[One paragraph. What this type covers, what deliverables it produces, and when
to pick it over adjacent types. Include at least one explicit disambiguation:
"If <X>, use `<other_type>` instead."]
</overview>

<domain_best_practices>
[8–12 bulleted best practices. Each bullet is a concrete, actionable directive
grounded in a named framework or well-known principle. Every bullet is
enforceable — a reader can tell whether an output follows it or violates it.]

- **<Principle 1 name>.** <Concrete rule + one-sentence rationale.>
- **<Principle 2 name>.** <...>
- **<Principle 3 name>.** <...>
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (<one-line scope for smallest reasonable task in this domain>)
```
<task-slug>/
  FILE1.md             # <one-line purpose>
  FILE2.md             # <one-line purpose>
  <agent-file>
  <workspace>/
```

## Tier 2 — Small (<scope of small task in this domain>)
```
<project-slug>/
  FILE1.md
  FILE2.md
  FILE3.md
  FILE4.md
  <agent-file>
  <workspace>/
```

## Tier 3 — Medium/Large (<scope of large task in this domain; omit if domain rarely goes large>)
```
<project-slug>/
  FILE1.md
  FILE2.md
  ...
  <agent-file>
  <workspace>/
```

Pick the lowest tier that fits. When unsure between tiers, pick the smaller one.

</tiers>

<artifact_specs>

[For every file named in any tier scaffold, provide a concrete content spec.
Each spec should make it obvious what goes in the file and what counts as
complete. Include inline examples, ban lists, or formal schemas where they
would sharpen enforceability.]

**FILE1.md**: <what it contains — fields, sections, requirements. Be specific.>

**FILE2.md**: <...>

**Nested agent-instruction file**: <3–5 directives the nested CLAUDE.md /
GEMINI.md / AGENTS.md inside the generated project must tell the downstream
agent about this domain.>

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:

[The concrete failure modes specific to this domain. These become "never do X"
rules baked into every generated prompt of this type. Target 8–10 bullets. Each
should be specific enough that a reader understands exactly what counts as a
violation. The best ones reference a real failure pattern, not a vague caution.]

- **Never <specific bad thing>.** <Why it's a failure mode.>
- **Never <specific bad thing>.** <...>
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:

[3–6 domain-specific rituals the downstream agent follows. These go into the
generated project's nested agent file. Usually: what to read at session start,
what to update during work, what hard gates to respect, what to ask before
doing.]

- Read <file> before <action>
- <Domain-specific discipline that must survive session boundaries>
- Log <X> to <file> whenever <Y>
</nested_agent_file_directives>
