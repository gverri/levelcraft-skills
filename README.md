# LevelCraft skills

Free skills for Claude Code from [LevelCraft](https://levelcraft.ai). We use them every day.

| Skill | What it does | Type |
|---|---|---|
| `compound` | At the end of a session, saves what happened into your project's notes: what changed, what is still open and what comes next. A new session reads one line and knows where you stopped. | `/compound` |
| `handoff` | Runs `compound`, then gives you one short prompt to paste into your next session. | `/handoff` |

## Install

Paste this into Claude Code:

```
Install the compound and handoff skills from https://github.com/gverri/levelcraft-skills

1. Read both SKILL.md files first. Tell me in plain English what each one does.
2. Copy the folders skills/compound and skills/handoff into my personal skills folder (~/.claude/skills on Mac, %USERPROFILE%\.claude\skills on Windows), so they work in all my projects.
3. Change nothing else. When you are done, tell me to start a new session.
```

Then start a new session. At the end of your next work session, type `/handoff` and keep the prompt it gives you.

## How they save your notes

`compound` keeps one short line per session in `docs/session-logs.md` and links that file from your project's `CLAUDE.md` (or `AGENTS.md`), so every new session reads it. It writes only facts from the session, never guesses. If your project uses git, it commits only the notes it changed.

## License

MIT
