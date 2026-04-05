# Adding a New Task Type to promptPrimer — Agent Workflow

**This file is read only when the user explicitly asks to add a new task type.** It is not part of the normal prompt-generation workflow. Do not read it for routine prompt generation. If the user asks something like "add a new type for X" or "I want promptPrimer to support Y-type tasks", read this file in full and follow the workflow.

Your role in that moment changes from "prompt generator" to "type module author". You will research the domain, draft the module, A/B test it against the baseline behavior, and prepare a contribution-ready PR bundle. You will not merge the type yourself — the final integration requires explicit user confirmation.

## 1. Decide whether a new type is actually warranted

Before writing anything, check whether the proposed domain is genuinely distinct from the nine existing types, or whether it is a special case of one of them. Most candidate "new types" are better handled by extending an existing module.

Ask yourself:
- Is the deliverable fundamentally different from the deliverables of all 9 existing types? (Code, data, prose, rigorous synthesis, user-facing docs, strategic memos, learning materials, creative concepts, fallback general tasks.)
- Does the domain have its own established best-practice frameworks distinct from the existing ones?
- Do the domain's failure modes differ from those already covered?
- If this type did not exist, would `general` be materially inadequate?

If the answer to any of those is "no", recommend to the user that they extend an existing type instead of adding a new one. Only proceed to step 2 if the new type genuinely expands the space.

Common cases that **do not** warrant a new type:
- "Marketing copy" — use `writing`.
- "Technical blog posts" — use `documentation` or `writing` depending on whether it's reference or editorial.
- "Data visualization" — use `data` with a visualization deliverable specified in BRIEF.md.
- "Academic papers that include data analysis" — use `research` with data captured in NOTES.md or EXTRACTION.md.
- "Game design with narrative" — use `creative` with MECHANICS.md + narrative bible.

Cases that **might** warrant a new type:
- Legal/compliance work (contracts, policies, regulatory filings) — distinct failure modes around citation accuracy and jurisdictional scope.
- Music composition or sound design — distinct deliverable form and evaluation criteria.
- Scientific computing / computational modeling — where the deliverable is a validated model, not code or data.
- Translation and localization — distinct failure modes around equivalence and cultural adaptation.

If unsure, batch a single AskUserQuestion asking the user to confirm the new type is warranted and to name one established framework or discipline they think anchors it. Do not proceed without that confirmation.

## 2. Research the domain

Once the type is approved, research the domain before drafting. You are looking for three things:

**Framework anchoring.** Identify 1–3 named, well-established frameworks or methodologies from the domain. Every type module in promptPrimer is anchored to named frameworks — Diátaxis for documentation, Bloom's taxonomy and backward design for education, PRISMA for research, pyramid principle for business, schema-first practices for data, and so on. A type module without framework anchoring produces vague best practices.

Examples of framework anchoring you would search for:
- For legal work: IRAC framework, plain-language drafting principles, citation formats (Bluebook, OSCOLA).
- For music: song form conventions, mix-reference methodology, A/B/A song structure.
- For scientific computing: reproducibility principles (runnable papers), TRACE guidelines, Sandve et al. rules.
- For translation: skopos theory, dynamic vs. formal equivalence, localization QA standards.

**Primary failure modes.** What are the 8–10 most common, specific ways work in this domain goes wrong? Not vague cautions ("be careful with X") but concrete anti-patterns a reader could point at. Examples from existing modules:
- research: "never fabricate citations", "never invent page numbers", "never paraphrase-as-quote".
- data: "never fabricate field values", "never silently coerce types", "never scale before sampling".
- writing: "no AI-slop phrases", "no tense drift", "no POV slippage".

**Artifact conventions.** What planning artifacts are standard in the domain? A literature review has a QUESTION / METHOD / SOURCES / NOTES / SYNTHESIS structure because that's how reviews are normally organized. A curriculum has OUTCOMES / CURRICULUM / ASSESSMENT because that's backward design. Find the natural artifact set for your domain before inventing one.

Do this research using whatever sources are available to you: web search, domain references you already know, documents the user provides. Document 2–3 sentences of notes on each framework and each primary source — this becomes part of the PR evidence.

## 3. Draft the module using the template

Copy `types/_TEMPLATE.md` to `types/<your_type_name>.md` and fill in every section. Rules:

- **Concrete > abstract.** "Write clearly" is not a best practice. "Commit to POV in CONCEPT.md, reject generic adjectives like 'innovative', 'dynamic', 'bold'" is a best practice.
- **Anchor to frameworks.** Every best-practice bullet should trace to a named framework from step 2 where possible.
- **Tier sizing.** Tier 1 is the tiniest reasonable task in the domain. Tier 3 is medium-to-large. Not every domain needs Tier 3 — if tasks in this domain rarely go large, mark Tier 3 as optional in the overview and still define it for completeness.
- **Artifact specs are enforceable.** A reader should be able to tell from the spec whether a produced file is complete or incomplete. Include ban lists, inline schemas, or example rows where they sharpen enforceability (see `types/writing.md` STYLE.md spec or `types/research.md` VERIFICATION_LOG.md spec for good examples).
- **Failure modes are specific.** "Never <generic bad thing>" is weak. "Never fabricate DOIs" is specific. "Never grade what wasn't taught" is specific.
- **No STATE.md, no guardrails blocks.** Those are applied universally by the orchestrator from `knowledge/guardrails.md`. Do not duplicate them in your type module.
- **Length: ~100–140 lines.** Comparable to existing modules. Shorter is fine if the domain is tight; longer is a smell.

## 4. A/B test the module (MANDATORY)

No type module ships without empirical evidence that it produces better scaffolds than the `general` fallback baseline. This is the same rubric phase 2 of promptPrimer's optimization used, scaled down for a single type.

### 4.1 Pick 3 dummy subjects

Choose three realistic user inputs for the new type. They should span the complexity range (one tiny, one small, one medium/large). Write them as a user would — scrambled, informal, missing details. Add pre-filled clarifying-question answers for each so producer subagents don't need to ask.

Save to `opti/new_type_<name>/dummy_subjects.md`.

### 4.2 Set up three conditions

For each dummy subject, run three conditions:
- **Baseline (general)** — the dummy subject goes through the existing `types/general.md` module. This is the "what we have today" reference point.
- **Simple** — stripped prompt with no type module, no guardrails. Same ablation as phase 2 of the main optimization. This is the floor.
- **New type** — the dummy subject goes through your new `types/<your_type>.md` module. This is what you are proposing.

Generate a blind label mapping (A/B/C) per dummy subject so evaluators are blind to conditions. Save to `opti/new_type_<name>/unblinding.md`.

### 4.3 Produce 9 scaffolds (3 subjects × 3 conditions)

Spawn 3 producer subagents (one per dummy subject). Each producer builds 3 scaffolds for its subject, one per condition, using the blind labels. Each scaffold lives in `opti/new_type_<name>/runs/subject_<N>/<label>/scaffold/` with a `LOG.md` next to it following the log schema from `opti/templates/log_schema.md` in the main optimization folder.

Producer prompt template is at `opti/templates/producer_brief.md`. Adapt it for a 3-condition type-creation A/B test.

### 4.4 Blind evaluate

Spawn 3 evaluator subagents (one per dummy subject). Each evaluator reads its 3 blind-labeled scaffolds, applies the 5-criterion rubric from `opti/templates/rubric.md`, writes scores and rationales to `opti/new_type_<name>/evaluations/subject_<N>.md`, and does not know which scaffold corresponds to which condition.

### 4.5 Unblind and synthesize

Join the evaluations with the unblinding table. Compute:
- Mean score per condition across the 3 subjects
- Per-subject winner
- Directional evidence: does the new type beat `general` in every subject, or only some?

Write findings to `opti/new_type_<name>/synthesis.md`.

### 4.6 Decision rule

The new type module is **ready to submit** if and only if:
1. Its mean score is at least **+1 point** over the `general` baseline across the 3 subjects (out of 15).
2. It wins or ties `general` in at least **2 of 3** subjects.
3. Its mean score is at least **+3 points** over the `simple` ablation (confirms it adds value beyond the floor).

If any of these fail, do not submit. Either refine the module and re-test, or recommend that the user use `general` or the closest existing type instead of adding a new one.

### 4.7 Auto-critique

Write an honest `opti/new_type_<name>/auto_critique.md` answering:
- Is the sample size (n=3) enough to trust conclusions?
- Could evaluator bias have shaped the result?
- Were the dummy subjects representative of what a real user would send?
- What would a larger experiment test differently?

This critique is non-optional. Acknowledging weakness is what separates evidence from theater.

## 5. Integrate into the orchestrator

Only if step 4 produced directional wins:

1. Add a row to the "Supported Task Types" table in `CLAUDE.md`, `GEMINI.md`, and `AGENTS.md` (all three — they are identical). Include disambiguation heuristics against the nearest adjacent types.
2. Update the table in `README.md` under "Supported task types".
3. If your type introduces a distinct workspace directory name (not `project/`, `data/`, `drafts/`, `research/`, `docs/`, `deliverables/`, `materials/`, `work/`, or `workspace/`), document it in the frontmatter of your type file.

Do not touch `knowledge/guardrails.md` — the universal autonomy and context-preservation blocks are not type-specific.

## 6. Prepare the PR bundle

Your contribution must include:
- `types/<your_type>.md` — the new module
- Updates to `CLAUDE.md`, `GEMINI.md`, `AGENTS.md`, `README.md` — the integration
- `opti/new_type_<name>/` — the full A/B test audit trail: dummy subjects, unblinding table, producer scaffolds, LOGs, blind evaluations, synthesis, auto-critique
- A PR description filled in against `.github/PULL_REQUEST_TEMPLATE.md`, specifically the A/B testing evidence section, linking to your synthesis file and showing the mean score differential

The PR cannot be merged without the `opti/new_type_<name>/` evidence folder. This is the contribution contract: every type module ships with empirical justification.

## 7. Stop and hand off

At this point, your role ends. Do not merge, do not push, do not open the PR yourself unless the user asks you to. Present the PR bundle to the user with a one-paragraph summary of:
- What type was added
- What frameworks it anchors to
- The A/B test mean-score result vs. `general`
- Any auto-critique caveats the user should weigh

The user decides whether to submit.

## Reference: the promptPrimer optimization experiment that validated this approach

The nine existing type modules were refined through a controlled experiment with 9 units, 3 conditions per unit, 27 producer scaffolds, and 9 blind evaluators. 8 of 9 targeted augmentations won or tied baseline; 1 was rejected because the experiment showed it hurt. The full audit trail is in `opti/` (plan, dummy subjects, unblinding table, producer LOGs, blind evaluations, synthesis, auto-critique, changelog). Read `opti/synthesis.md` and `opti/auto_critique.md` if you want to see what a finished A/B test bundle looks like — they are the reference shape for your own.
