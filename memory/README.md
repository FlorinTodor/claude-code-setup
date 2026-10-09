# Auto memory: how it is laid out and how to keep it useful

Claude Code has two memory systems and people mix them up:

| | `CLAUDE.md` | Auto memory |
| --- | --- | --- |
| Who writes it | You | Claude |
| What goes in | Instructions and rules | Things it learned: your corrections, your preferences, decisions |
| Where it lives | In the repo (or `~/.claude/CLAUDE.md`) | `~/.claude/projects/<project>/memory/` on your machine |
| Loaded | Every session, in full | Every session, first 200 lines or 25 KB of `MEMORY.md` |

Auto memory is on by default in local sessions. Run `/memory` inside a session to toggle it, open the folder, or see every memory file Claude is reading.

## The folder

```text
~/.claude/projects/<project>/memory/
├── MEMORY.md             # index: one line per memory, this is what loads at start
├── user-role.md          # one memory per file
├── feedback-testing.md
└── project-billing-v2.md
```

`<project>` is derived from the git repository, so all worktrees and subfolders of one repo share the same memory. Outside git it is the folder you launched from.

## The four kinds of memory

Claude tags each file with a `type` in its frontmatter:

- `user`: your role, expertise, working preferences.
- `feedback`: corrections you gave and approaches you confirmed. These are the valuable ones.
- `project`: ongoing work, deadlines, decisions Claude cannot derive from the code or git history.
- `reference`: where to find things outside the repo (issue tracker, dashboards, docs).

Claude skips anything it can read from the code (architecture, file paths, past fixes) and anything CLAUDE.md already says.

## A memory file

See `project-example.md` next to this file. The shape is:

```markdown
---
name: project-billing-v2
description: Billing refactor on feat/billing-v2, due end of October 2026
type: project
---

The billing refactor replaces billing_legacy/ with src/billing/. Target: end of October 2026.

**Why:** the old module has no tests and bills twice on retry.
**How to apply:** do not touch billing_legacy/ except to delete it; new code goes in src/billing/.
```

And one line in `MEMORY.md`:

```markdown
- [Billing refactor](project-billing-v2.md) — feat/billing-v2, due end of October 2026
```

## Making Claude remember something on purpose

Say it in plain words: "remember that the API tests need a local Redis". Claude writes it to auto memory. If you want it in `CLAUDE.md` instead, say "add this to CLAUDE.md".

## Keeping it small

`MEMORY.md` is read up to 200 lines or 25 KB. Past that, nothing loads. Keep it one line per memory and move the detail into topic files. Claude Code reminds Claude to trim it when it gets close.

## Turning it off

- For one project: `"autoMemoryEnabled": false` in that project's `.claude/settings.json`.
- Everywhere: the toggle in `/memory`, or `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`.
- Move it: `"autoMemoryDirectory": "~/my-memory"` in any settings file.

Verified against the official docs on 2026-10-09: https://code.claude.com/docs/en/memory
