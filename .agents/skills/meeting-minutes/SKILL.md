---
name: meeting-minutes
description: Use when producing a concise record from a short internal meeting, transcript, raw notes, or a Telegram channel or group discussion.
---

# Meeting Minutes Skill — Short Internal Meetings

## Purpose / Overview

This Skill produces high-quality, consistent meeting minutes for internal meetings that are 60 minutes or shorter. Output is designed to be clear, actionable, and easy to convert into task trackers (e.g., GitHub Issues, Jira). The generated minutes prioritize decisions and action items so teams can move quickly from discussion to execution.

## Telegram Discussion Summary Mode

Use this mode when the source is a Telegram channel or group. It replaces the meeting-specific intake questions and the Strict Minutes Schema below: do not ask for an organizer, agenda, reviewer, action items, or a next meeting unless the Telegram messages themselves establish them.

The core unit of the summary is a topic with its participants, positions, humor, and resources kept together. This preserves the social context of the discussion instead of reducing it to an anonymous list of conclusions.

### Topic block

Use this shape for every substantive topic:

```markdown
### <Topic>

- **What was discussed:** <concise synthesis>
- **Active participants and positions:**
  - @handle — <main claims, arguments, examples, or disagreement>
- **Outcome / disagreement:** <supported conclusion or unresolved split>
- **Resources:**
  - <exact URL> — <why it mattered in this topic> — <source message link>
- **From the chat:**
  > "<verbatim joke or meme quote>" — @handle, <reaction count>, <source message link>
```

Apply these rules:

1. **Attribute every topic.** Name the people who materially advanced that topic and summarize the main thought each person contributed. Message volume alone does not make someone an active participant. A global participant table may supplement this, but never replace topic-level attribution.
2. **Use humor as evidence of the chat's tone.** Quote a topical joke or meme verbatim when it naturally fits the topic. A context-free joke may appear in a separate `From the chat` interlude only when verified reaction metadata totals more than 4 reactions. Prefer the strongest 1–3 quotes across the whole summary so humor adds texture without taking over.
3. **Never infer reactions.** Show a reaction count only when the source payload provides it. If reaction metadata is unavailable, do not claim that a quote passed the reaction threshold and do not use it as a context-free interlude.
4. **Keep resources with their discussion.** Place GitHub repositories, documentation, articles, tools, videos, and other links inside the topic where they were mentioned. Preserve URLs exactly and add the source Telegram message link when possible.
5. **Reserve a standalone resources section for orphan links.** Use `Standalone resources` only for links posted without enough surrounding discussion to assign them to a topic. Do not duplicate topic resources there.
6. **Preserve quotation fidelity.** Do not rewrite, correct, sanitize, or merge a quoted joke. Keep it short, identify the author, and link the source message when possible.

### Compact example

```markdown
### Agent autonomy and review

- **What was discussed:** A more autonomous model finds deeper defects but may widen the task.
- **Active participants and positions:**
  - @anna — uses architecture linting and CI to catch unauthorized changes.
  - @boris — separates read-only audit from an explicitly authorized fix pass.
- **Outcome / disagreement:** The group preferred external checks over relying only on prompt instructions.
- **Resources:**
  - https://github.com/example/arch-lint — architectural guardrails — <source message>
- **From the chat:**
  > "You completed 50% of success, but your limits are over." — @cat, 7 reactions, <source message>
```

## When to Use

Use this skill when:

- Internal syncs, standups, design reviews, triage, planning or ad-hoc meetings with short duration
- Situations that require a concise record of decisions, assigned action items, and follow-ups
- Creating a standardized minutes document from a live meeting, transcript, recording, or notes
- Summarizing Telegram channels or groups into topics, participant positions, useful resources, and representative humor

---

## Operational Workflow

### Phase 1: Intake (before drafting)

- Obtain meeting metadata: title, date, start/end time (or duration), organizer, and intended audience.
- Confirm available inputs: agenda, slides, recording, transcript, or raw notes.
- If key details are missing, ask up to 3 clarifying questions before producing minutes (see "Discovery" below).

### Phase 2: Capture (during / immediately after meeting)

- Record attendees and absentees.
- Capture brief notes per agenda item with time markers if available.
- Record explicit decisions, rationale summary (1–2 sentences), and action items (owner + due date).

### Phase 3: Drafting

- Generate minutes following the **Strict Minutes Schema** (below).
- Ensure every action item includes owner, due date (or timeframe), and acceptance criteria when applicable.
- Mark unresolved issues or items requiring follow-up in the Parking Lot.

### Phase 4: Review & Publish

- If possible, send draft to meeting organizer or a designated reviewer for quick verification (within 24 hours).
- Publish final minutes to the agreed channel (shared drive, repo, ticket, or email) and optionally create tasks in the team's tracker.

---

## Discovery (required clarifying questions)

Before generating minutes, the agent **MUST** ask up to three clarifying questions if any of these are missing:

- What is the meeting title, date, start time (or duration), and organizer?
- Is there an agenda or transcript/recording to reference? If yes, please provide.
- Who should be assigned as the reviewer or approver for the minutes?

If the user responds "no transcript" or "no agenda," proceed but mark source material as "ad-hoc notes" and flag potential gaps.

---

## Strict Minutes Schema (Output Structure)

You **MUST** produce meeting minutes following this exact structure. If information is unavailable, use `TBD` or `Unknown` and explain how to obtain it.

### 1. Metadata

- **Title**:
- **Date (YYYY-MM-DD)**:
- **Start Time (UTC)**:
- **End Time (UTC) or Duration**:
- **Organizer**:
- **Location / Virtual Link**:
- **Minutes Author** (agent or person):
- **Distribution List** (who receives the minutes):

### 2. Attendance

- **Present**: [list of names + roles]
- **Regrets / Absent**: [list]
- **Notetaker / Recorder**: [name or "agent"]

### 3. Agenda

Bullet list of agenda items, in order:

- Item 1: short title
- Item 2: short title
- ...

### 4. Summary

A concise one-paragraph summary (1–3 sentences) of the meeting's objective and high-level outcome.

### 5. Decisions Made

Each as a separate bullet:

- **Decision 1**: statement of decision.
  - Who decided / approved: [name(s) or group]
  - Rationale (1–2 sentences): brief reason.
  - Effective date (if applicable): YYYY-MM-DD
- **Decision 2**: ...

### 6. Action Items

Table-style bullets; **must include owner and due date**:

- **[ID] Action**: short description
  - **Owner**: Name (team)
  - **Due**: YYYY-MM-DD or "ASAP" / timeframe
  - **Acceptance Criteria**: (what completes this action)
  - **Linked artifacts / tickets**: (optional URL or ticket id)

**Example:**

- [A1] Draft deployment runbook for feature X
  - Owner: Alex (Engineering)
  - Due: 2026-02-05
  - Acceptance Criteria: runbook includes steps for rollback, health checks, and monitoring links
  - Linked artifacts: https://github.com/owner/repo/issues/123

### 7. Notes by Agenda Item

Brief, factual, timestamp optional:

- **Agenda Item 1**: title
  - Key points:
    - Point A (timestamp 00:05)
    - Point B (timestamp 00:12)
  - Open issues / questions:
    - Q1: question text (owner if assigned)
- **Agenda Item 2**: ...

### 8. Parking Lot / Unresolved Items

- **Item**: short description
  - Why parked / next step:
  - Suggested owner or next meeting to resolve

### 9. Risks / Blockers (if any)

- **Risk 1**: short description, impact, mitigation owner
- **Risk 2**: ...

### 10. Next Meeting / Follow-up

- Proposed date/time (if any)
- Objectives for next meeting

### 11. Attachments / References

- Agenda document: URL
- Slides: URL
- Transcript / Recording: URL
- Related tickets: list of URLs or IDs

### 12. Version & Change Log

- **Version**: 1.0
- **Last updated**: YYYY-MM-DDTHH:MM:SSZ
- **Changes**: short notes on edits and who made them

---

## Style & Quality Rules

- Keep minutes concise: total length should typically be under 1 A4 page for meetings <= 30 minutes and under 2 pages for meetings close to 60 minutes.
- Use plain language and bullet lists for readability.
- Prioritize decisions and action items at the top of the document.
- Do NOT include speculative language or unverified claims. If something is uncertain, label it `TBD` and note the missing info source.
- Use consistent timestamps and ISO 8601 dates (YYYY-MM-DD or full UTC timestamp).

---

## DO / DON'T

**DO:**

- Include owner and due date for every action item.
- Provide acceptance criteria for action items when possible.
- Link to artifacts (tickets, slides, recordings) for traceability.
- Send draft for quick review if minutes contain significant decisions.

**DON'T:**

- Omit decisions or action items — these are the primary value of minutes.
- Mix personal opinions with facts. Keep commentary clearly marked as "Opinion" or exclude it.
- Publish raw PII gathered during discussion unless required and authorized.

---

## Example Prompts (for Copilot / Agent)

**Prompt to generate minutes from transcript:**

> "Generate meeting minutes from the following meeting transcript. Meeting title: 'Platform Weekly Sync'. Date: 2026-02-10. Duration: 45 minutes. Organizer: Priya (Platform Lead). Transcript: <paste transcript>. Follow the Strict Minutes Schema. Highlight decisions and create action items with owners and due dates where implied."

**Prompt to generate minutes from notes:**

> "I have raw notes from a 30-minute design review. Title: 'Feature Y Design Review'. Date: 2026-02-11. Notes: <paste notes>. Produce concise minutes following the Strict Minutes Schema. Ask up to 3 clarifying questions if critical fields are missing."

---

## Quick Templates (copyable)

### Concise minutes template (short):

```
- Title:
- Date:
- Organizer:
- Present:
- Summary:
- Decisions:
  - Decision 1 — Who — Effective:
- Action Items:
  - [A1] Action — Owner — Due — Acceptance Criteria
- Next Steps / Next Meeting:
```

### Detailed minutes template (full schema):

Use the Strict Minutes Schema above.

---

## Verification & Acceptance Criteria for Generated Minutes

A generated minutes document is acceptable if:

- It contains Metadata, Attendance, Decisions, and Action Items sections.
- Every action item has an assigned owner and a due date or a clear timeframe.
- All significant decisions are captured with at least 1-line rationale.
- Attachments or references are listed or explicitly marked `None`.
- The document is factual; uncertain items are labeled `TBD`.

For a Telegram discussion summary, use these acceptance criteria instead:

- Every substantive topic names its active participants and states each participant's main contribution.
- Every topical resource appears inside its topic with context; only orphan links appear under `Standalone resources`.
- Topical humor is quoted verbatim. Any context-free joke included for popularity has a verified reaction total greater than 4.
- Reaction counts, consensus, roles, and source links are never invented.
