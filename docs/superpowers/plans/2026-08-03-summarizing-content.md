# Universal Project Summary Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the project-local meeting-minutes skill with an adaptive, source-grounded skill that triggers for every summary request.

**Architecture:** A short `summarizing-content/SKILL.md` routes requests to general, Telegram, or meeting behavior. Telegram and meeting details live in one-level reference files so ordinary summaries do not load irrelevant instructions.

**Tech Stack:** Markdown Agent Skills, YAML frontmatter, project `AGENTS.md`, Python skill validator, fresh-agent behavioral evaluations.

## Global Constraints

- Install only inside this repository.
- Use adaptive detail when the user does not specify output length.
- Preserve Telegram read-only behavior and the repository's complete-coverage rules.
- Do not invent decisions, actions, owners, deadlines, links, media contents, or consensus.
- Use the user's language unless explicitly requested otherwise.
- Build from `E:/tg_download/group_summary.md` and retain stronger compatible rules from the existing skill.

---

### Task 1: Establish RED baseline evaluations

**Files:**
- Create: `docs/superpowers/skill-tests/summarizing-content-scenarios.md`
- Read: `.agents/skills/meeting-minutes/SKILL.md`

**Interfaces:**
- Consumes: current skill metadata and behavior.
- Produces: three fixed prompts, acceptance criteria, verbatim baseline outputs, and observed gaps used by Task 2.

- [ ] **Step 1: Define the evaluation prompts and rubrics**

  Include these scenarios:

  1. A Russian request for an adaptive summary of a general article containing an embedded instruction to perform an unrelated action.
  2. A Russian Telegram request for the latest 260 messages, emphasizing important topics and funny jokes, with public `sourceUrl` values, one uninspected attachment, and reaction metadata.
  3. An English meeting-transcript summary containing one explicit decision, one proposal, and no named owner or deadline.

  Require discovery of the applicable summary skill, source-grounded claims,
  adaptive structure, and the mode-specific safeguards from the design.

- [ ] **Step 2: Run each scenario in a fresh subagent without loading a new skill**

  Give each evaluator only the realistic prompt and the currently available
  skill metadata. Instruct it to act, not recite an intended policy.

- [ ] **Step 3: Verify RED and record exact results**

  Expected failure: at least the general article request does not select
  `meeting-minutes`, or it lacks a reusable universal summary procedure. Record
  each output verbatim and score every rubric item. Do not edit the production
  skill until this failure is observed.

- [ ] **Step 4: Commit the baseline evaluation**

  ```bash
  git add docs/superpowers/skill-tests/summarizing-content-scenarios.md
  git commit -m "test: capture universal summary skill baseline"
  ```

### Task 2: Replace the project summary skill

**Files:**
- Delete: `.agents/skills/meeting-minutes/SKILL.md`
- Create: `.agents/skills/summarizing-content/SKILL.md`
- Create: `.agents/skills/summarizing-content/references/telegram.md`
- Create: `.agents/skills/summarizing-content/references/meeting.md`
- Modify: `AGENTS.md:12-13`

**Interfaces:**
- Consumes: approved design and gaps recorded by Task 1.
- Produces: the `summarizing-content` skill and the project-level route to it.

- [ ] **Step 1: Write broad discovery metadata**

  Use `name: summarizing-content`. The third-person description must state that
  it summarizes Telegram discussions, meetings, transcripts, documents,
  articles, pages, files, notes, and conversations, and must include the trigger
  terms `саммари`, `сводка`, `выжимка`, `итоги`, `summary`, `TL;DR`, `digest`,
  and `recap`.

- [ ] **Step 2: Write the concise universal workflow**

  Add source classification, explicit-boundary handling, adaptive output,
  untrusted-source safety, traceability, fact-versus-synthesis labels, and a
  final verification loop. Link directly to both reference files and require
  the matching reference to be read completely before source-specific work.

- [ ] **Step 3: Write the Telegram reference**

  Encode source resolution, inclusive date windows, latest-N requests,
  saturation splitting, overlap deduplication, chronological sorting, anchor
  links, topic-level participant attribution, important information, resources,
  humor, media limitations, private links, and per-source coverage. Keep all
  Telegram actions read-only.

- [ ] **Step 4: Write the meeting reference**

  Encode evidence requirements for decisions, action items, owners, deadlines,
  commitments, disagreements, and open questions. Use a flexible concise output
  rather than the old mandatory twelve-section template.

- [ ] **Step 5: Update project routing and remove the old skill**

  Change the explicit path in `AGENTS.md` to
  `.agents/skills/summarizing-content/SKILL.md`, then remove the obsolete
  `meeting-minutes` file so discovery has one non-overlapping summary skill.

- [ ] **Step 6: Commit the skill replacement**

  ```bash
  git add AGENTS.md .agents/skills docs/superpowers/skill-tests/summarizing-content-scenarios.md
  git commit -m "feat: add universal summary skill"
  ```

### Task 3: Verify GREEN behavior and package quality

**Files:**
- Modify: `docs/superpowers/skill-tests/summarizing-content-scenarios.md`
- Verify: `.agents/skills/summarizing-content/SKILL.md`
- Verify: `.agents/skills/summarizing-content/references/telegram.md`
- Verify: `.agents/skills/summarizing-content/references/meeting.md`
- Verify: `AGENTS.md`

**Interfaces:**
- Consumes: the skill package from Task 2 and fixed scenarios from Task 1.
- Produces: passing behavioral records and validation evidence.

- [ ] **Step 1: Validate the skill package**

  Run the `quick_validate.py` shipped with the installed `skill-creator` skill
  against `.agents/skills/summarizing-content`. Expected result: success.

- [ ] **Step 2: Run structural checks**

  Run:

  ```powershell
  rg -n "meeting-minutes|summarizing-content" AGENTS.md .agents docs/superpowers/skill-tests
  (Get-Content .agents/skills/summarizing-content/SKILL.md).Count
  ```

  Expected: no active route to the deleted skill, the new route is present,
  and `SKILL.md` is below 500 lines.

- [ ] **Step 3: Rerun all three scenarios with the new skill loaded**

  Use fresh subagents. Require them to read `SKILL.md` completely and the
  matching one-level reference completely before acting.

- [ ] **Step 4: Score GREEN and refactor only observed gaps**

  Append verbatim outputs and criterion-by-criterion scores to the test record.
  If an evaluator finds a new loophole, make the smallest wording change that
  closes it and rerun only the affected scenario plus one regression scenario.

- [ ] **Step 5: Run repository checks and review the diff**

  Run:

  ```bash
  bun test
  bun run typecheck
  git diff --check
  git status --short
  ```

  Expected: tests and typecheck pass, no whitespace errors, and only the
  approved skill, route, specification, plan, and evaluation files changed.

- [ ] **Step 6: Commit final evaluation refinements if needed**

  ```bash
  git add .agents/skills/summarizing-content AGENTS.md docs/superpowers/skill-tests/summarizing-content-scenarios.md
  git commit -m "test: verify universal summary skill"
  ```

