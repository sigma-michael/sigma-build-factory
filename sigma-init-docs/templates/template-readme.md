# {{PROJECT_NAME}} - Work repo

Read this file before you add a file. The layout is closed.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-build-factory` → `/sigma-merge`

`/sigma-feedback` files a ticket or an epic and does not edit requirements or a spec. `/sigma-unslop-docs` rewrites one spec from the code. Those two lanes do not call each other. `/sigma-retro` is optional. Merge does not wait for it.

`/sigma-unslop-tickets` also points at `/sg-create-ticket`. It is optional. It is not a required hop between `/sigma-to-tickets` and `/sigma-pick-tickets`.

Each command stops and names the next command. The step lives in the skill. A factory or implement chat returns a feedback note to that worker. A standalone note fences `/sigma-plan` or `/sigma-pick-tickets`.

Four entrances are parallel.

1. `/sigma-feedback` files the note and returns it to an open worker, or fences `/sigma-plan` or `/sigma-pick-tickets`.
2. The user types `/sigma-unslop-tickets` to repair open tickets. A yes writes through `/sg-create-ticket`. The reply names `/sigma-pick-tickets`.
3. The user types `/sigma-unslop`. That command writes one review and stops.
4. The user types `/sigma-unslop-docs`. That command rewrites one spec from the code and the passing tests. A closed ticket with no code is left out.

## How a build finishes

`/sigma-implement` matches one ticket. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`.

`/sigma-implement` opens or creates `.worktrees/loop_<YYYY-MM-DD>-<slug>/issue-<N>/` on branch `agent/<N>-<slug>`. It reuses a `git worktree list` row already on that branch under `.worktrees/`. A new branch comes from `origin/<default>`, not the user's current branch. It writes `.worktrees/loop_<YYYY-MM-DD>-<slug>/worktrees.json` only when it creates the worktree. A fix stays on that same branch and worktree.

1. The review screenshots the live page. A script pass line is not evidence.
2. An `agent/` pass does not print `/sigma-merge`. That branch merges into a factory branch.
3. A factory pass merges into the factory branch. A second miss leaves that branch unchanged. Yes, accept, or happy prints `/sigma-merge`.
4. The user's sentence replaces the old requirements line. No question about which line wins. A line the user reads starts with that sentence, then the sentence we will write. An issue number stays in the command the user will type.
5. A closed ticket the user says was not delivered is reopened, not duplicated.
6. A second flag of a passed acceptance opens with: `/sg-review and /sigma-implement both passed a page the user still rejects.`
7. Topic pickup reads the Topic line, the parent spec, and the numbered requirements line. The other tickets in the wave still start.

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
| GitHub issues | `/sg-create-ticket` only |

If nothing fits, ask. Do not invent a path. Do not create a loose file at `docs/` root, a new top-level doc folder, or a `-vN` filename.

The only file at `docs/` root is `docs/README.md`.

## Scaffold

This command writes `AGENTS.md`, `docs/README.md`, `docs/requirements/REQUIREMENTS.md` (headings only, until `/sigma-plan`), and `scripts/.gitkeep`. It writes `.gitignore` (`.DS_Store` and `.worktrees/`) only when that file is missing. It does not create `docs/reviews/`.
