# {{PROJECT_NAME}} - agent notes

The lane, the code home, and the write-scope live in [`docs/README.md`](docs/README.md). Requirements live in [`docs/requirements/REQUIREMENTS.md`](docs/requirements/REQUIREMENTS.md). `/sigma-plan` fills that file. Read it before you add a script.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-implement` (one ticket, branch `agent/{N}-{slug}`, its own chat; commits scripts; does not review) → `/sigma-review` (a different chat) → `/sigma-pr` → `/sigma-merge`

`/sigma-unslop` is off the lane.

## Write scope

| Path | Owner |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `/sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `/sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `/sigma-unslop` (not created up front) |
| `scripts/**` named in the ticket | `/sigma-implement` |
| GitHub issues | `/sigma-create-ticket` only |

If nothing fits, ask. Do not invent a path. Do not create a loose file at `docs/` root, a new top-level doc folder, or a `-vN` filename.

## Code

`scripts/` is the only code home. Each file is an API call named in the ticket.
