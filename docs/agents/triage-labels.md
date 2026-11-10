# Triage Labels

The skills speak in terms of five canonical triage roles. In this repo they are Beads
labels, applied with `bd label add <id> <label>` and removed with `bd label remove <id> <label>`.

| Role in mattpocock/skills | Label string in our tracker | Meaning                                  |
| ------------------------- | --------------------------- | ---------------------------------------- |
| `needs-triage`            | `needs-triage`              | Maintainer needs to evaluate this issue  |
| `needs-info`              | `needs-info`                | Waiting on reporter for more information |
| `ready-for-agent`         | `ready-for-agent`           | Fully specified, ready for an AFK agent  |
| `ready-for-human`         | `ready-for-human`           | Requires human implementation            |
| `wontfix`                 | `wontfix`                   | Will not be actioned                     |

Beads has no separate label registry: a label exists once some issue carries it, so the
first `bd label add` creates it. `bd label list-all` shows the vocabulary actually in use.

When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the corresponding
label string from this table.
