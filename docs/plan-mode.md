# Plan mode cheat sheet

Plan mode tells Claude to research and propose changes without making them. It reads files, runs commands to explore, and writes a plan. Edits stay blocked until you approve the plan.

## Turn it on

| How | What it does |
| --- | --- |
| `Shift+Tab` | Cycles permission modes. From auto: `default` → `acceptEdits` → `plan`. Status bar shows `⏸ plan mode on`. |
| `/plan <your prompt>` | Plan mode for that one prompt. |
| `claude --permission-mode plan` | Start the session in plan mode. |
| `"defaultMode": "plan"` in `.claude/settings.json` | Every terminal session in that project starts in plan mode. |

In VS Code the project setting is not read for the starting mode. Set `claudeCode.initialPermissionMode` to `plan` in your VS Code user settings instead.

## While planning

- Ask it to read, not to do: "read src/auth and explain how sessions work". Then: "I want to add Google OAuth. What files change? Create a plan."
- `Ctrl+G` opens the proposed plan in your editor so you can edit it before Claude continues.
- `Shift+Tab` again leaves plan mode without approving anything.

## When the plan is ready

Claude asks how to proceed:

- **Yes, and use auto mode** (or **Yes, auto-accept edits** where auto mode is not available): approve and let it edit.
- **Yes, manually approve edits**: approve the plan, review each edit.
- **No, keep planning**: stay in plan mode and tell it what to change.

Approving exits plan mode. To plan again later, `Shift+Tab` back or prefix the prompt with `/plan`.

## When to skip it

From the official best practices: if you could describe the diff in one sentence, skip the plan. Plan when the change touches several files, when you are unsure of the approach, or when you do not know the code.

## The workflow that works

1. **Explore** in plan mode: "read X, understand Y".
2. **Plan**: "what needs to change? create a plan". Edit it with `Ctrl+G` if needed.
3. **Implement**: approve, then "implement the plan, write tests, run them, fix failures".
4. **Commit**: "commit with a descriptive message".

Verified against the official docs on 2026-10-09:
https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode
https://code.claude.com/docs/en/best-practices
