---
name: handoff
description: Use when the user is ending a session and wants to continue later: says "handoff", "give me a prompt for tomorrow / the next session", "how do I resume this", or "wrap up and tell me how to pick this up".
---

# Handoff

Save the session to repo memory, then hand the user one short prompt that points a new session at that memory.

**REQUIRED SUB-SKILL:** run `compound` first. Skip only if `docs/session-logs.md` already has today's line for this thread and it is committed.

The prompt is a pointer, not a copy. Every noun in it comes from the L0 line compound just wrote, or from the files that line points to. Nothing else exists.

## Prompt shape

```
Continue <area/thread> from <date>. Read the top line of docs/session-logs.md and follow its → links before anything else.
State: <STATE clause from L0>.
Waiting on me: <pending: items from L0>.
First: <one action from the table>.
Then: <next: from L0>.
```

≤80 words. Omit an empty line (no pending → no "Waiting on me"). No file lists. The → links carry them. No steps, tools, IDs, or wording the user did not say this session.

## First action, by STATE

| L0 STATE | First |
|---|---|
| `UNCOMMITTED` | Show me the diff for review. Do not commit until I say yes. |
| `pending:` present | Ask me the pending decisions: 2 options each, your pick first. |
| `OPEN` | Read the backlog entry, then propose the smallest fix. |
| `SHIPPED` | Start on "Then". |

`UNCOMMITTED` wins over `pending:`; the pending items ride along in "Waiting on me".

## Procedure

1. Run `compound`.
2. Take the L0 line it printed. Fill the shape from its fields.
3. Print the prompt in one fenced block. Nothing else: no file, no clipboard.
