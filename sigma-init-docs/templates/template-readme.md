# {{PROJECT_NAME}} - Work repo

Read this file before you add a file. The layout is closed.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-implement` → `/sigma-merge`

`/sigma-unslop-tickets` also points at `/sg-create-ticket`. It is optional. It is not a required hop between `/sigma-to-tickets` and `/sigma-pick-tickets`.

Each command stops and names the next command. Follow the next command in the same chat only for `/sigma-feedback` and for a `/sigma-plan` assumption the user accepted. A yes to that assumption carries the named ids onto the spec and the tickets that already cite them. The question is asked in the chat. It is not a question card. Before the question, the reply says what this step does, then `We have:` and `We will write:` when a line changes. The question is the last line. An id does not stand in for the sentence. A confirmed feature with no SPEC ends at `/sigma-to-spec`.

Three entrances are parallel.

1. `/sigma-feedback` follows exactly one of `/sigma-plan`, `/sigma-to-spec`, or `/sigma-to-tickets`. It does not follow `/sigma-unslop`.
2. The user types `/sigma-unslop-tickets` to repair open tickets. A yes writes through `/sg-create-ticket`. The reply names `/sigma-pick-tickets`.
3. The user types `/sigma-unslop`. That command writes one review and stops.

## How a build finishes

`/sigma-implement` matches one ticket. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`.

`/sigma-implement` opens or creates `.worktrees/loop_<YYYY-MM-DD>-<slug>/issue-<N>/` on branch `agent/<N>-<slug>`. It reuses a `git worktree list` row already on that branch under `.worktrees/`. A new branch comes from `origin/<default>`, not the user's current branch. It writes `.worktrees/loop_<YYYY-MM-DD>-<slug>/worktrees.json` only when it creates the worktree. A fix stays on that same branch and worktree.

1. The review screenshots the live page and compares it with the ticket. A script line that says pass is not evidence.
2. A user run of `/sigma-implement` is `hitl`. It prints What holds, What does not, Ticket intent, and Note. A pass prints `<promise>IMPLEMENTATION COMPLETE</promise>`, the pull request URL, and `Next: /sigma-merge <pull-request-number>`, and does not ask. A miss prints the return block, then the last line is `Which lines should I fix?`, then it waits. The question is not a question card. `fix` sends every "What does not" sentence. A quoted "What does not" sentence sends that sentence only. `stop` stops and does not name merge.
3. A factory run is `afk`. It does not ask and does not wait. The first miss goes back once. A second miss stops and does not name merge. When the wave returns, the factory prints Title, What holds, What does not, Ticket intent, and Worktree. A pass then prints `Next: /sigma-merge <pull-request-number>`. A stop prints one unblock line.
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
