# flatissues

[![npm version](https://img.shields.io/npm/v/flatissues.svg?color=blue)](https://www.npmjs.com/package/flatissues)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A self-organizing record system for [Claude Code](https://claude.com/claude-code) projects. Bugs, decisions, research notes, follow-ups — anything you'd otherwise lose at the end of a session.

## Why use it

flatissues is two things at once:

- **A folder of markdown files**, one per record, status in the filename, indexed automatically, tracked by git. Plain files you can read, grep, and commit like any other.
- **A convention Claude already knows.** Install once and every Claude session in the project can read, write, rename, and reason about those files from plain English. Nothing to memorize, no commands to learn, no MCP server to run.

The combination is the point. On its own, a folder of markdown is just a folder. With Claude reading the convention on every launch, it becomes a record system you talk to: *"file an issue for the dropdown bug"*, *"what's still open?"*, *"mark the auth refactor resolved"*. Claude knows when to suggest filing one, too — after a bug surfaces mid-task, after a research synthesis, before a refactor. It always asks first.

For work too large for one session, issues can be split into sprints up front. Each sprint runs in a fresh Claude session and hands off via a one-line prompt. No "where were we?".

![Asking Claude to list open issues in a flatissues-tracked project](docs/list-issues-example.png)

*A real Claude Code session reading the issues folder directly. No commands, no API.*

## Install

Ask Claude to install flatissues. Then start a fresh conversation — Claude Code auto-loads the convention and plain-language commands work directly.

Prefer to run it yourself? `npx flatissues init` in your project root.

## Things to ask Claude

After install, these work in plain English:

| What you want | Just say |
|---|---|
| File a new issue | *"file an issue for the dropdown bug"* |
| File a big task with sprints | *"this is going to be a big change — open an issue and split it into sprints"* |
| Update status | *"mark the auth refactor resolved"* |
| Look something up | *"what's still open?"* or *"show me the bug-category issues"* |
| Add notes to an existing issue | *"add to the dropdown bug — turns out the cause is X"* |
| Continue a multi-session task | *"start sprint 2 of the auth refactor"* |

Phrase any of these however feels natural — Claude reads the rule file and works from intent, not exact wording.

## How it works

Issues are markdown files in `issues/`, named `YYYY-MM-DD_STATUS_category_short-slug.md`. Status changes are `git mv` (the filename is the source of truth, every change is a normal commit). An auto-generated `INDEX.md` lists everything by status. The tracker is ~470 lines of Node, zero dependencies.

The Claude-side protocol lives in `.claude/rules/flatissues.md`, which Claude Code auto-loads on launch. It teaches the naming convention, the suggest-don't-autonomously-file rule, and the sprint workflow.

<details>
<summary><b>What an issue file looks like</b></summary>

Every issue starts as a copy of this template, then gets filled in as the work progresses. The Sprint Execution Plan section is optional — only used when the work is too large for a single Claude session.

````markdown
# Issue Title Here

## Metadata
- **Date**: YYYY-MM-DD
- **Status**: OPEN
- **Category**: feature | bug | performance | infra | docs | refactor | research

## Problem
What's happening, what should be happening, and why this matters.

## Investigation Notes
- What was checked
- What was discovered
- Relevant file/line references

## Proposed Solution
What to do, and roughly how big the change is.

## Sprint Execution Plan
*Delete this section if the work is small enough for a single Claude instance.
Otherwise, fill it in before starting work — see issues/README.md § Sprint Workflow.*

### Sprint 1 — [name]
- **Scope:** [layers/phases]
- **Key files:** [paths to read first]
- **Deliverables:** [what should exist when this sprint is done]
- **Kickoff prompt:**
  ```
  Read issues/<this-file> — implement Sprint 1. Follow the sprint process
  in issues/README.md. Start by reading the files listed under Sprint 1.
  ```

### Sprint 2 — [name]
- **Scope:** ...
- **Key files:** ...
- **Deliverables:** ...
- **Kickoff prompt:** *(written by Sprint 1's instance during handoff)*

## Resolution
*Filled in once resolved.*

## Status History
- YYYY-MM-DD — OPEN, initial report
````

</details>

## Configuration & CLI

See `issues/config.json` (statuses, categories, project name) and `npx flatissues help` for CLI flags, including `--inline` (append to `CLAUDE.md` instead of writing `.claude/rules/`).

## License

MIT.
