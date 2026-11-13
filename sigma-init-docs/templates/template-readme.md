# {{PROJECT_NAME}} - Work repo

Read this file before you add a file. The layout is closed.

## Lane

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-build-factory` → `/sigma-merge`

`/sigma-feedback` files a ticket or an epic and does not edit requirements or a spec. `/sigma-unslop-docs` rewrites one spec from the code. Those two lanes do not call each other.

`/sigma-unslop-tickets` also points at `/sg-create-ticket`. It is optional. It is not a required hop between `/sigma-to-tickets` and `/sigma-pick-tickets`.

Each command stops and names the next command. A next command is a fenced code block with only the command inside. Follow the next command in the same chat only when `/sigma-feedback` hands a factory ticket to the workers, and for a `/sigma-plan` assumption the user accepted. Feedback does not edit requirements or a spec. A yes to that assumption carries the named ids onto the spec and the tickets that already cite them. The question is asked in the chat. It is not a question card. Before the question, the reply says what this step does, then `We have:` and `We will write:` when a line changes. The question is the last line. An id does not stand in for the sentence. A confirmed feature with no SPEC ends at `/sigma-to-spec`.

Four entrances are parallel.

1. `/sigma-feedback` files each screen change as a ticket. It does not edit requirements or a spec. A factory ticket is handed to a worker. An epic is only a new area with no screen outcome, and it records `Next skill: /sigma-plan`.
2. The user types `/sigma-unslop-tickets` to repair open tickets. A yes writes through `/sg-create-ticket`. The reply names `/sigma-pick-tickets`.
3. The user types `/sigma-unslop`. That command writes one review and stops.
4. The user types `/sigma-unslop-docs`. That command rewrites one spec from the code and the passing tests. A closed ticket with no code is left out.

## How a build finishes

`/sigma-implement` matches one ticket. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`.

`/sigma-implement` opens or creates `.worktrees/loop_<YYYY-MM-DD>-<slug>/issue-<N>/` on branch `agent/<N>-<slug>`. It reuses a `git worktree list` row already on that branch under `.worktrees/`. A new branch comes from `origin/<default>`, not the user's current branch. It writes `.worktrees/loop_<YYYY-MM-DD>-<slug>/worktrees.json` only when it creates the worktree. A fix stays on that same branch and worktree.

1. The review screenshots the live page and compares it with the ticket. A script line that says pass is not evidence.
2. A user run of `/sigma-implement` is `hitl`. It prints What holds, What does not, Ticket intent, and Note. A pass prints `<promise>IMPLEMENTATION COMPLETE</promise>`, the pull request URL, and a fenced code block that contains only `/sigma-merge <pull-request-number>`, and does not ask. A miss prints the return block, then the last line is `Which lines should I fix?`, then it waits. The question is not a question card. `fix` sends every "What does not" sentence. A quoted "What does not" sentence sends that sentence only. `stop` stops and does not name merge.
3. A factory run builds each `type:ticket` from the sentences in its body on `factory/<YYYY-MM-DD>-<slug>`. A missing requirements line does not stop a ticket. A spec or an epic is too big and does not start. Each ticket branch starts from that factory branch and merges back after a pass. One pull request tracks the factory branch into main. The chat waits. The reply is one table. Columns are Title, Does, Outcome, and Status. Each cell is one sentence. Status is Met or Not met. A product sentence is filed as a ticket and handed to the workers. An epic is filed with `Next skill: /sigma-plan` and is not built in that turn. The words yes, accept, or happy print a fenced code block that contains only `/sigma-merge` and that pull request number. Main stays unchanged until that command runs. A title stops before the branch and the script only when it still uses a word the current line replaced. The stop names the word the title should use. The old git branch stays.
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
