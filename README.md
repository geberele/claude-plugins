# claude-plugins

Personal marketplace of [Claude Code](https://claude.com/claude-code) plugins by [@geberele](https://github.com/geberele).

---

## `/ship` — commit and push without burning your context

### The problem

Every "commit and push" turn in Claude Code looks the same: read `git status`, read the diff, scan recent commits to match the convention, draft a message, run `git commit`, run `git push`, handle the missing upstream. It works — but it dumps the full diff and twenty log entries into your main session every single time. Your context window fills up with shipping ceremony instead of the work you actually care about, and you pay for those tokens at your main model's rate.

### The fix

`/ship` is a one-line slash command that hands the whole flow to a dedicated subagent (`git-shipper`) running on **Haiku**. The subagent has its own context window, its own tool budget, and exactly one job. Your main session sees a short summary — commit hash, message, push result — and nothing else. The diff, the log, the convention-sniffing all happen elsewhere and get thrown away.

You get:

- **A lean main context.** No diffs, no `git log` dumps polluting the conversation you're actually having.
- **Lower cost.** The expensive part (reading the diff, drafting the message) runs on Haiku, not your main model.
- **A repeatable flow.** Same command, same behavior, every repo.
- **Repo-aware messages.** The subagent detects whether you use Conventional Commits (`feat:`, `fix:`, ...), ticket-prefixed messages (`PROJ-1234: ...`), or free-form, and matches the dominant style. It pulls ticket IDs from the branch name when the repo uses that format.

### What it will and won't do

Will:

- Stage your changes, write the commit message, commit, and push.
- Set the upstream (`-u origin <branch>`) on first push if missing.
- Stop and tell you if the diff is empty.

Won't:

- `git push --force`, even if asked.
- Skip hooks (`--no-verify`) or bypass signing.
- Touch your source code, run tests, or create branches.

## Install

```
/plugin marketplace add geberele/claude-plugins
/plugin install ship@geberele
```

## Usage

```
/ship                          # infer everything from the diff
/ship fixes flaky auth test    # add extra context for the message
```

## How it works

```
you: /ship
  └─ main session
       └─ delegates to subagent: git-shipper (Haiku, Bash only)
            ├─ git status / git diff      (read changes)
            ├─ git log --oneline -20      (detect convention)
            ├─ git branch --show-current  (extract ticket ID if applicable)
            ├─ git add / commit / push    (ship)
            └─ returns: hash + message + push result
  └─ main session sees only the summary
```

The subagent is defined in [`ship/agents/git-shipper.md`](./ship/agents/git-shipper.md) and the slash command in [`ship/commands/ship.md`](./ship/commands/ship.md). Both are short — read them before you install if you want to know exactly what runs against your repo.

## License

[MIT](./LICENSE) © Gabriele Manna
