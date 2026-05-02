# flatissues

A file-based issue tracker for [Claude Code](https://claude.com/claude-code) projects — and a convention that changes how Claude collaborates with you on engineering work.

## Why use it

Conversation context is volatile. Bugs found during a long session, decisions made during research, follow-ups noted in passing — none of it sticks around unless something writes it down. flatissues writes it down: one file per issue in your repo, status in the filename, history in the body, all tracked by git. New Claude sessions can read `INDEX.md` and pick up where the last one left off.

What you get on top of a normal tracker:

**Claude proactively suggests filing issues** at moments most setups would drop them: when a bug surfaces mid-task, when scope creeps, after a research synthesis, before a refactor begins. It asks before creating — never autonomously files.

**Multi-session work has a real handoff.** Tasks too large for one Claude instance can be planned as sprints from the start. Each sprint runs in a fresh Claude session with a clean context; the previous sprint writes a one-line prompt you paste to start the next one. No "where were we?" rediscovery.

**You don't manage the tracker — Claude does.** Filenames, status renames, index regeneration, archiving. You stay in plain English: *"file an issue for the dropdown bug"*, *"mark the auth refactor resolved"*, *"what's still open?"*. Claude handles the file ops correctly because the rule file teaches them.

## Install

Ask Claude to install flatissues. Then start a fresh conversation — Claude Code auto-loads the convention and plain-language commands work directly.

Prefer to run it yourself? `npx flatissues init` in your project root.

## How it works

Issues are markdown files in `issues/`, named `YYYY-MM-DD_STATUS_category_short-slug.md`. Status changes are `git mv` (the filename is the source of truth, every change is a normal commit). An auto-generated `INDEX.md` lists everything by status. The tracker is ~470 lines of Node, zero dependencies.

The Claude-side protocol lives in `.claude/rules/flatissues.md`, which Claude Code auto-loads on launch. It teaches the naming convention, the suggest-don't-autonomously-file rule, and the sprint workflow.

## Configuration & CLI

See `issues/config.json` (statuses, categories, project name) and `npx flatissues help` for CLI flags, including `--inline` (append to `CLAUDE.md` instead of writing `.claude/rules/`).

## License

MIT.
