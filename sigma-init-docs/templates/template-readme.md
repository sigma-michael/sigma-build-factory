# {{PROJECT_NAME}} - Work repo

Read this file before you add a file. The layout is closed.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-unslop-tickets` → `/sigma-pick-tickets` → `/sigma-implement` (one ticket, branch `agent/{N}-{slug}`; one worker follows `/sg-tdd` then `/sg-pr`, a second worker follows `/sg-review`) → `/sigma-merge`

Each command stops and names the next command. Follow the next command in the same chat only for `/sigma-feedback` and for a `/sigma-plan` assumption the user accepted. A yes to that assumption carries the named ids onto the spec and the tickets that already cite them. A confirmed feature with no SPEC ends at `/sigma-to-spec`.

A pasted note starts at `/sigma-feedback`. It follows one skill in the same chat.

1. New behavior follows `/sigma-plan`.
2. A clear bug with no spec line follows `/sigma-to-spec`.
3. A clear bug whose spec line already states the behavior follows `/sigma-to-tickets`.
4. Drifted scripts follow `/sigma-unslop`. It writes `docs/reviews/<date>-<topic>.md` and stops.

## How a build finishes

`/sigma-implement` matches one ticket. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`.

1. The review screenshots the live page and compares it with the ticket. A script line that says pass is not evidence.
2. A met intent names `/sigma-merge` with the pull request number from `/sg-pr`.
3. The first miss sends those ids back on the same branch. The second miss stops and prints what the picture shows and what the ticket asks for.

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
