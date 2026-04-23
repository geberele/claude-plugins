# claude-plugins

Personal marketplace of [Claude Code](https://claude.com/claude-code) plugins by [@geberele](https://github.com/geberele).

## Plugins

### `/ship`

Stage, commit, and push your current changes with a commit message that **matches your repo's convention** — delegated to a fast Haiku subagent so it's cheap and snappy.

- Detects Conventional Commits (`feat:`, `fix:`, ...), ticket-prefixed messages (`PROJ-1234: ...`), or free-form style from recent history.
- Extracts ticket IDs from the branch name when the repo uses that format.
- Pushes, setting upstream if missing.
- Will **not** push with `--force`, skip hooks, or modify your source code.

## Install

```
/plugin marketplace add geberele/claude-plugins
/plugin install ship@geberele
```

## Usage

```
/ship                       # infer everything from the diff
/ship fixes flaky auth test # add extra context for the commit message
```

## License

[MIT](./LICENSE) © Gabriele Manna
