# flatissues

File-based issue tracking for [Claude Code](https://claude.com/claude-code) projects. Issues are markdown files in `issues/`. Status changes are `git mv`. ~150 lines of Node, zero dependencies.

After install, Claude Code knows the convention. You ask in plain English; Claude files, renames, and indexes for you.

## Install

In any Claude Code session, paste:

> Install flatissues in this project, then read `.claude/rules/flatissues.md` so you can use it this session.

Claude runs `npx flatissues init` and loads the convention. From then on:

> file an issue for the dropdown bug

> this is going to be a big change — open an issue and split it into sprints

That's the interface.

Prefer to run it yourself? `npx flatissues init` in your project root.

## What gets installed

- `issues/` — scaffolding, an issue template, and an `update-index.js` that regenerates `INDEX.md` from filenames.
- `.claude/rules/flatissues.md` — auto-loaded by Claude Code on launch. Teaches the naming convention, when to suggest (not autonomously file) issues, and a multi-instance sprint workflow for tasks too large for one Claude session.

Status lives in the filename: `2026-05-02_OPEN_bug_dropdown.md` → `…_RESOLVED_…`. Every change is a normal commit.

## Configuration & CLI

See `issues/config.json` (statuses, categories, project name) and `npx flatissues help` for CLI flags, including `--inline` (append to `CLAUDE.md` instead of writing `.claude/rules/`).

## License

MIT.
