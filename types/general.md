---
type: general
description: Fallback for tasks that do not cleanly fit other types
workspace: workspace/
---

# General — promptPrimer Type Module

<overview>
Use this module ONLY when no other type fits — mixed-domain work, personal projects, planning tasks, process design, life admin, bespoke workflows. Before selecting `general`, double-check whether `coding`, `data`, `writing`, `research`, `documentation`, `business`, `education`, or `creative` actually fits; specific types always give better scaffolds. Use `general` as a last resort.
</overview>

<domain_best_practices>
- **Clarify before acting.** Unclear goals produce unclear work.
- **State the definition of done.** What does success look like, concretely?
- **Break big things into small steps.** A plan with checkable items is better than a vague sprint.
- **Surface assumptions.** Every unstated assumption is a future disagreement.
- **Identify constraints early.** Time, budget, dependencies, blockers.
- **Separate what you know from what you're guessing.** Label them differently.
- **Checkpoint often.** Small reviews beat one giant one at the end.
- **Single source of truth per decision.** Don't scatter decisions across files.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single task, quick plan)
```
<task-slug>/
  GOAL.md              # Success criteria, constraints
  PLAN.md              # Ordered steps
  <agent-file>
  workspace/
```

## Tier 2 — Small (multi-step project, moderate scope)
```
<project-slug>/
  GOAL.md
  PLAN.md              # Phases with milestones
  RESOURCES.md         # Inputs, references, tools
  CHECKLOG.md          # Progress and decisions
  <agent-file>
  workspace/
```

## Tier 3 — Medium/Large (extended initiative, multi-stakeholder)
```
<project-slug>/
  GOAL.md
  CONTEXT.md           # Situation, stakeholders, constraints
  PLAN.md              # Phased plan with dependencies
  RISKS.md             # Known risks and mitigations
  RESOURCES.md
  CHECKLOG.md
  <agent-file>
  workspace/
```

</tiers>

<artifact_specs>

**GOAL.md**: one-sentence goal, **three explicit SMART success metrics** (specific, measurable, achievable, relevant, time-bound), non-goals, deadline, and a definition of done that references the metrics. Each metric must include a concrete verification method — how the user will prove, after the fact, that the metric was hit. Example format per metric: `1. <metric> — <target value> — <verification method>`. E.g., `1. Visit all 5 parks — 5/5 — photo of self at each entrance sign, committed to /trip/photos/`. Without the verification method, the metric does not count; rewrite it until the method is concrete.

**CONTEXT.md** (Tier 3): situation, stakeholders, constraints, history, prior attempts.

**PLAN.md**: ordered steps (Tier 1) or phased milestones (Tier 2/3). Each step: description, dependencies, acceptance check, estimated effort if relevant.

**RISKS.md** (Tier 3): known risks, likelihood and impact, mitigation, owner.

**RESOURCES.md**: inputs (documents, data, accounts), reference materials, tools, people.

**CHECKLOG.md**: progress entries with date, step, outcome, blockers, next step.

**Nested agent-instruction file**: points to GOAL.md for recentering; requires updating CHECKLOG.md after each session; forbids scope drift without updating GOAL.md.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **No vague success criteria.** "Done when X, Y, Z" — concrete and checkable.
- **No hidden assumptions.** State them or ask.
- **No silent scope drift.** Scope changes update GOAL.md explicitly.
- **No giant unchecked plans.** Break into verifiable steps.
- **No guessing when asking would cost less.** Ask before committing effort.
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read GOAL.md before every session
- Update CHECKLOG.md after every work block
- Surface assumptions and ask before proceeding on ambiguity
- Never drift scope without updating GOAL.md first
- Stop and recheck when a step's acceptance criterion is unclear
</nested_agent_file_directives>
