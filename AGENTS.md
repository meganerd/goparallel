# AGENTS.md — Beads (bd) Task Tracking

This project uses **beads** (`bd`) for dependency-aware issue tracking. Issues are chained together like beads — each can block or depend on others.

## Database

- **Location:** `.beads/goparallel.db`
- **Issue prefix:** `goparallel` (issues named `goparallel-1`, `goparallel-2`, ...)

## Quick Reference

| Command | Purpose |
|---------|---------|
| `bd list` | List all issues |
| `bd ready` | Show unblocked work ready to claim |
| `bd blocked` | Show blocked issues |
| `bd show <id>` | Show issue details |
| `bd create "title" -t task\|feature\|epic -p 0-4 -d "desc"` | Create an issue |
| `bd dep add <id> <blocker-id>` | Add dependency (blocker must complete first) |
| `bd dep tree <id>` | Visualize dependency tree |
| `bd update <id> --status in_progress` | Update status |
| `bd close <id>` | Mark done |

## Priority Levels

- **P0** — Critical / blocking
- **P1** — High priority
- **P2** — Normal
- **P3** — Low priority
- **P4** — Backlog

## Agent Workflow

1. Run `bd ready` to find unblocked work
2. Claim a task: `bd update <id> --status in_progress --assignee <name>`
3. Do the work
4. Close: `bd close <id> --reason "description of what was done"`
5. Check `bd ready` again for newly unblocked work

## Git Sync

Beads auto-syncs with git:
- CRUD operations export to JSONL (5s debounce)
- JSONL imported when newer than DB (e.g., after `git pull`)
- No manual export/import needed across machines

## Dependency Types

| Type | Meaning |
|------|---------|
| `blocks` | Task B must complete before task A can start |
| `related` | Soft connection, doesn't block progress |
| `parent-child` | Epic/subtask hierarchy |
| `discovered-from` | Auto-created when discovering related work |
