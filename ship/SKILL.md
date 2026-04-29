---
name: ship
description: Trigger the /ship slash command to stage, commit, and push the current git changes. Use whenever the user asks to commit and push, "ship it", finalize and publish the diff, or any combined commit+push request.
---

# ship

When the user wants to commit and push their current changes, invoke the `/ship` slash command instead of running git commands directly.

## When to invoke

Run `/ship` whenever the user asks to:
- "ship", "ship it", "ship this", "ship the changes"
- commit and push (in one step)
- finalize and publish the current diff
- any phrasing that combines commit + push intent

If the user asks for *only* a commit (no push) or *only* a push (no commit), do not use `/ship` — use the normal git workflow.

## How to invoke

Issue the slash command exactly:

```
/ship
```

If the user gave extra context for the commit message (e.g. "ship it, mention the perf fix"), pass it as an argument:

```
/ship mention the perf fix
```

`/ship` delegates to the `git-shipper` subagent, which handles convention detection, staging, message writing, commit, and push. Do not duplicate that work — let the command run.

## Requirements

This skill assumes the `ship` plugin from the `geberele/claude-plugins` marketplace is installed, which provides the `/ship` command and the `git-shipper` subagent. If `/ship` is not available in the current session, fall back to a normal git commit + push flow and tell the user the plugin isn't installed.
