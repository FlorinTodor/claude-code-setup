# Context cheat sheet: why you hit the limit and what to do

Everything in a session lives in one context window: every message, every file Claude read, every command output. As it fills, Claude gets worse, and you burn more on every message. Most "I hit the limit" moments are context problems, not quota problems.

## Commands

| Command | Use it when |
| --- | --- |
| `/clear` | Switching to an unrelated task. Resets the context entirely. |
| `/compact` | The session is long but you need to keep going. Claude summarises what matters. |
| `/compact Focus on the API changes` | Same, but you decide what survives the summary. |
| `/context` | You want to see what is loaded: CLAUDE.md files, rules, memory, and how full the window is. |
| `/btw <question>` | A side question whose answer should not stay in the conversation. |
| `Esc` | Stop Claude mid-action. Context is kept, you redirect. |
| `Esc Esc` or `/rewind` | Go back to a checkpoint: restore conversation, code, or both, or summarise from a message. |
| `/memory` | See and edit every CLAUDE.md and the auto memory folder. |

Auto compaction also kicks in on its own near the limit. To control what it keeps, put a line in CLAUDE.md such as "When compacting, always preserve the list of modified files and the test command".

## Habits that keep the window small

- `/clear` between unrelated tasks. The "kitchen sink session" is the most common failure.
- After two failed corrections on the same thing, `/clear` and write a better first prompt with what you learned.
- Scope investigations: "look at src/auth/token.py", not "investigate auth".
- Delegate reading to subagents: "use a subagent to find how token refresh works". They read in their own context and report a summary.
- Keep CLAUDE.md under 200 lines. Imports with `@file` do not save context: imported files load too.
- Reference files with `@path` instead of pasting them when they are big; Claude reads what it needs.

## Check your setup loaded

Run `/context` and look under **Memory files**. If your CLAUDE.md or a rule is not listed, Claude cannot see it.

Verified against the official docs on 2026-10-09:
https://code.claude.com/docs/en/best-practices
https://code.claude.com/docs/en/memory
