---
type: creative
description: Design systems, branding, narrative worlds, game design, visual and verbal concepts
workspace: work/
---

# Creative — PromptGen Type Module

<overview>
Use this module for concept-driven creative work where point of view and coherence matter: brand identity systems, design concepts, UX vision, narrative worlds, game design documents, campaign ideas, character and world bibles. If the primary deliverable is long-form prose, use `writing`. If it is an academic analysis of creative work, use `research`.
</overview>

<domain_best_practices>
- **Commit to a point of view.** Distinctiveness beats safety. Generic work is forgotten.
- **Reference-informed, not reference-bound.** Touchstones guide; they do not get copied.
- **Internal consistency.** Every element of a system reinforces the same core concept. Contradictions dilute impact.
- **Avoid AI-slop defaults.** Purple gradients on white, Inter/Space-Grotesk/Roboto defaults, generic hero layouts, safe stock-photo metaphors, "as X meets Y" pitches, five-adjective brand pillars that could belong to any brand.
- **Name the rule, then break it intentionally.** Know conventions before subverting them.
- **Test concepts against the brief.** Every creative decision is justified by the brief, not by personal taste drift.
- **Specificity creates distinctiveness.** Named things, textures, concrete references beat abstractions.
- **Single unifying metaphor.** A concept with one strong spine outperforms a kitchen sink.
- **The brief is not the output.** Restating the brief in fancier words is not a concept.
- **Kill darlings.** Cut anything that doesn't serve the concept, even if it's clever.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single-asset concept, short creative brief, one-off idea)
```
<concept-slug>/
  CREATIVE_BRIEF.md    # Purpose, audience, tone, constraints, references
  CONCEPT.md           # Core concept and one-line articulation
  DELIVERABLES.md      # What gets made, with specs
  <agent-file>
  work/
```

## Tier 2 — Small (brand mini-system, short narrative bible, small game concept)
```
<project-slug>/
  CREATIVE_BRIEF.md
  CONCEPT.md           # Big idea, mood, POV
  SYSTEM.md            # Visual + verbal system rules
  DELIVERABLES.md
  REVISIONLOG.md
  <agent-file>
  work/
```

## Tier 3 — Medium/Large (full brand system, full game design doc, full narrative universe)
```
<project-slug>/
  CREATIVE_BRIEF.md
  CONCEPT.md           # Core concept + manifesto
  SYSTEM.md            # Complete design system
  PRINCIPLES.md        # Decision principles for edge cases
  WORLD.md             # (Narrative/game) setting bible
  CHARACTERS.md        # (Narrative/game) character bible
  MECHANICS.md         # (Game) systems and interactions
  VOICE.md             # (Brand) voice and tone
  REFERENCES.md        # Mood board notes, touchstones, anti-references
  DELIVERABLES.md
  REVISIONLOG.md
  <agent-file>
  work/
```

</tiers>

<artifact_specs>

**CREATIVE_BRIEF.md**: purpose, audience, desired feeling, brand/project constraints, non-negotiables, mandatory elements, anti-patterns (what to avoid), touchstone references, timeline, deliverable list.

**CONCEPT.md**: one-line concept statement, why it fits the brief, what it unlocks, what it rules out. The manifesto if Tier 3.

**SYSTEM.md**: for visual — color palette with rationale, typography with rationale, layout principles, motion principles, iconography rules, photography style. For verbal — tone, sentence rhythm, vocabulary, forbidden constructions, signature moves. Everything traceable to CONCEPT.md.

**PRINCIPLES.md** (Tier 3): decision rules for edge cases ("when we're unsure, pick the weirder option", "never use more than two accent colors", "characters never lie directly to the reader").

**WORLD.md / CHARACTERS.md / MECHANICS.md** (narrative/game Tier 3): setting rules that cannot be contradicted; character bibles with voice and arc; game mechanics with interactions and edge cases.

**VOICE.md** (brand Tier 3): tone matrix, sample copy across contexts, ban-list.

**REFERENCES.md**: touchstones (what to echo) and anti-references (what to avoid resembling). Specific, named works.

**DELIVERABLES.md**: every asset with format, dimensions, usage context, and acceptance criteria.

**REVISIONLOG.md**: iteration notes tied back to the concept.

**Nested agent-instruction file**: mandates reading CONCEPT.md at every session; requires every creative decision to trace back to CONCEPT.md; forbids default AI aesthetics; requires specificity over abstraction.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **No default AI aesthetics.** Avoid: purple-to-blue gradients, Inter/Roboto/Space Grotesk defaults, stock-photo metaphors, "innovative solutions that empower", five identical brand adjectives.
- **No safe middle ground.** Pick a direction and commit.
- **No kitchen-sink concepts.** One unifying idea, not three diluted ones.
- **No drift from the brief.** Every decision justified by the brief, not by personal taste.
- **No reference copying.** Touchstones inform; they do not become the work.
- **No abstraction where specificity would land harder.** Named things > categories.
- **No "X meets Y" as a concept.** That's a shortcut, not an idea.
- **No contradictions inside the system.** Consistency is the point.
- **No darlings that don't serve the concept.** Clever ≠ fit.
- **No generic adjectives as brand pillars.** "Innovative, bold, human, smart, trusted" means nothing.
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read CONCEPT.md and CREATIVE_BRIEF.md at the start of every session
- Trace every new creative decision back to CONCEPT.md in writing
- Resist defaulting to safe, on-distribution aesthetics
- Log every iteration with the concept-tie rationale
- Ask the user before pivoting the core concept
</nested_agent_file_directives>
