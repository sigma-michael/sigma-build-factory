# {{PROJECT_NAME}} - Work repo

Read this file before you add a file. The layout is closed.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-triage` → `/sigma-implement` (one ticket, branch `agent/{N}-{slug}`; one worker follows `/sg-tdd` then `/sg-pr`, a second worker follows `/sg-review`) → `/sigma-merge`

Each command stops and names the next command. None of them invoke the next, except `/sigma-feedback`. A pasted note starts there. It follows one skill in the same chat.

`/sigma-unslop` is off the lane. It writes `docs/reviews/<date>-<topic>.md` and stops. A pasted note starts at `/sigma-feedback`. New behavior follows `/sigma-plan`. A clear bug with no spec line follows `/sigma-to-spec`. A clear bug whose spec line already states the behavior follows `/sigma-to-tickets`. Drifted scripts follow `/sigma-unslop`.

## Code

`scripts/` is the only code home. Each file is an API call named in the ticket. `/sg-tdd` commits that file. `/sigma-implement` starts that skill.

An application tree or a second code folder has no row in the write-scope table. Ask.

## Write scope

| Path | Owner |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `/sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `/sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `/sigma-unslop` (not created up front) |
| `scripts/**` named in the ticket | `/sg-tdd` |
| GitHub issues | `/sigma-create-ticket` only |

If nothing fits, ask. Do not invent a path. Do not create a loose file at `docs/` root, a new top-level doc folder, or a `-vN` filename.

The only file at `docs/` root is `docs/README.md`.

## Scaffold

This command writes `AGENTS.md`, `docs/README.md`, `docs/requirements/REQUIREMENTS.md` (headings only, until `/sigma-plan`), and `scripts/.gitkeep`. It writes `.gitignore` (`.DS_Store` and `.worktrees/`) only when that file is missing. It does not create `docs/reviews/`.
