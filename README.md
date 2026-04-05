# PromptGen

Turn scrambled ideas into high-quality, structured prompts that bootstrap agentic work in any domain.

PromptGen is a meta-prompt system that runs inside an agentic CLI (Claude Code, Gemini CLI, Codex, OpenCode, Cursor, Aider, and others) and acts as a prompt-generating agent. You give it a rough idea — whether a piece of software, a research paper, a data extraction job, a book chapter, a curriculum, or a brand system — and it produces a single, self-contained prompt that another agent (or a fresh session) can execute to produce the planning scaffold for that work.

## Why

Most "first prompts" to an agent are under-specified. The agent fills gaps with defaults, and you end up reviewing output that was already going the wrong direction. PromptGen moves the quality problem upstream: it classifies what kind of task you have, loads domain-specific best practices, asks a small batch of high-leverage clarifying questions, then produces a tailored prompt grounded in established frameworks for that domain.

## Supported task types

| Type | What it covers |
|---|---|
| `coding` | Software, scripts, libraries, apps, systems |
| `data` | Extraction, scraping, ETL, cleaning, analysis, reporting |
| `writing` | Articles, essays, books, newsletters, marketing, fiction |
| `research` | Literature reviews, systematic reviews, academic papers, rigorous synthesis |
| `documentation` | Technical docs, tutorials, reference, knowledge bases |
| `business` | Strategy, market analysis, memos, plans, pitches |
| `education` | Curricula, lesson plans, training content, exercises |
| `creative` | Brand systems, design concepts, narrative worlds, game design |
| `general` | Fallback for tasks that don't cleanly fit above |

Each type has its own module in `types/` defining domain-specific best practices, artifact scaffolds across three complexity tiers, failure modes, and downstream-agent guardrails. Modules are anchored to established frameworks — Diátaxis for documentation, Bloom's taxonomy and backward design for education, PRISMA-style discipline for research, the pyramid principle for business, schema-first practices for data, and so on.

## Supported harnesses

PromptGen runs in any agentic harness that loads a root-level instructions file:

| Harness | Loads |
|---|---|
| Claude Code | `CLAUDE.md` |
| Gemini CLI | `GEMINI.md` |
| Codex CLI | `AGENTS.md` |
| OpenCode | `AGENTS.md` |
| Cursor | `AGENTS.md` |
| Aider | `AGENTS.md` |
| Other AGENTS.md-aware tools | `AGENTS.md` |

The three instruction files at the repo root are identical duplicates so whichever harness you use picks one up automatically. The generated prompts also target any of these harnesses — you pick yours when PromptGen asks during the clarifying-questions step.

## How it works

1. Drop this repo into your working directory and launch your agentic CLI.
2. Describe your task, however scrambled.
3. PromptGen classifies the task type and loads the matching module from `types/`.
4. It asks 3–8 clarifying questions in a single batch — scaled to task complexity, including your target harness.
5. It writes a single prompt file to `output/` named after your task.
6. You hand that prompt to a fresh agent session. That agent builds the planning scaffold (tier-sized to your task) and stops for your review before producing any deliverable.

No code, no content, no deliverable is produced until you've reviewed the plan.

## Repository layout

```
promptgen/
├── CLAUDE.md          # Instructions for Claude Code (identical to GEMINI.md and AGENTS.md)
├── GEMINI.md          # Instructions for Gemini CLI
├── AGENTS.md          # Instructions for Codex / OpenCode / Cursor / Aider / etc.
├── knowledge/
│   └── bestpractices.md   # Prompting best practices, loaded every session
├── types/
│   ├── coding.md
│   ├── data.md
│   ├── writing.md
│   ├── research.md
│   ├── documentation.md
│   ├── business.md
│   ├── education.md
│   ├── creative.md
│   └── general.md
├── output/            # Generated prompts land here
├── README.md          # This file (not read by agents)
└── LICENSE            # MIT (not read by agents)
```

## Philosophy

- **Right-sized.** Three complexity tiers per type. A one-off script doesn't get a PRD; a full system doesn't get a napkin sketch.
- **Grounded.** Every artifact and guardrail traces to an established domain practice, not ad-hoc LLM intuition.
- **Harness-agnostic.** The generated prompt works on any major agentic CLI.
- **Plan first, act later.** The generated prompt always stops for human review before execution.

## Adding a new task type

1. Create `types/<new-type>.md` following the structure of existing modules: frontmatter, `<overview>`, `<domain_best_practices>`, `<tiers>`, `<artifact_specs>`, `<failure_modes>`, `<nested_agent_file_directives>`.
2. Anchor best practices and guardrails to named, established frameworks from that domain.
3. Add the type to the table in `CLAUDE.md` / `GEMINI.md` / `AGENTS.md` (all three — they are identical) and include disambiguation heuristics against neighboring types.
4. Add it to the table in this README.

## Adding a new harness

1. Add a row to the harness table in `CLAUDE.md` / `GEMINI.md` / `AGENTS.md` (all three).
2. If the harness uses a different root instruction filename, create an additional duplicate at the repo root.
3. Add it to the harness table in this README.

## License

MIT. See [LICENSE](LICENSE).

## Contributing

New type modules, improved best-practice grounding, and new harness support are welcome. Each type module should be self-contained, opinionated, and traceable to established frameworks. Keep the main instruction files as thin orchestrators — the weight lives in `types/`.
