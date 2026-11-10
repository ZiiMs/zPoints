# Issue tracker: Beads (`bd`)

Issues and specs for this repo live in the local Beads database under `.beads/` (Dolt).
Use the `bd` CLI for every operation. Do not mirror tickets into markdown files, GitHub
issues or TODO lists; `.beads/issues.jsonl` is a passive export, not the source of truth.

IDs look like `zp-a3f2dd` (prefix `zp`).

## Conventions

| Operation | Command |
| --- | --- |
| Create | `bd create "Title" -t task -p 2 -d "Body" --acceptance "Done when…" -l ready-for-agent` |
| Read | `bd show <id> --long` (`--include-comments` for the thread) |
| List | `bd list --status open --json` with `--label`, `--type`, `--priority`, `--parent` |
| Ready work | `bd ready` (open, no active blockers) |
| Comment | `bd comment <id> "text"` |
| Notes | `bd note <id> "text"` |
| Labels | `bd label add <id> needs-triage` / `bd label remove <id> needs-triage` |
| Status | `bd update <id> --status in_progress`; `bd close <id> --reason "…"` |
| Claim | `bd update <id> --claim` (assignee = you, status = in_progress) |
| Child | `bd create "…" --parent <id>`; `bd children <id>` |
| Blocking | `bd dep add <blocked> <blocker>` (or `bd dep <blocker> --blocks <blocked>`) |
| Dependencies | `bd dep list <id>`, `bd dep tree <id>`, `bd blocked` |

Statuses: `open`, `in_progress`, `blocked`, `deferred`, `closed`, `pinned`, `hooked`.
Types: `task`, `bug`, `feature`, `epic`, `decision`, `spike`, `story`, `chore`, `milestone`.

`bd close` refuses while children are open or a blocker is live; use `--force` only when
that is genuinely intended.

## Pull requests as a triage surface

**PRs as a request surface: no.** Work lands as commits on `master` at
`github.com/ZiiMs/zPoints`; issues live only in Beads. Flip this to `yes` if external PRs
should enter the triage queue.

## When a skill says "publish to the issue tracker"

Create a Beads issue with `bd create` and report its ID.

## When a skill says "fetch the relevant ticket"

Run `bd show <id>` and state the title before starting work.

## Wayfinding operations

The **map** is a single Beads issue labelled `wayfinder:map`; its **child** issues are tickets.

- **Map**: `bd create "Map: <destination>" -t epic -l wayfinder:map --body-file map.md` (Notes / Decisions-so-far / Fog).
- **Child ticket**: `bd create "<question>" --parent <map-id> -l wayfinder:<research|prototype|grilling|task>`.
- **Blocking**: `bd dep add <child> <blocker>`; `bd` recomputes readiness. Unblocked when every blocker is closed.
- **Frontier query**: `bd ready --parent <map-id>`; drop assigned tickets, first in map order wins.
- **Claim**: `bd update <id> --claim`, the session's first write.
- **Resolve**: `bd comment <id> "<answer>"`, `bd close <id> --reason "<one line>"`, then `bd note <map-id> "- <decision> → <ticket id>"`.
