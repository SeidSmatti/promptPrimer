---
type: documentation
description: Technical docs, user guides, tutorials, reference manuals, knowledge bases
workspace: docs/
---

# Documentation — promptPrimer Type Module

<overview>
Use this module when the deliverable is user-facing documentation for a product, API, library, system, or process. Covers tutorials, how-to guides, reference, and explanation (the Diátaxis four quadrants), plus README files, runbooks, and onboarding materials. If the output is a marketing blog post, use `writing`. If it is an academic synthesis, use `research`.
</overview>

<domain_best_practices>
- **Diátaxis quadrants are distinct.** Tutorials (learning), how-tos (task completion), reference (lookup), explanation (understanding). Mixing them on one page produces a document that serves no one.
- **Task-oriented, not feature-oriented.** Users arrive with a goal. Organize by what they want to do.
- **Verify against source.** Every documented flag, endpoint, parameter, behavior must match the current code or product. Invented APIs destroy trust instantly.
- **Runnable examples, tested examples.** Every code snippet executes and produces the shown output.
- **Progressive disclosure.** Simple case first, edge cases later, advanced configuration last.
- **Define terms on first use.** Every piece of jargon gets a definition or a link.
- **Active voice, imperative mood** for instructions. "Run `foo`", not "The command `foo` can be run".
- **Audience-calibrated prerequisites.** State what the reader must know upfront.
- **Findability matters.** Headings are search targets. Write them as questions or tasks.
- **Banned words: "simply", "just", "easy", "obvious", "trivial", "of course".** They are condescending and usually wrong.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (README, single runbook, one-page guide)
```
<doc-slug>/
  DOC_BRIEF.md         # Audience, prerequisites, goal, scope
  SOURCE_CHECKLIST.md  # What to verify against the product
  <agent-file>
  docs/
```

## Tier 2 — Small (product docs section, tutorial series, API reference for a small library)
```
<project-slug>/
  DOC_BRIEF.md
  IA.md                # Information architecture, page map
  CONVENTIONS.md       # Terminology, voice, example style
  SOURCE_CHECKLIST.md  # Features/APIs to verify
  DRAFTLOG.md          # Drafting and review log
  <agent-file>
  docs/
```

## Tier 3 — Medium/Large (full product documentation set, multi-audience docs portal)
```
<project-slug>/
  DOC_BRIEF.md         # Per-audience briefs
  IA.md                # Full site map with Diátaxis quadrants marked
  PERSONAS.md          # Reader personas with goals
  CONVENTIONS.md       # Terminology, voice, code style
  STYLE_GUIDE.md       # Expanded voice and formatting rules
  SOURCE_CHECKLIST.md
  REVIEW_PLAN.md       # Who reviews what (SMEs, editors)
  DRAFTLOG.md
  <agent-file>
  docs/
```

</tiers>

<artifact_specs>

**DOC_BRIEF.md**: target reader, prerequisites, what the reader achieves after reading, scope, non-goals, success criteria ("reader can complete X without asking for help").

**IA.md**: page map organized by Diátaxis quadrant. Each page carries an **explicit Diátaxis quadrant tag** (`tutorial` | `how-to` | `reference` | `explanation`), audience, one-sentence purpose, prerequisites, linked-from and links-to. **Hard rule: no page may mix quadrants.** Include this inline cheat sheet verbatim at the top of IA.md so no external lookup is needed:
> - **Tutorial** (learning-oriented): holds the reader's hand through a first experience; optimized for discovery and confidence, not efficiency.
> - **How-to** (task-oriented): steps to accomplish a specific real-world goal for someone who already knows the basics.
> - **Reference** (information-oriented): accurate, complete, dry lookup. Describes the machinery.
> - **Explanation** (understanding-oriented): context, concepts, design rationale. Why it works this way.
>
> If a page "feels like it could be two quadrants", split it into two pages.

**PERSONAS.md** (Tier 3): reader personas — role, goals, pain points, prior knowledge, context of use.

**CONVENTIONS.md**: terminology glossary (preferred and forbidden terms), voice (direct, active, imperative), code-example style (language, formatting, length), heading style, screenshot requirements.

**STYLE_GUIDE.md** (Tier 3): expanded CONVENTIONS.md with sentence length, list usage rules, tense, UI-element formatting, admonition/callout usage.

**SOURCE_CHECKLIST.md**: enumerated features, APIs, flags, behaviors, error messages that the docs must verify against the actual product. Each item carries a last-verified date.

**REVIEW_PLAN.md** (Tier 3): per-section reviewer assignment (SME for accuracy, editor for voice), sign-off criteria.

**DRAFTLOG.md**: date, page, status, reviewer notes, next action.

**Nested agent-instruction file**: mandates verifying every technical claim against source before writing; forbids inventing flags or behaviors; requires running every code example; enforces CONVENTIONS.md.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **Never document behavior that isn't verified in the source.** Read the code or test the product.
- **Never invent flags, endpoints, parameters, error messages, or defaults.**
- **Never include unrun code examples.** Execute every snippet and paste actual output.
- **Never mix Diátaxis quadrants on one page.** Tutorial ≠ reference ≠ how-to ≠ explanation.
- **Never use "simply", "just", "easy", "obvious", "trivial", "of course", "it goes without saying".**
- **Never assume reader context beyond stated prerequisites.**
- **Never drop definitions.** First use gets defined or linked.
- **Never reference removed features.** Date-stamp verification.
- **Never use screenshot stand-ins.** Real screenshots or none.
- **Never use passive voice for instructions.** "Run the command", not "The command should be run".
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read DOC_BRIEF.md and CONVENTIONS.md at the start of every session
- Verify every technical claim against the actual product before writing
- Execute every code example and paste actual output
- Tag every verified statement with a verification date in SOURCE_CHECKLIST.md
- Never use banned words
- Ask rather than guess when source behavior is unclear
</nested_agent_file_directives>
