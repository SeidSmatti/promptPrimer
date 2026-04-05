# Contributing to promptPrimer

Contributions are welcome. promptPrimer is designed to be extended — new task types, new harness support, improved type modules, and sharper guardrails are all in scope. This document explains how to contribute in a way that matches the quality bar of the existing repo.

## Types of contributions

### 1. New task type modules
Adding a new type (e.g., legal, music, scientific computing, translation) is a substantial contribution. It requires:
- Domain research with framework anchoring
- A filled-out module following `types/_TEMPLATE.md`
- **Mandatory A/B test evidence** showing the new type beats the `general` fallback on at least 3 realistic dummy subjects
- Integration into the orchestrator files and README

The full workflow is documented in [`knowledge/new_type_workflow.md`](knowledge/new_type_workflow.md). Read that file before starting. You can also ask an agent running inside the promptPrimer repo to add a new type for you — the orchestrator files reference the workflow file and the agent will follow it.

### 2. Augmentations to existing type modules
Proposing a change to an existing type module (new failure mode, new artifact, stricter enforcement, removed bloat) also requires A/B test evidence. Use the same experimental pattern as `knowledge/new_type_workflow.md` but apply it to a single existing type. Minimum evidence:
- 3 dummy subjects
- 3 conditions (current module as baseline, simple ablation, your proposed augmentation)
- Blind evaluator scoring on the 5-criterion rubric at `opti/templates/rubric.md`
- Your augmentation must **win or tie** the baseline on at least 2 of 3 subjects to merit merging

The reference for this pattern is the main promptPrimer optimization pass documented in `opti/synthesis.md`. One of its nine augmentations was rejected because the experiment showed it actively hurt scaffold quality — that is the correct outcome when evidence goes against the proposal. Contributions are welcome to be rejected for the same reason.

### 3. New harness support
If you use an agentic CLI that doesn't auto-load `CLAUDE.md`, `GEMINI.md`, or `AGENTS.md`, you can add support by:
- Creating an additional root-level instruction file with the filename your harness expects
- Making it identical in content to the existing three
- Updating the harness table in all three existing files and in `README.md`

Harness support contributions do not require A/B testing (they are mechanical) but do require verification that the harness actually loads the file.

### 4. Documentation improvements
Typos, clarifications, better examples, improved beginner onboarding — welcome without A/B testing. Open a PR with a clear description.

### 5. Bug reports and discussion
Open an issue. Be specific about what you observed and what you expected.

## Quality bar

Every contribution should match the existing repo's tone and discipline:
- **Concrete over abstract.** "Write clearly" is not a rule; "ban these 15 phrases literally" is a rule.
- **Grounded in frameworks.** Domain best practices trace to named methodologies (Diátaxis, Bloom's taxonomy, pyramid principle, PRISMA, backward design, etc.) where possible.
- **Enforceable.** A reader should be able to tell whether an output follows the rule or violates it.
- **Right-sized.** Type modules are ~100–140 lines. Longer is a smell; shorter is fine if the domain is tight.
- **No STATE.md or autonomy blocks in type modules.** Those are applied universally by the orchestrator from `knowledge/guardrails.md`. Do not duplicate them.

## The A/B testing requirement — why it exists

promptPrimer's own type modules were refined through a controlled multi-agent experiment (9 units, 27 scaffolds, 9 blind evaluators). Eight of nine proposed augmentations won or tied baseline on a 5-criterion rubric; one was rejected because it actively hurt. That experiment's full audit trail lives in `opti/` and its findings are documented in `opti/synthesis.md`, `opti/auto_critique.md`, and `opti/changelog.md`.

The reason A/B testing is required for type module contributions is that intuition about what makes a good prompt is unreliable. The coding worked-example augmentation that was rejected in the main experiment looked like a clear win on paper — few-shot learning, worked examples, structural clarity. The experiment showed it hurt. That kind of counter-intuitive finding only surfaces when you test.

Requiring A/B evidence also protects contributors: if a proposal doesn't beat the baseline, that's useful information, not a rejection of the contributor. The finding goes into the auto-critique and informs the next iteration.

## Submitting a pull request

PRs are submitted against the main branch. Use the PR template in `.github/PULL_REQUEST_TEMPLATE.md` and fill in every section. For type module and augmentation contributions, the A/B evidence section is mandatory and the PR cannot be merged without it.

Reviewers will check:
1. The proposed change matches the repo's quality bar (concrete, framework-anchored, enforceable)
2. The A/B evidence folder (`opti/new_type_<name>/` or `opti/augmentation_<id>/`) is present and complete
3. The synthesis file shows the required directional win
4. The auto-critique honestly names weaknesses
5. The orchestrator files and README are updated consistently
6. The PR description summarizes the experimental result in one paragraph

## Licensing and attribution

promptPrimer's orchestrator files, type modules, guardrails, and templates are MIT-licensed. Contributions are accepted under the same license. You retain copyright on your contribution but grant an MIT-license to users of the repo.

`knowledge/bestpractices.md` is sourced from Anthropic's official prompt engineering documentation and is governed by Anthropic's terms — not by the promptPrimer MIT grant. Do not modify that file in contributions; if you disagree with something in it, open an issue rather than a PR.

## Code of conduct

Be kind to the person on the other side of the conversation. Critique ideas, not contributors. Assume the other party is acting in good faith until evidence says otherwise. Remember that many contributors are not native English speakers and are writing in a second or third language.

## Questions?

Open a discussion or issue. If you are unsure whether a contribution is in scope, ask first — it's much cheaper than writing a full A/B test bundle that gets rejected for being out of scope.
