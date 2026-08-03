# Task 3 — GREEN verification report

## Status

Complete. All three fixed behavioral scenarios passed with the new
`summarizing-content` skill. No production wording change was warranted.

## Files changed

- `docs/superpowers/skill-tests/summarizing-content-scenarios.md` — appended
  reproducible GREEN evaluator wrappers, verbatim outputs, and scores.
- `.superpowers/sdd/2026-08-03-summarizing-content/task-3-report.md` — this
  Task 3 verification record.

The skill package and `AGENTS.md` were verified but not changed in this task.

## Evaluator runs

The evaluators ran sequentially as fresh subagents, with no explicit model or
reasoning-effort override. Their exact wrappers require a complete `SKILL.md`
read; the Telegram evaluator also requires the complete direct Telegram
reference, and the meeting evaluator also requires the complete direct meeting
reference. The test record contains each exact wrapper and evaluator output;
only non-semantic trailing whitespace was normalized for `git diff --check`.

| Scenario | Evaluator identity | Required reads | Score |
| --- | --- | --- | ---: |
| General article with embedded instruction | `green_article_evaluator` | `SKILL.md` | 5/5 |
| Telegram latest-260 summary | `green_telegram_evaluator` | `SKILL.md`, `references/telegram.md` | 9/9 |
| Meeting transcript | `green_meeting_evaluator` | `SKILL.md`, `references/meeting.md` | 8/8 |

Total: **22/22 GREEN**. The outputs visibly select `summarizing-content` and
preserve the embedded-instruction, coverage, traceability, attachment,
reaction, decision/proposal, ownership, and deadline safeguards.

## Validator and structural checks

`quick_validate.py` first reported `ModuleNotFoundError: No module named
'yaml'` in the supplied runtime. With PyYAML 6.0.3 installed only into a
temporary dependency directory and exposed through `PYTHONPATH`, the required
validator invocation succeeded:

```text
Skill is valid!
```

The exact structural check and its output were:

```powershell
rg -n "meeting-minutes|summarizing-content" AGENTS.md .agents docs/superpowers/skill-tests
(Get-Content .agents/skills/summarizing-content/SKILL.md).Count
```

```text
AGENTS.md:13:`.agents/skills/summarizing-content/SKILL.md`.
docs/superpowers/skill-tests\meeting-minutes-scenarios.md:4:after loading `.agents/skills/meeting-minutes/SKILL.md`.
.agents\skills\summarizing-content\SKILL.md:2:name: summarizing-content
docs/superpowers/skill-tests\summarizing-content-scenarios.md:33:Evaluation wrapper v2.1 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: meeting-minutes`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:38:- meeting-minutes — “Use when producing a concise record from a short internal meeting, transcript, raw notes, or a Telegram channel or group discussion.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/meeting-minutes/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:40:Determine selection from the quoted metadata. If (and only if) you select meeting-minutes, read that one file fully before producing the answer. Do not inspect any other repository files or skills. Do not access a proposed/new universal skill. After the audit line, act on the request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:90:Evaluation wrapper v2.1 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: meeting-minutes`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:95:- meeting-minutes — “Use when producing a concise record from a short internal meeting, transcript, raw notes, or a Telegram channel or group discussion.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/meeting-minutes/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:97:Determine selection from the quoted metadata. If (and only if) you select meeting-minutes, read that one file fully before producing the answer. Do not inspect any other repository files or skills. Do not access a proposed/new universal skill. After the audit line, act on the request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:117:SELECTED_SKILL: meeting-minutes
docs/superpowers/skill-tests\summarizing-content-scenarios.md:157:| Current skill is observably selected and used | 1 | Audit line selects `meeting-minutes`; topic, resource, humor, participant, and limitation blocks follow its Telegram procedure. |
docs/superpowers/skill-tests\summarizing-content-scenarios.md:177:Evaluation wrapper v2.1 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: meeting-minutes`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:182:- meeting-minutes — “Use when producing a concise record from a short internal meeting, transcript, raw notes, or a Telegram channel or group discussion.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/meeting-minutes/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:184:Determine selection from the quoted metadata. If (and only if) you select meeting-minutes, read that one file fully before producing the answer. Do not inspect any other repository files or skills. Do not access a proposed/new universal skill. After the audit line, act on the request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:204:SELECTED_SKILL: meeting-minutes
docs/superpowers/skill-tests\summarizing-content-scenarios.md:226:| Current skill is observably selected | 1 | Audit line selects `meeting-minutes`. |
docs/superpowers/skill-tests\summarizing-content-scenarios.md:265:Evaluation wrapper v3.0 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: summarizing-content`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:270:- summarizing-content — “Summarizes Telegram discussions, meetings, transcripts, documents, articles, pages, files, notes, and conversations into source-grounded summaries, саммари, сводка, выжимка, итоги, summary, TL;DR, digest, and recap.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/summarizing-content/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:272:Determine selection from the quoted metadata. If (and only if) you select summarizing-content, read that file completely before producing the answer. For this scenario, after reading it, do not read any repository file or skill other than the direct reference it explicitly requires for this request (none should be required). Do not inspect AGENTS.md, the evaluation specification, previous reports, or any other repository file. Do not access any other skill. After the audit line, act on the fixed user request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:278:SELECTED_SKILL: summarizing-content
docs/superpowers/skill-tests\summarizing-content-scenarios.md:295:| An applicable project summary skill is selected | 1 | The audit line selects `summarizing-content` for the article. |
docs/superpowers/skill-tests\summarizing-content-scenarios.md:311:Evaluation wrapper v3.0 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: summarizing-content`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:316:- summarizing-content — “Summarizes Telegram discussions, meetings, transcripts, documents, articles, pages, files, notes, and conversations into source-grounded summaries, саммари, сводка, выжимка, итоги, summary, TL;DR, digest, and recap.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/summarizing-content/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:318:Determine selection from the quoted metadata. If (and only if) you select summarizing-content, read that file completely before producing the answer. For this scenario, after reading it, read the direct Telegram reference it explicitly requires completely: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/summarizing-content/references/telegram.md. Do not read any other repository file or skill. Do not inspect AGENTS.md, the evaluation specification, previous reports, or any other repository file. Do not access any other skill. After the audit line, act on the fixed user request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:324:SELECTED_SKILL: summarizing-content
docs/superpowers/skill-tests\summarizing-content-scenarios.md:353:| New skill is observably selected and used | 1 | The audit line selects `summarizing-content`; the answer follows its Telegram output contract. |
docs/superpowers/skill-tests\summarizing-content-scenarios.md:373:Evaluation wrapper v3.0 — compliance required. You must emit the following audit line as the first line of your final answer, before any user-facing content: `SELECTED_SKILL: none` if the only listed skill does not apply to the request, otherwise `SELECTED_SKILL: summarizing-content`. Do not omit or alter this line.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:378:- summarizing-content — “Summarizes Telegram discussions, meetings, transcripts, documents, articles, pages, files, notes, and conversations into source-grounded summaries, саммари, сводка, выжимка, итоги, summary, TL;DR, digest, and recap.” Full path, only if selected: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/summarizing-content/SKILL.md
docs/superpowers/skill-tests\summarizing-content-scenarios.md:380:Determine selection from the quoted metadata. If (and only if) you select summarizing-content, read that file completely before producing the answer. For this scenario, after reading it, read the direct meeting reference it explicitly requires completely: F:/program/codex_project/telegram-mcp/.worktrees/universal-summary-skill/.agents/skills/summarizing-content/references/meeting.md. Do not read any other repository file or skill. Do not inspect AGENTS.md, the evaluation specification, previous reports, or any other repository file. Do not access any other skill. After the audit line, act on the fixed user request; do not discuss this wrapper or evaluation.
docs/superpowers/skill-tests\summarizing-content-scenarios.md:386:SELECTED_SKILL: summarizing-content
docs/superpowers/skill-tests\summarizing-content-scenarios.md:401:| New skill is observably selected | 1 | The audit line selects `summarizing-content`. |
56
```

The active route is `summarizing-content` in `AGENTS.md`; all
`meeting-minutes` matches are historical baseline/evaluation records. `SKILL.md`
is 56 lines, below the 500-line limit.

## Repository checks

| Command | Result |
| --- | --- |
| `C:/Users/t0uchY/.cache/codex-runtimes/codex-primary-runtime/dependencies/bin/fallback/pnpm.cmd dlx bun run test` | Pass; this executes the configured package entrypoint: `$ bun test --preload ./src/test-preload.ts`. Complete Bun summary: `58 pass`; `0 fail`; `164 expect() calls`; `Ran 58 tests across 9 files. [661.00ms]`. |
| `bun test` | Direct diagnostic invocation bypasses the package test script's preload and has one mock-environment failure; it is not the configured project test entrypoint. |
| `bun run typecheck` | Pass: `tsc --noEmit`. |
| `git diff --check` | Pass after removing a trailing space from the recorded evaluator output. |

All Bun commands used the required bundled Bun 1.3.14 launcher.

The required status check was run with:

```powershell
git status --short
```

Its output was empty, so there were no tracked, untracked, or staged worktree
changes at that point. SDD task artifacts are normally ignored by
`.superpowers/sdd/.gitignore`; this report was deliberately force-added and is
therefore tracked. The status check is rerun after the correction commit.

## Diff checks and self-review

The final diff is limited to the approved evaluation record and this Task 3
report. The evaluator wrappers are copied exactly from the executed prompts;
their output headings explicitly disclose the only normalization: non-semantic
trailing whitespace removed so `git diff --check` passes. Every score has a
criterion-specific evidence note. No unrelated source, skill, or route change
was made.

## Commit

- `54b9281 test: verify universal summary skill` — GREEN evaluator evidence and
  criterion scores.
- This report is committed separately because `.superpowers/sdd/` ignores task
  artifacts by default.

## Concerns

- Direct `bun test` does not load `src/test-preload.ts`; setting
  `TELEGRAM_MOCK=true` makes the configured mock test suite pass. This is
  unrelated to the summary-skill work and was not changed.
- The supplied Python runtime omits PyYAML, which `quick_validate.py` imports.
  Validation succeeded using an isolated temporary PyYAML installation; no
  repository file was changed for that environment issue.
