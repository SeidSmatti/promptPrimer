# promptPrimer

Turn scrambled ideas into high-quality, structured prompts that bootstrap agentic work in any domain — coding, data, writing, research, documentation, business, education, creative, and more.

promptPrimer is a meta-prompt system that runs inside an agentic command-line tool. You describe a rough idea in plain language, it asks you a handful of sharp questions, and it produces a single, self-contained prompt file that you then hand to a fresh agent session to start the actual work. That second session builds a planning scaffold for your project, stops for your review, and only then begins execution.

promptPrimer does not do the work. It prepares the work.

**Repository:** [github.com/SeidSmatti/promptPrimer](https://github.com/SeidSmatti/promptPrimer) · **License:** MIT

---

## Table of contents

- [Why use it](#why-use-it)
- [What you'll need](#what-youll-need)
- [Quick start (step-by-step for beginners)](#quick-start-step-by-step-for-beginners)
- [A complete worked example](#a-complete-worked-example)
- [Supported task types](#supported-task-types)
- [Supported harnesses](#supported-harnesses)
- [How it works under the hood](#how-it-works-under-the-hood)
- [Repository layout](#repository-layout)
- [Philosophy](#philosophy)
- [Knowledge sources and attribution](#knowledge-sources-and-attribution)
- [FAQ](#faq)
- [Extending promptPrimer](#extending-promptprimer)
- [License](#license)
- [Contributing](#contributing)

---

## Why use it

Most first prompts to an agentic CLI are under-specified. The agent fills the gaps with defaults, and you spend the next hour steering it back on course. promptPrimer moves the quality problem upstream: it classifies what kind of task you have, loads domain-specific best practices, asks a small batch of high-leverage clarifying questions, and generates a tailored prompt grounded in established frameworks for that domain — Diátaxis for documentation, Bloom's taxonomy and backward design for education, PRISMA-style discipline for research, the pyramid principle for business, schema-first practices for data, and so on.

Every generated prompt is also **token-efficient** (it raises the autonomy bar so the downstream agent works without drip-asking) and **compaction-resilient** (it mandates a durable `STATE.md` snapshot that survives context-window compaction without quality loss).

---

## What you'll need

Before you start, make sure you have:

1. **A terminal.** On macOS, Terminal.app or iTerm2. On Windows, Windows Terminal or PowerShell. On Linux, whatever you already use.

2. **Git**, to clone this repo. If you don't have it, install from [git-scm.com](https://git-scm.com).

3. **One agentic CLI.** Any of the following works — pick whichever you already use or prefer:
   - **[Claude Code](https://www.anthropic.com/claude-code)** — Anthropic's official CLI for Claude.
   - **[Gemini CLI](https://github.com/google-gemini/gemini-cli)** — Google's official CLI for Gemini.
   - **[Codex CLI](https://github.com/openai/codex)** — OpenAI's official CLI for GPT/Codex.
   - **[OpenCode](https://opencode.ai)** — open-source terminal agent.
   - **[Cursor](https://cursor.sh)** — AI-first code editor (uses the `AGENTS.md` convention).
   - **[Aider](https://aider.chat)** — AI pair programmer for the terminal.

You do **not** need to be a developer to use promptPrimer. It works for writing, research, education, business strategy, design, and more. But you do need basic comfort with the terminal: opening a folder, running one command, and copy-pasting text.

---

## Quick start (step-by-step for beginners)

promptPrimer uses **two folders**:
- The **promptPrimer folder** — where the tool lives. You clone it once and reuse it forever.
- Your **project folder** — a separate, empty folder where your actual work happens. You create a new one per project.

Think of promptPrimer as a workshop you visit to get a work order written up — then you take that work order to your own workshop to do the real job.

### Step 1 — Clone this repo somewhere permanent

Open a terminal and run:

```sh
git clone https://github.com/SeidSmatti/promptPrimer.git
cd promptPrimer
```

Put it somewhere you'll keep long term, for example `~/tools/promptprimer`. You will reuse the same install for every project.

### Step 2 — Open your agentic CLI inside the promptPrimer folder

Still inside the cloned folder, launch the agentic CLI you chose. For Claude Code:

```sh
claude
```

For Gemini CLI: `gemini`. For Codex: `codex`. For Aider: `aider`. Adjust as needed.

When the CLI starts, it automatically reads the instruction file at the repo root (`CLAUDE.md`, `GEMINI.md`, or `AGENTS.md` depending on your harness). That file turns the agent into promptPrimer.

### Step 3 — Describe your idea in plain language

Type your rough idea into the CLI. It doesn't need to be polished. Examples:

> I want to build a small tool that takes a folder of PDFs and extracts tables into CSV files, one CSV per PDF.

> I want to write a 6-week beginner photography curriculum for adults, 90-minute weekly sessions, in-person.

> I need a literature review on the effects of urban green spaces on adult mental health, academic tone, around 5000 words.

### Step 4 — Answer the clarifying questions

promptPrimer will classify your task into one of 9 types and then ask you 3–8 focused questions in a single batch. One question is always which agentic CLI you'll use for the actual project work. Answer them once and you're done — promptPrimer does not ask a second round.

### Step 5 — promptPrimer writes your prompt file

After your answers, promptPrimer writes a single prompt file into the `output/` folder inside the repo, named after your task. For example:

```
output/pdf-tables-prompt.md
output/photography-curriculum-prompt.md
output/urban-green-review-prompt.md
```

You can now close this CLI session. The prompt file is safely on disk.

### Step 6 — Create a new empty folder for your actual project

This is where your real work will live. It must be **separate** from the promptPrimer folder:

```sh
mkdir ~/projects/pdf-tables
cd ~/projects/pdf-tables
```

### Step 7 — Open a fresh CLI session inside the empty project folder

Launch your agentic CLI again, this time inside the **empty project folder**:

```sh
claude
```

(Or `gemini`, `codex`, `aider`, etc.)

This is a **different** session than the one you ran in the promptPrimer folder — a fresh agent with no prior context.

### Step 8 — Paste the generated prompt as your first message

Open the prompt file promptPrimer wrote (for example `~/tools/promptprimer/output/pdf-tables-prompt.md`) in any text editor, select everything, copy it, and paste it into the new agent session as your first message. Don't worry about formatting — agents read Markdown natively.

### Step 9 — The agent builds the scaffold and stops for your review

The agent reads the prompt, creates a planning scaffold for your project (a set of Markdown files describing requirements, plan, rules, state tracking, and so on), and **stops before writing any code or real deliverable**. It waits for you to review the scaffold.

### Step 10 — Review, adjust, and approve

Read the scaffold files. If anything is off, tell the agent what to change — it will update the scaffold in place. When you're happy, tell it to proceed. From that point on, the agent works through the plan, keeping `STATE.md` updated so it can resume cleanly across future sessions even if your laptop restarts or the CLI compacts its context.

---

## A complete worked example

Let's walk through an end-to-end session.

### Opening promptPrimer

```sh
cd ~/tools/promptprimer
claude
```

### Your first message

> I want a Python CLI that takes a folder of PDFs, extracts every table it can find, and writes one CSV per PDF. No OCR needed — the PDFs all have proper text layers.

### promptPrimer's questions

promptPrimer classifies the task as `coding` and asks 4 questions in a single bundle:

- **Target harness?** → `Claude Code`
- **Language and key libraries?** → `Python with pdfplumber`
- **Output file naming?** → `match input filename, with .csv extension`
- **What should happen on extraction failure?** → `skip the file, log the error, keep going`

### promptPrimer writes your prompt

After your answers, promptPrimer writes `output/pdf-tables-prompt.md`. You close the session.

### Opening your actual project

```sh
mkdir ~/projects/pdf-tables
cd ~/projects/pdf-tables
claude
```

### Pasting the prompt

You open `~/tools/promptprimer/output/pdf-tables-prompt.md`, copy everything, and paste it as your first message in the new `claude` session.

### The scaffold appears

The agent creates:

```
~/projects/pdf-tables/
├── SPEC.md             # Requirements and brief design
├── PLAN.md             # Ordered task list with acceptance per task
├── RULES.md            # Conventions, tech choices, anti-patterns
├── BUGLOG.md           # Bug tracking template
├── STATE.md            # Session-continuity snapshot
├── CLAUDE.md           # Agent instructions for this project
└── project/            # Empty — code lives here
```

The agent summarizes what it made and stops, waiting for you.

### You review

You read `SPEC.md`, notice the agent assumed sequential processing, and tell it: "Add parallel processing with a configurable worker count, default 4." The agent updates `SPEC.md` and `PLAN.md`, logs the decision in `RULES.md`, and asks if you're ready to proceed.

### Implementation

You say go. The agent works through `PLAN.md` step by step, updating `STATE.md` after each milestone and committing code to `project/` as it goes. Even if you close your laptop and come back two days later, the next session starts by reading `STATE.md` and picks up exactly where you left off — this is what the compaction-resilient design is for.

---

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

Each type has its own module in `types/` defining domain-specific best practices, artifact scaffolds across three complexity tiers, failure modes, and downstream-agent guardrails. Modules are anchored to established frameworks — Diátaxis for documentation, Bloom's taxonomy and backward design for education, PRISMA-style discipline for research, pyramid principle for business, schema-first practices for data, and so on.

## Supported harnesses

| Harness | Loads instruction file | Install reference |
|---|---|---|
| Claude Code | `CLAUDE.md` | [claude.com/claude-code](https://www.anthropic.com/claude-code) |
| Gemini CLI | `GEMINI.md` | [github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) |
| Codex CLI | `AGENTS.md` | [github.com/openai/codex](https://github.com/openai/codex) |
| OpenCode | `AGENTS.md` | [opencode.ai](https://opencode.ai) |
| Cursor | `AGENTS.md` | [cursor.sh](https://cursor.sh) |
| Aider | `AGENTS.md` | [aider.chat](https://aider.chat) |
| Other AGENTS.md-aware tools | `AGENTS.md` | — |

The three instruction files at the repo root are identical duplicates so whichever harness you use picks one up automatically. The generated prompts also target any of these harnesses — you pick yours during the clarifying-questions step.

---

## How it works under the hood

When you describe your idea, the promptPrimer agent:

1. Reads `knowledge/bestpractices.md` (prompt engineering reference) and `knowledge/guardrails.md` (universal autonomy and compaction-resilience directives).
2. Classifies your task into one of 9 types based on your language, goals, and deliverable.
3. Loads the matching module from `types/`. Each module contains domain-specific best practices, artifact scaffolds across three complexity tiers, failure modes, and guardrails grounded in established frameworks for that domain.
4. Asks 3–8 clarifying questions in a single batch, scaled to task complexity, always including which harness you'll use for the actual work.
5. Writes a tailored prompt to `output/`, combining the type-specific structure with universal autonomy and context-preservation blocks from the guardrails file.

The generated prompt, when pasted into a fresh session, produces a scaffold tier-sized to your task: a one-off script gets a lean `TASK.md + STATE.md + agent file + project/`, while a full system gets a complete `PRD.md + DEVPLAN.md + ARCHITECTURE.md + RULES.md + BUGLOG.md + STATE.md + agent file + project/`.

---

## Repository layout

```
promptprimer/
├── CLAUDE.md                # Orchestrator (Claude Code)
├── GEMINI.md                # Orchestrator (Gemini CLI) — identical to CLAUDE.md
├── AGENTS.md                # Orchestrator (Codex/OpenCode/Cursor/Aider/etc) — identical
├── knowledge/
│   ├── bestpractices.md     # Prompt engineering reference (see attribution below)
│   ├── guardrails.md        # Universal autonomy + context preservation directives
│   └── new_type_workflow.md # Agent-guided workflow for adding a new task type (A/B tested)
├── types/
│   ├── coding.md
│   ├── data.md
│   ├── writing.md
│   ├── research.md
│   ├── documentation.md
│   ├── business.md
│   ├── education.md
│   ├── creative.md
│   ├── general.md
│   └── _TEMPLATE.md         # Template to copy when contributing a new type module
├── .github/
│   └── PULL_REQUEST_TEMPLATE.md  # PR template (requires A/B evidence for type modules)
├── output/                  # Generated prompts land here (gitignored except .gitkeep)
├── CONTRIBUTING.md          # Contribution guide (quality bar, A/B testing requirement)
├── .gitignore
├── README.md                # This file (not read by agents)
└── LICENSE                  # MIT (not read by agents)
```

---

## Philosophy

- **Right-sized.** Three complexity tiers per type. A one-off script doesn't get a PRD; a full system doesn't get a napkin sketch.
- **Grounded.** Every artifact and guardrail traces to an established domain practice, not ad-hoc LLM intuition.
- **Harness-agnostic.** The generated prompt works on any major agentic CLI.
- **Plan first, act later.** The generated prompt always stops for human review before execution.
- **Token-efficient.** Every generated prompt raises the autonomy bar so the downstream agent decides, documents, and proceeds on reversible choices instead of drip-asking.
- **Compaction-resilient.** Every scaffolded project includes `STATE.md` — a durable session-continuity snapshot — so the agent can pick up cleanly after context compaction or between sessions.

---

## Knowledge sources and attribution

The file at `knowledge/bestpractices.md` is sourced from Anthropic's official prompt engineering documentation:

> [Claude prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) — Anthropic

That content is **Anthropic's work, not promptPrimer's**. It is reproduced in this repo so the orchestrator can load it at session start without fetching it over the network each time. All credit for the content in `knowledge/bestpractices.md` belongs to Anthropic.

### Terms of use

Anthropic's documentation is governed by Anthropic's own terms and usage policies. promptPrimer itself (the orchestrator files, type modules in `types/`, and `knowledge/guardrails.md`) is MIT-licensed — but the sourced best-practices content is **not** part of that grant. If you fork promptPrimer, publish a derivative, or use it commercially, review Anthropic's documentation terms at [anthropic.com/legal](https://www.anthropic.com/legal) (or the successor page) to make sure your use is compatible. This README is not legal advice; when in doubt, ask a lawyer or replace the file with content you own (see below).

### Limitations

- The content is **Claude-specific**, targeting the Claude Opus 4.6 / Sonnet 4.6 / Haiku 4.5 generation. Some guidance (thinking configuration, effort parameters, API-specific behavior) does not generalize to other model families without adjustment.
- The content is a **snapshot**. Anthropic updates the source documentation regularly; the file in this repo will drift out of date. Re-fetch periodically if you want the latest guidance.
- Some sections are **Anthropic-API-specific** (adaptive thinking, memory tool, extended thinking migration) and irrelevant if you drive promptPrimer with non-Anthropic harnesses.

### Replacing `knowledge/bestpractices.md`

The orchestrator loads whatever file exists at `knowledge/bestpractices.md` — the filename is the contract, not the content. You can replace it freely with any prompt-engineering reference you prefer. Suggestions:

- **[Google's Gemini prompting guide](https://ai.google.dev/gemini-api/docs/prompting-strategies)** if you primarily use Gemini CLI.
- **[OpenAI's prompt engineering guide](https://platform.openai.com/docs/guides/prompt-engineering)** if you primarily use Codex / ChatGPT workflows.
- **Your own distilled notes** tailored to your domain, model, and harness.
- **A hybrid** that merges multiple sources into one coherent reference.

The only requirement is that the file exists at `knowledge/bestpractices.md` and contains concrete, actionable prompting guidance. If you replace it, update this README's attribution section to reflect the new source.

`knowledge/guardrails.md` — the universal autonomy and context-preservation directives — is original promptPrimer content and is MIT-licensed along with the rest of the repo.

---

## FAQ

**Do I need to be a developer?**
No. promptPrimer handles writing, research, education, business strategy, design, and more. You need basic terminal comfort — running one command and copy-pasting text — but not programming skills.

**Why two folders? Why not run promptPrimer and the project in the same place?**
Because the promptPrimer instructions would confuse the agent in the project folder and vice versa. Keeping them separate means the tool is the tool and the workspace is the workspace. You clone promptPrimer once and reuse it for every project.

**Can I use one promptPrimer install for many projects?**
Yes — that is the intended use. Clone once, run it as many times as you have ideas. Each run writes a new prompt file to `output/`.

**Can I edit the generated prompt before pasting it into my project session?**
Yes. The output file is yours. Open it in any text editor and adjust anything before pasting it into the project session.

**What if my idea doesn't fit any of the 9 types?**
promptPrimer classifies into `general` as a fallback. You can also override the classification during the clarifying-questions step if you disagree with the automatic pick.

**What if I pick the wrong harness?**
The only practical difference is the filename of the agent instruction file inside your project (`CLAUDE.md` vs `GEMINI.md` vs `AGENTS.md`). If you change your mind later, rename the file — the content is harness-agnostic.

**Does promptPrimer cost money?**
promptPrimer itself is free (MIT). Your chosen agentic CLI may have its own usage costs (API tokens, subscriptions). Check each harness's pricing.

**What happens if I close my terminal mid-project?**
The scaffold, code, and `STATE.md` are all on disk. Re-launch your CLI in the project folder and it will read `STATE.md` first and resume exactly where you left off. That is what `STATE.md` is for.

**What if the agent makes a wrong decision autonomously?**
promptPrimer's generated prompts instruct the agent to write every judgment call to the project's rules, state, or issue-log files with a short rationale. You can review and override asynchronously — much cheaper than the agent stopping every few minutes to ask.

**Why does the agent use so few checkpoints?**
Token efficiency. Every intermediate "stop and confirm" turn becomes expensive once the session has accumulated context. The generated prompt gives the agent exactly one mandatory checkpoint (after the scaffold) and otherwise tells it to decide, document the decision, and proceed. You still have full oversight — just asynchronously, via the files the agent writes.

**Can I add a new task type or support a new harness?**
Yes. See the Extending section below.

---

## Extending promptPrimer

promptPrimer is designed to be extended. Two kinds of extensions are common: new task types (e.g., a hypothetical `legal`, `music`, or `translation` module) and new harness support. Both are welcome contributions.

### Adding a new task type — the easy way

You can ask an agent running inside the promptPrimer repo to add a new type for you. Say something like *"I want to add a new task type for legal work"* or *"Add a type module for music composition"*. The orchestrator will read `knowledge/new_type_workflow.md` and follow the full workflow, which includes:

1. **Deciding whether a new type is actually warranted** (many candidate "new types" are better handled by extending an existing module).
2. **Researching the domain** to identify 1–3 named, established frameworks that will anchor the module's best practices.
3. **Drafting the module** by copying `types/_TEMPLATE.md` and filling in every section.
4. **Mandatory A/B testing** — picking 3 realistic dummy subjects, running a 3-condition experiment (current `general` baseline vs. simple ablation vs. new type), blind-evaluating with the 5-criterion rubric at `opti/templates/rubric.md`, and checking that the new type beats baseline by at least +1 point on average and wins or ties in at least 2 of 3 subjects.
5. **Writing an honest auto-critique** that names the experiment's weaknesses.
6. **Preparing a PR bundle** against `.github/PULL_REQUEST_TEMPLATE.md` including the full A/B audit trail.

The agent stops before submitting — the user makes the merge decision.

### Adding a new task type — the manual way

If you prefer to draft by hand:
1. Read `knowledge/new_type_workflow.md` in full.
2. Copy `types/_TEMPLATE.md` to `types/<your_type>.md` and fill it in following the rules in the workflow file.
3. Run the mandatory A/B test following the experimental shape of promptPrimer's own optimization pass (the `opti/` folder is your reference for what a finished test bundle looks like).
4. Add your type to the table in `CLAUDE.md` / `GEMINI.md` / `AGENTS.md` (all three are identical) with disambiguation heuristics against neighboring types, and to the table in this README.
5. Submit a PR using `.github/PULL_REQUEST_TEMPLATE.md`, filling in the A/B evidence section.

You do **not** need to add `STATE.md` or the universal autonomy / context-preservation blocks to your type module — those are applied automatically by the orchestrator from `knowledge/guardrails.md`.

### Augmenting an existing type module

Proposing a change to an existing type module (new failure mode, stricter enforcement, removed bloat) follows the same pattern as adding a new type, but scoped to a single existing module. The promptPrimer optimization pass documented in `opti/synthesis.md` is the reference: 8 of 9 proposed augmentations won or tied baseline, and 1 was rejected because the experiment showed it hurt. Your augmentation needs to win or tie in at least 2 of 3 A/B test subjects to merit merging.

### Adding a new harness

Mechanical, no A/B testing required:
1. Add a row to the harness table in `CLAUDE.md` / `GEMINI.md` / `AGENTS.md` (all three).
2. If the harness uses a different root instruction filename than `CLAUDE.md`, `GEMINI.md`, or `AGENTS.md`, create an additional duplicate at the repo root with identical content.
3. Add it to the harness table in this README with a link to the harness's install documentation.
4. Verify the harness actually loads the file before submitting the PR.

---

## License

MIT — see [LICENSE](LICENSE). This covers the promptPrimer orchestrator, type modules, and `knowledge/guardrails.md`. The content of `knowledge/bestpractices.md` is sourced from Anthropic's documentation and is governed by Anthropic's terms; see the attribution section above.

## Contributing

**Contributions are welcome.** See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full guide. The short version:

- **New type modules** and **augmentations to existing type modules** require **A/B testing evidence**. No new type ships without empirical proof that it beats the `general` fallback on at least 3 realistic dummy subjects. This is a high bar intentionally — promptPrimer's own type modules were refined through the same process and one proposed augmentation was rejected by it (see [`opti/synthesis.md`](opti/synthesis.md) for the details).
- **New harness support** is mechanical and does not require A/B testing, but does require verification that the harness actually loads the instruction file.
- **Documentation improvements**, bug fixes, typos, and beginner-onboarding clarifications are welcome without A/B testing.
- **Bug reports and feature discussions**: open an issue.

### Quality bar

Every contribution should match the existing repo's discipline:
- **Concrete over abstract.** "Write clearly" is not a rule; "ban these 15 phrases literally" is a rule.
- **Grounded in named frameworks.** Domain best practices trace to Diátaxis, Bloom's taxonomy, pyramid principle, PRISMA, backward design, schema-first data practices, or equivalent where possible.
- **Enforceable.** A reader should be able to tell whether an output follows the rule or violates it.
- **Right-sized.** Type modules are ~100–140 lines, matching the existing modules.
- **No `STATE.md` or autonomy/context-preservation blocks inside a type module.** Those are applied universally by the orchestrator from `knowledge/guardrails.md`.

### Pull requests

PRs use the template at [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md), which includes a mandatory A/B evidence section for type modules and augmentations. The A/B evidence bundle lives in a dedicated `opti/<folder_name>/` directory and travels with the PR — the PR cannot be merged without it.

If you are unsure whether a contribution is in scope, open an issue first. That is much cheaper than writing a full A/B test bundle and discovering it was out of scope.
