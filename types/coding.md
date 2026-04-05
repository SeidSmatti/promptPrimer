---
type: coding
description: Software projects, scripts, libraries, apps, and systems — deliverable is executable code
workspace: project/
---

# Coding — promptPrimer Type Module

<overview>
Use this module when the primary deliverable is executable code: scripts, utilities, libraries, apps, CLIs, services, full systems. If the user is producing software artifacts that compile, run, or get imported by other code, this is the right type. Hybrid tasks (e.g., "build a scraper and analyze the results") default here if the code component is substantial; otherwise consider `data`.
</overview>

<domain_best_practices>
- **Investigate before changing.** Read relevant files before proposing or writing code. Never speculate about code you haven't opened.
- **Minimum necessary complexity.** The right amount is what the task actually requires — no speculative abstractions, but no half-finished implementations either.
- **Trust framework guarantees.** Validate at system boundaries (user input, external APIs). Don't add defensive checks for scenarios internal code cannot produce.
- **No unrequested refactoring.** A bug fix doesn't need surrounding cleanup. A small feature doesn't need extra configurability.
- **Test behavior, not implementation.** Tests verify correctness; they don't define the solution. Never hardcode to pass specific test inputs.
- **Idempotent setup.** Build scripts, migrations, and bootstraps should be safely rerunnable.
- **Fail loudly by default.** Prefer crashes with clear errors over silent fallbacks that hide bugs.
- **Standard tools over bespoke helpers.** Don't invent utility scripts when the language or framework already provides the primitive.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single script, one-file utility, quick CLI)
```
<task-slug>/
  TASK.md              # Goal, inputs, outputs, constraints, acceptance criteria (≤1 page)
  <agent-file>         # Agent instructions (CLAUDE.md / GEMINI.md / AGENTS.md)
  project/             # Empty — code lives here
```

## Tier 2 — Small (library, small app, tool with a few moving parts)
```
<project-slug>/
  SPEC.md              # Condensed requirements + brief design
  PLAN.md              # Ordered task list with acceptance per task
  RULES.md             # Conventions, tech choices, anti-patterns
  BUGLOG.md            # Bug tracking template
  <agent-file>
  project/
```

## Tier 3 — Medium/Large (multi-component system, non-trivial architecture)
```
<project-slug>/
  PRD.md               # Product Requirements Document
  DEVPLAN.md           # Phased, step-by-step development plan
  ARCHITECTURE.md      # Components, data flow, interfaces
  RULES.md             # Conventions, tech, anti-patterns
  BUGLOG.md            # Bug tracking template
  <agent-file>
  project/
```

Pick the lowest tier that fits. When unsure between tiers, pick the smaller one.

</tiers>

<artifact_specs>

**PRD.md / SPEC.md / TASK.md** (scaled versions of the same idea):
- Name, one-line description, motivation
- Target users and use cases
- Functional requirements (numbered, prioritized)
- Non-functional requirements (performance, security, accessibility) as relevant
- Out of scope (explicit boundaries)
- Success / acceptance criteria

**DEVPLAN.md / PLAN.md**:
- Phases with milestones (or a flat ordered list for smaller tiers)
- Each step discrete and testable
- Dependencies called out
- Tech stack decisions with rationale
- Testing strategy per phase
- Definition of done per step

**ARCHITECTURE.md** (Tier 3 only):
- High-level diagram (textual or mermaid)
- Component breakdown with responsibilities
- Data flow between components
- API contracts / interfaces
- Directory layout for `project/`
- External dependencies and integrations

**RULES.md**:
- Coding conventions (naming, formatting, patterns)
- Git workflow if relevant
- Testing requirements (coverage, types)
- Dependency management
- Security practices
- What NOT to do (no premature abstraction, no speculative error handling, no silent fallbacks, no unrequested refactors, no defensive validation for internal calls)

**BUGLOG.md**:
- Template: ID, date, description, repro steps, root cause, fix, status
- Starts empty — filled during development

**Nested agent-instruction file** (`CLAUDE.md` / `GEMINI.md` / `AGENTS.md`):
- References the other docs with one-line purpose summaries
- Working directory and structure overview
- Key commands (build / test / lint / run) as known at planning time
- Workflow pointers: RULES.md for conventions, the plan file for current phase, BUGLOG.md for logging issues
- Instruction to keep docs up to date as the project evolves

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **No speculative files.** Don't create helpers, utilities, or abstractions for one-time operations.
- **No hallucinated APIs.** Verify library/framework surfaces before using them.
- **No hardcoding to pass tests.** Implement the actual logic; tests verify correctness.
- **No silent fallbacks.** Errors surface with clear messages.
- **No unrequested refactors.** Fix what's asked; leave the rest alone.
- **No defensive validation for internal calls.** Validate only at system boundaries.
- **No docstring/type-annotation sprawl.** Only on code actually being changed.
- **No premature abstraction.** Three similar lines > an abstraction designed for a hypothetical fourth case.
- **Investigation before answering.** Read referenced files first; never guess.
</failure_modes>

<nested_agent_file_directives>
The generated prompt must instruct the downstream agent to write the nested agent-instruction file so it:
- Tells future agents to work incrementally: pick the next task from the plan, finish it, update progress, move on
- Points to RULES.md before coding and at the first sign of style ambiguity
- Requires reading related files before editing
- Requires logging bugs to BUGLOG.md when found
- Forbids scope creep beyond the current task
- Tells the agent to stop and ask when the plan is genuinely ambiguous, not guess
</nested_agent_file_directives>
