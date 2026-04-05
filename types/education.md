---
type: education
description: Curricula, lesson plans, course materials, exercises, training content
workspace: materials/
---

# Education — PromptGen Type Module

<overview>
Use this module for learning materials: curricula, lesson plans, courses, training modules, exercises, quizzes, workbooks, worksheets, and instructional content for any age or setting. If the deliverable is general explanation without learning-outcome alignment, use `writing` or `documentation`.
</overview>

<domain_best_practices>
- **Backward design** (Wiggins & McTighe). Define learning outcomes → design assessments that measure them → then design activities that enable them. Never the reverse.
- **Bloom's taxonomy alignment.** Match verbs in outcomes and assessments to the intended cognitive level (remember / understand / apply / analyze / evaluate / create).
- **Active learning beats passive reception.** Every segment ends with the learner doing something.
- **Scaffolding.** Worked example → guided practice → independent practice. Don't jump straight to independent work.
- **Formative + summative assessment.** Check for understanding during; measure mastery at the end.
- **Reading level and cognitive load calibrated to audience.** Verify with Fry/Flesch or equivalent when text-heavy.
- **Culturally responsive examples.** Names, contexts, scenarios that don't exclude learners.
- **Universal Design for Learning.** Multiple means of representation, engagement, and expression where possible.
- **Fight the curse of knowledge.** Explain what feels obvious to you but isn't to the learner.
- **Every activity maps to an outcome.** If it doesn't, cut it — it's filler.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single lesson, one worksheet, one quiz)
```
<lesson-slug>/
  LEARNER_PROFILE.md   # Audience, prior knowledge, context
  OUTCOMES.md          # 2-5 measurable outcomes with Bloom's levels
  LESSON.md            # Lesson plan
  ASSESSMENT.md        # How outcomes are measured
  <agent-file>
  materials/
```

## Tier 2 — Small (multi-lesson unit, short course, training workshop)
```
<unit-slug>/
  LEARNER_PROFILE.md
  OUTCOMES.md
  CURRICULUM.md        # Lesson sequence with dependencies
  LESSON_TEMPLATE.md   # Standard lesson format
  ASSESSMENT_PLAN.md   # Formative + summative strategy
  REVISIONLOG.md
  <agent-file>
  materials/
```

## Tier 3 — Medium/Large (full course, semester curriculum, certification program)
```
<course-slug>/
  LEARNER_PROFILE.md
  PROGRAM_OUTCOMES.md  # Program-level outcomes
  MODULE_OUTCOMES.md   # Module-level outcomes mapped to program
  CURRICULUM.md        # Full sequencing across modules
  LESSON_TEMPLATE.md
  ASSESSMENT_PLAN.md   # Formative + summative + mastery gates
  ACCESSIBILITY.md     # UDL notes, reading level, adaptations
  INSTRUCTOR_GUIDE.md  # Facilitation notes, timing, FAQ
  REVISIONLOG.md
  <agent-file>
  materials/
```

</tiers>

<artifact_specs>

**LEARNER_PROFILE.md**: age or experience level, prior knowledge, context of learning (classroom / self-paced / workshop), time budget, motivations, known barriers.

**OUTCOMES.md**: measurable statements using Bloom-aligned verbs. Bad: "understand recursion". Good: "trace execution of a recursive function on a given input and predict its return value". Each outcome tagged with Bloom level.

**PROGRAM_OUTCOMES.md / MODULE_OUTCOMES.md** (Tier 3): hierarchical outcomes where module outcomes aggregate to program outcomes.

**CURRICULUM.md**: lesson sequence with dependencies, time allocation, throughlines between lessons.

**LESSON.md / LESSON_TEMPLATE.md**: objectives, warm-up/hook, direct instruction, guided practice, independent practice, assessment, closure, materials needed, timing.

**ASSESSMENT.md / ASSESSMENT_PLAN.md**: per outcome — how it's measured, what success looks like, formative checks, summative evidence, rubric where appropriate.

**ACCESSIBILITY.md** (Tier 3): UDL notes, reading level, adaptations for different learners, alt text for visuals, transcript requirements.

**INSTRUCTOR_GUIDE.md** (Tier 3): facilitation tips, anticipated student questions, common misconceptions, timing flexibility, differentiation strategies.

**REVISIONLOG.md**: date, change, reason (student feedback / pilot result / alignment fix).

**Nested agent-instruction file**: mandates reading OUTCOMES.md before designing any activity; requires every activity to map to a listed outcome; forbids creating assessment items that don't map to outcomes.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **Never create activities that don't map to an outcome.** Filler is cut.
- **Never misalign assessment with outcome level.** If the outcome is "analyze", the assessment cannot be "recall".
- **Never use "understand" as the only verb in outcomes.** Use measurable Bloom verbs.
- **Never write instructions above the target reading level** without scaffolding.
- **Never assume learner knowledge beyond the stated profile.**
- **Never use culturally specific examples that exclude the target audience.**
- **Never skip the "you do" step.** Learners must practice, not just watch.
- **Never grade what wasn't taught.** Assessments cover only covered material.
- **Never cram multiple outcomes into one assessment item** unintentionally.
- **Never treat "engagement" as a proxy for learning.** Engagement supports learning; it doesn't replace it.
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read OUTCOMES.md and LEARNER_PROFILE.md before creating any material
- Verify every activity maps to a stated outcome; delete or move any that don't
- Run a Bloom-level alignment check between outcomes and assessments
- Pilot a single lesson before scaling to a full unit when possible
- Log revisions with reasons tied to outcomes or learner feedback
</nested_agent_file_directives>
