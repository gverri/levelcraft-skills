---
name: compound
description: Use when a session is ending and the repo must remember what happened: the user says "compound", "wrap up", "save progress", "handoff", or is leaving with work mid-flight, uncommitted, or a decision open.
---

# Compound

Turn this session into repo memory that a later agent (Claude, Codex, a session on another topic) loads cheaply and follows deeper only when needed.

**Source of truth is the transcript.** A fact goes into memory only if it happened in this session. Do not read the repo to fill gaps. If something is unknown, leave it out. Never guess.

## Three layers

| Layer | What | Where | Budget |
|---|---|---|---|
| L0 | One line for this session, always loaded | `docs/session-logs.md`, `@`-imported from `AGENTS.md` (else `CLAUDE.md`) | ≤12 lines in the file, ≤40 words per line |
| L1 | What + why, per area | The repo's living docs: `STATUS.md`, `backlog.md`, `known-issues.md`, decision log, `patterns.md` | ≤60 words per fact, edited in place |
| L2 | The work itself | Code, tests, commits, task packets | Name the path. Never describe what is inside it. |

No new files. The only file this skill ever creates is `docs/session-logs.md`, once.

Session state words (`UNCOMMITTED`, `pending`, `OPEN`) live only in L0 and `STATUS.md`. Durable docs (`patterns.md`, `known-issues.md`, decision log) carry facts, never state.

## Where each fact goes

| Fact from the transcript | Home |
|---|---|
| Shipped work | area `STATUS.md` + commit hash |
| Work in flight; next step the user asked for | area `STATUS.md` Active/Next |
| Open bug or blocker with a decision pending | `backlog.md`, at the priority the repo's own definition gives it |
| Bug fixed this session | `known-issues.md` (what/why/how/risk). It is usually already written: check it, do not rewrite it |
| Decision the user said yes to, in the transcript | decision log |
| Decision proposed, not approved | `pending:` field of the L0 line, nowhere else |
| Durable fact learned about a library or system | `patterns.md` or the nearest existing doc, one paragraph |
| How the user likes to work | Not the repo. Use personal memory (`~/.claude/CLAUDE.md`) |
| Dirty working tree | `UNCOMMITTED (<why>)` in the L0 line, nowhere else |

## L0 line

One line per session. A second line only for a topic unrelated to the first. Date = today.

```
- YYYY-MM-DD · area/thread · what changed (IDs) · STATE · pending: <decision> · open: <bug> · next: <step> · → path, path
```

STATE ∈ `SHIPPED <hash>` · `UNCOMMITTED (<why>)` · `OPEN`. Omit empty fields. Newest first. Delete a line when its thread is closed: git history keeps it.

File header, written once:
```
# Session logs
One line per session, newest first. Follow → for detail. Closed threads are deleted.
```

## Procedure

1. `grep -n '^@' CLAUDE.md AGENTS.md`; `ls docs/*/STATUS.md docs/*/backlog.md`; locate `known-issues`, decision log, `patterns`. If `docs/session-logs.md` is missing, create it and add `@docs/session-logs.md` to `AGENTS.md` (else `CLAUDE.md`).
2. `git status --porcelain`; `git log --oneline -10`. These are the only repo reads. If the folder is not a git repo, skip the git steps and say so.
3. Bucket the session's facts: shipped · in flight · open · decided · proposed · learned · next · dirty paths.
4. Write each to its home from the table.
5. Write the L0 line. Prune closed lines.
6. If the folder is a git repo: `git add` only the files edited in steps 1 to 5; `git commit -m "docs: compound YYYY-MM-DD, <thread>"`. Code paths stay unstaged.
7. Print the L0 line to the user.

## What memory is not

A `docs/status-<date>.md` diary. A "rebuild from this" copy of the code. A recipe you did not run this session. A decision-log row for a call the user did not approve. A backlog entry for "uncommitted work".
