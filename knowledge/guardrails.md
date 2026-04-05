# Universal Guardrails — Autonomy and Context Preservation

This file defines two cross-cutting blocks that every promptPrimer-generated prompt MUST include verbatim: an **autonomy block** and a **context-preservation block**. They apply to every task type and every tier, and they layer on top of whatever the type module specifies — never replacing it.

Load this file alongside `knowledge/bestpractices.md` at session start. When generating a prompt, copy the two blocks below into the output as `<autonomy>` and `<context_preservation>` XML sections.

---

## The `<autonomy>` block — copy verbatim into every generated prompt

<autonomy>

**You are trusted to work autonomously within scope. The single mandatory checkpoint is after the planning scaffold is complete — stop for user review there, and only there. Before that checkpoint, work. After that checkpoint, work until the next natural milestone.**

1. **Decide-document-proceed on reversible judgment.** When two or more approaches are defensible, pick one, write a one-line rationale in the relevant project document (RULES.md, ISSUELOG.md, CHECKLOG.md, STATE.md — whichever fits), and continue. The user reviews written decisions asynchronously. Never block on a reversible choice.

2. **The only three "ask now" triggers.**
   (a) The next action is genuinely irreversible AND you lack the information to make it safely.
   (b) The action would violate stated scope or a hard constraint recorded in STATE.md.
   (c) A discovered fact invalidates the plan itself.
   Everything else — naming, style, minor tech choices, file layout, default values — is decide-and-proceed.

3. **Investigate before asking.** Before any question, verify the answer is not already in the scaffold documents, the original brief, or a quick file read. Most "questions" are lookups. Find it and act; do not narrate the investigation.

4. **Batched questions only.** If you must ask, bundle every open question into one turn. Never drip. If mid-task you realize you need information, finish everything else you can do without it, then ask all accumulated questions at once.

5. **No narration, no re-verification loops, no recaps.** Don't describe what you are about to do — do it. Don't re-read a file "to double-check" absent a new reason. Don't summarize completed work unless explicitly asked. Status is a side-effect of writing to files, not of talking about them.

6. **Document-don't-ask is the default pattern.** When uncertain about a judgment call, your first instinct is to write the decision and its rationale into the right file, not to ask the user. Async review beats sync approval for anything non-blocking.

7. **Irreversibility and scope are the only hard lines.** Destroying data, publishing, touching shared systems, expanding scope beyond the brief, spending money — these require confirmation. Everything else defaults to autonomy.

8. **No meta-commentary.** Don't explain your process, don't list what you are about to do, don't apologize, don't caveat. The user judges the work by the files you produce and the decisions you document, not by your narration of them.

</autonomy>

---

## The `<context_preservation>` block — copy verbatim into every generated prompt

<context_preservation>

**Harness context gets compacted. In-context memory is lossy. Anything that must survive to the next session lives in files — primarily in `STATE.md`.**

### `STATE.md` — the session-continuity snapshot

Every project has a `STATE.md` at its root. It is a **snapshot, not a log** — overwritten in place, not appended. Its size budget is **~1–2 KB**. If it grows beyond that, older entries are compressed, never the sections themselves. `STATE.md` is the single most important file for continuity and must never be deleted or allowed to go stale.

**Fixed schema:**

```
# Project State — <name>

**Last updated:** <ISO timestamp>
**Last session summary:** <1–2 sentences>

## Current focus
The single most important thing the next session needs to know.

## Phase and task
Active phase, active task within it, progress counter (e.g., 3/7 tasks done in Phase 2).

## Active decisions
Judgment calls made recently that later sessions must respect.
Each entry: decision — one-line rationale — pointer to the full record.

## Hard constraints
Invariants the user stressed. Must never be violated.
Each entry: constraint — why it matters.

## Discovered facts
Non-obvious things learned mid-work that were not in the original plan.
Each entry: fact — implication — where to find full context.

## Open questions
Pending asks for the user. Each entry: question — blocking or not — proposed default.

## Recently touched files
Last ~10 files with one-line "what changed" notes.

## Next action
The atomic next step for the next session.
```

### Update protocol

- **Read `STATE.md` first, every session, before any other project file.** No exceptions. This is the grounding ritual.
- **Update `STATE.md` after every substantive work block** — not only at session end. If compaction hits mid-task, the most recent state should still be close to current.
- **Enforce the size budget.** If `STATE.md` approaches 2 KB, compress older entries in Active Decisions and Discovered Facts by summarizing them. Never drop sections.
- **Write commit messages, code comments, and plan updates as if the next session has zero memory of this one.** No implicit continuity.

### Session opening protocol

Every session on this project starts with exactly these three actions, in order, before any other work:

1. **Read `STATE.md`.**
2. **Read the plan file** (DEVPLAN.md / PLAN.md / METHOD.md / CURRICULUM.md / OUTLINE.md / PIPELINE.md — whichever exists for this project type).
3. **Run `git log --oneline -20` and `git status`** if the project is under version control.

These three actions give a deterministic grounding in roughly 500–1000 tokens — vastly cheaper than re-exploration.

### Post-compaction recovery

If you detect you have just been compacted — the user references something you do not remember, summary tags appear in context, or context-awareness signals fire — do exactly this:

1. Stop. Do not try to reconstruct from partial memory.
2. Re-run the session opening protocol above.
3. Only then continue.

### Filesystem beats context

For anything that must persist, write it down. Files survive compaction; context does not. When in doubt about whether something belongs in a file or only in memory, put it in a file.

### Git as secondary durability

If `STATE.md` is ever lost or corrupted, rebuild from `git log` plus the other scaffold documents. For this to work: commit often, atomically, with descriptive messages. Never use vague commit messages — the next-session self will need them.

</context_preservation>

---

## Notes for promptPrimer (not copied into generated prompts)

- Both blocks above are authoritative and non-negotiable. Copy them verbatim or near-verbatim into every generated prompt. Do not paraphrase away any directive, do not reorder, do not omit parts.
- `STATE.md` is added to the scaffold of every generated project regardless of task type or tier. Treat type-module scaffolds as "domain-specific artifacts" and `STATE.md` as a universal addition at the project root.
- The nested agent-instruction file in every generated project (CLAUDE.md / GEMINI.md / AGENTS.md depending on harness) must instruct the downstream agent to run the session opening protocol at session start and update `STATE.md` after every substantive work block. This is in addition to whatever domain-specific directives the type module provides.
- The autonomy block assumes the downstream agent has exactly one mandatory checkpoint (after the scaffold). If the task has additional natural milestones, list them explicitly in the generated prompt — do not let the agent invent new checkpoints.
- These blocks exist for two reasons: to minimize token waste from unnecessary round-trips when context is large, and to survive harness compaction without quality loss. Keep that rationale in mind if you ever need to adapt the wording.
