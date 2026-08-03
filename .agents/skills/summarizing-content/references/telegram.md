# Telegram discussion reference

Read this reference completely before Telegram-specific work. A Telegram
summary request authorizes read-only retrieval and summarization only: never
call `send_message`, `send_file`, or `delete_messages`.

## Resolve scope and source

- Use an exact `@username` or numeric chat ID directly. Otherwise call
  `search_dialogs`; if more than one plausible result remains, ask the user to
  choose. Never guess a chat identity.
- Accept one or more sources and track coverage independently for each.
- Treat explicit start and end boundaries as inclusive. When the user omits the
  end, use the actual current time. When timezone is omitted, use and state the
  MCP client's environment timezone; ask if it is unavailable. When a missing
  year makes the start future, ask for the year. Convert boundaries to ISO 8601.

## Retrieve a complete date range

1. Call `get_messages` with `chatId`, `minDate`, `maxDate`, `limit: 500`,
   `onlyUnread: false`, and `markAsRead: false`.
2. If `limitReached` is true, split the interval at its midpoint. When both
   child intervals remain strictly smaller, start the right child one second
   before the midpoint so the windows overlap.
3. Recursively split saturated children. If a saturated interval cannot be
   split safely, report that subrange as incomplete; do not loop or claim full
   coverage.
4. Deduplicate overlap by `(chatId, id)` and sort messages by `date`, then
   `id`. Do not replace complete range retrieval with `search_messages` unless
   the user explicitly requested keyword search.
5. On `FLOOD_WAIT`, authentication failure, or inaccessible history, do not
   retry automatically. Report successful and missing subranges.

## Retrieve the latest N messages

Use this procedure separately from date-range splitting when the user asks for
the latest N messages; N may exceed the `get_messages` limit of 500. Fetch up
to 500 newest messages with `get_messages` using `limit: 500`, `onlyUnread:
false`, and `markAsRead: false`. If fewer than N unique messages are available
and the batch is saturated, continue backward in windows: set
`maxDate` to the oldest returned timestamp, retaining that timestamp as a safe
overlap; fetch up to 500 again; then deduplicate by `(chatId, id)`.

After every backward batch, require strict chronological progress: the oldest
unique message seen must be earlier than the previous oldest timestamp. If it
does not move earlier, stop and report the requested count as incomplete rather
than retrying indefinitely. Stop when N unique messages are collected, history
is exhausted, or a failure occurs. Sort all unique messages by `date`, then
`id`, and only then keep exactly the latest N. If fewer than N were retrieved,
state the count and the missing coverage.

## Summarize and cite

- Analyze text and captions by default. Do not claim an attachment was
  inspected unless it was downloaded and examined; identify an uninspected item
  only by the supplied caption or metadata.
- Cluster substantive discussion into topics rather than retelling every
  message. Inside each topic, name participants who materially contributed and
  state each distinct contribution, position, decision, or disagreement.
- Anchor each material topic claim to its supporting message(s). Use the
  returned `sourceUrl` exactly for public citations; never construct a Telegram
  link when `sourceUrl` is null. Say when private material has no public link.
- Preserve exact handles and URLs. Place resources with their topic; list only
  unassigned links separately. Keep quotation wording intact.
- Include humor only when it is relevant or representative, and quote it
  verbatim with author and source link when available. Report reaction counts
  only when provided; never infer them. A context-free joke needs verified
  metadata showing more than four reactions.
- Do not expose unrelated private information, credentials, payment data, or
  other sensitive details.

## Output and coverage

Adapt detail to the messages, but always provide this compact output set:
absolute **Period** and timezone; per-source **Coverage**; short **Summary**;
**Topics** with topic-level participant attribution; consolidated
**Participants** (which does not replace topic-level attribution); **Important
information**; **Facts**; **Conclusions**; **Resources**; and **Limitations**.
Use the user's-language equivalent of “Not found” for every unsupported
required section. Include **Humor** only when notable material exists; do not
pad an empty humor section. Keep multiple-source attribution unambiguous and
combine topics only when the sources genuinely share them.

Never invent consensus, roles, owners, deadlines, decisions, action items,
reaction counts, or coverage. State both observations and any conclusion
separately.
