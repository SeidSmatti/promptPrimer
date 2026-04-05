---
type: writing
description: Articles, blog posts, books, newsletters, marketing copy, fiction, long-form creative prose
workspace: drafts/
---

# Writing — PromptGen Type Module

<overview>
Use this module for long-form prose where voice, structure, and audience matter more than raw information transfer. Covers articles, blog posts, essays, newsletters, marketing copy, short stories, scripts, book chapters, and speeches. If the output is technical reference or how-to, use `documentation`. If it requires citations and rigorous synthesis, use `research`.
</overview>

<domain_best_practices>
- **Audience first.** Who reads this, in what state, looking for what? Every line serves them.
- **Clarity over cleverness** for non-fiction; **specificity over abstraction** for all genres.
- **Commit to a voice** and hold it line by line. Drift kills trust.
- **Structural revision before line editing.** Reshape the argument before polishing sentences.
- **Concrete nouns and verbs carry prose.** Adjectives and adverbs prop up weak ones.
- **Read aloud.** Rhythm problems are audible before they're visible.
- **Cut hedges and throat-clearing.** "It's important to note that..." is usually deletable.
- **Show, don't tell** for fiction and memoir. Scene beats summary.
- **Earn every metaphor.** Dead metaphors ("level the playing field") add noise, not image.
- **Title and opening do 80% of the work.** Write them last; revise them most.
- **The draft is not the piece.** Treat every draft as the raw material for the next one.
</domain_best_practices>

<tiers>

## Tier 1 — Tiny (single post, short piece, ≤1500 words)
```
<piece-slug>/
  BRIEF.md             # Audience, purpose, tone, length, hook, takeaway
  OUTLINE.md           # 3-7 beat structural plan
  STYLE.md             # Voice rules (POV, tense, forbidden phrases, touchstones)
  <agent-file>
  drafts/
```

## Tier 2 — Small (essay series, long article, newsletter season, short story)
```
<project-slug>/
  BRIEF.md
  OUTLINE.md           # Per-piece or per-section
  STYLE.md
  REFERENCES.md        # Source material, facts to verify, inspirations
  REVISIONLOG.md       # Feedback and changes across drafts
  <agent-file>
  drafts/
```

## Tier 3 — Medium/Large (book, novel, season-length content arc)
```
<project-slug>/
  BRIEF.md             # Thesis or premise, reader takeaway, scope
  STRUCTURE.md         # Chapter/act map with throughlines
  OUTLINE.md           # Per-chapter beats
  STYLE.md             # Voice bible
  CHARACTERS.md        # (Fiction) character bible
  WORLD.md             # (Fiction) setting and lore rules
  REFERENCES.md
  REVISIONLOG.md
  <agent-file>
  drafts/
```

</tiers>

<artifact_specs>

**BRIEF.md**: title or working title, audience (specific, not "everyone"), purpose (what the reader does/feels/knows after), length target, tone, hook idea, promised takeaway.

**OUTLINE.md**: beat-by-beat structural plan. Each beat: what happens or what is argued, why it's here, how it connects to the next. Non-fiction: claim → evidence → so-what. Fiction: scene purpose → conflict → change.

**STYLE.md**: POV, tense, reading level, sentence-length target, rhythm notes, stylistic touchstones (authors/works to echo, works to avoid resembling), forbidden phrases, punctuation conventions, paragraph shape.

**STRUCTURE.md** (Tier 3): chapter/act map with throughlines, pacing, arc shape, setups and payoffs.

**CHARACTERS.md / WORLD.md** (Tier 3 fiction): character bible with voice samples, arcs, relationships; world rules that must never be contradicted.

**REFERENCES.md**: inputs, inspirations, facts requiring verification, quotes to attribute, anti-references (works to avoid resembling).

**REVISIONLOG.md**: date, draft version, changes, feedback source, next focus.

**Nested agent-instruction file**: points to BRIEF.md and STYLE.md as non-negotiable; mandates reading STYLE.md before any drafting; requires structural review before line edits; logs revisions.

</artifact_specs>

<failure_modes>
Guardrails the generated prompt MUST include:
- **No AI-slop phrases.** Ban-list in STYLE.md: "delve into", "it's important to note", "in today's fast-paced", "navigate the landscape", "unlock", "leverage", "tapestry", "testament to", "journey", "ever-evolving", "at the end of the day", "when it comes to", opening with "In a world where".
- **No hedging sprawl.** One "perhaps" or "arguably" per section maximum.
- **No filler transitions.** "Moreover", "Furthermore", "Additionally" — almost always deletable.
- **No tense drift.** Pick past or present and hold it.
- **No POV slippage.** First person stays first person unless the piece explicitly shifts.
- **No invented facts.** Any factual claim must be verifiable; if it isn't, rewrite or cut.
- **No "as an AI" meta.** The author voice is human.
- **No structural over-bulleting.** Prose uses paragraphs; reserve lists for truly discrete items.
- **No generic examples.** If you need an example, make it specific and named.
- **No ending-on-summary.** Earn the last line; don't restate the middle.
</failure_modes>

<nested_agent_file_directives>
The nested agent-instruction file must direct future agents to:
- Read STYLE.md before writing a single line of draft
- Read BRIEF.md at the start of every session to recenter on audience and promise
- Do structural revision passes separately from line edits
- Log every revision session with focus and outcome
- Stop and ask when direction is ambiguous rather than drifting
- Never declare a draft "done" — only "ready for review"
</nested_agent_file_directives>
