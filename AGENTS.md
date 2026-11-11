# Edit this pack

Edit this pack when you change a skill folder in `sigma-build-factory`. The product is the skill folders. Each folder holds one `SKILL.md`.

Write the action, the stop, and the next command in the skill file. Do not add a path that exists only on one machine. The public README stays the install guide for every agent.

## Files you can add

Write `README.md`, `AGENTS.md`, and each skill's `SKILL.md` or templates. Do not add a doc outside those paths. Ask when a file fits none of them.

Skill folders: `sigma-init-docs`, `sigma-feedback`, `sigma-plan`, `sigma-to-spec`, `sigma-to-tickets`, `sg-create-ticket`, `sigma-unslop-tickets`, `sigma-pick-tickets`, `sigma-build-factory`, `sigma-implement`, `sg-tdd`, `sg-pr`, `sg-review`, `sigma-merge`, `sigma-unslop`. Ask before you add a skill folder. Commands the user types use the `sigma-` prefix. Skills another command starts use the `sg-` prefix.

## Lane

Each command stops and names the next command. A next command is a fenced code block with only the command inside. Do not call the next command from inside a skill. Four exceptions exist. `/sigma-feedback` follows exactly one of `/sigma-plan`, `/sigma-to-spec`, or `/sigma-to-tickets`. It does not follow `/sigma-unslop`. `/sigma-plan` follows `/sigma-to-spec` and then `/sigma-to-tickets` only for the ids in an assumption the user accepted. A confirmed feature with no SPEC ends at `/sigma-to-spec`. `/sigma-build-factory` starts one `/sigma-implement` worker per ticket in the wave. `/sigma-implement` starts one worker for `/sg-tdd` then `/sg-pr`, and a second worker for `/sg-review`. Those skills still stop at their own end.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-implement` → `/sigma-merge`

`/sigma-unslop-tickets` also points at `/sg-create-ticket`. It is optional. It is not a required hop between `/sigma-to-tickets` and `/sigma-pick-tickets`.

Three entrances are parallel.

1. `/sigma-feedback` follows exactly one of `/sigma-plan`, `/sigma-to-spec`, or `/sigma-to-tickets`. It does not follow `/sigma-unslop`.
2. The user types `/sigma-unslop-tickets` to repair open tickets. A yes writes through `/sg-create-ticket`. The reply names `/sigma-pick-tickets`.
3. The user types `/sigma-unslop`. That command writes one review and stops.

`/sigma-implement` matches one ticket. It opens or creates `.worktrees/loop_<YYYY-MM-DD>-<slug>/issue-<N>/` on branch `agent/<N>-<slug>` when the build starts. It reuses a `git worktree list` row already on that branch under `.worktrees/`. A new branch comes from `origin/<default>`, not the user's current branch. It writes `.worktrees/loop_<YYYY-MM-DD>-<slug>/worktrees.json` only when it creates the worktree. It creates that path only if `.worktrees/` is gitignored. A fix stays on that same branch and worktree. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`. That worker did not write the scripts. The review screenshots the live page. A script line that says pass is not evidence. A script outside the spec globs is a note. A user run is `hitl`. It prints What holds, What does not, Ticket intent, and Note. A pass prints `<promise>IMPLEMENTATION COMPLETE</promise>`, the pull request URL, and a fenced code block that contains only `/sigma-merge <pull-request-number>`, and does not ask. A miss prints the return block. The last line is `Which lines should I fix?`. The question is not a question card. It waits. `fix` sends every "What does not" sentence. A quoted "What does not" sentence sends that sentence only. `stop` stops and does not name merge. A factory run is `afk`. It does not ask and does not wait. A worker that would ask, or wait for `fix`, `stop`, `overwrite`, or "Which lines should I fix?", stops that ticket. The first miss goes back once. A second miss stops and does not name merge. When the wave returns, the factory prints a table with Title, Result, and Next, then each review as separate paragraphs. A pass then prints a fenced code block that contains only `/sigma-merge <pull-request-number>`. A stop prints one unblock line and does not name merge. The implement chat does not edit scripts and does not judge the diff. `/sigma-merge` stays the command you type.

1. The user's sentence replaces the old requirements line. No question about which line wins.
2. A closed ticket the user says was not delivered is reopened, not duplicated.
3. A second flag of a passed acceptance opens with: `/sg-review and /sigma-implement both passed a page the user still rejects.`
4. Topic pickup reads the Topic line, the parent spec, and the numbered requirements line. A factory stop names one unblock, and the other tickets in the wave still start.

Inside `/sigma-feedback`, new or changed behavior follows `/sigma-plan`, which writes `docs/requirements/REQUIREMENTS.md`. A question is asked in the chat. It is not a question card. Before the question, the reply says what this step does, then `We have:` and `We will write:` when a line changes. The question is the last line. When a spec or a ticket still has the old sentence, plan shows that sentence, then the sentence the user stated, then the spec title and the ticket title. A yes carries that sentence onto the spec and the tickets that already cite it. A line the user reads starts with the sentence. An issue number stays in the command the user will type. It does not stand in for the sentence. When nothing downstream differs, plan names `/sigma-to-spec`. A clash the user does not speak about stays an open question. `/sigma-to-spec` leaves that pair out and still offers the other sentences. A clear bug whose spec has no acceptance sentence follows `/sigma-to-spec`. A clear bug whose spec sentence already states the behavior follows `/sigma-to-tickets`. The ticket copies the sentence.

`/sigma-implement` flags a spec, an epic, or a requirements mismatch and names the command that fixes it. The word overwrite continues that build. A user run that does not say overwrite leaves the build waiting. When the ticket names no script, `/sigma-implement` chooses the closest file or one new path under `scripts/`. `/sigma-unslop` records a wrong file and does not edit it.

`/sg-create-ticket` is the only skill that runs `gh`. `/sigma-to-spec` and `/sigma-to-tickets` hand it a body. `/sigma-unslop-tickets` may close, edit, or link after one yes. That yes does not create a ticket. A clash names `/sigma-plan`. A split names `/sigma-to-tickets`. The reply ends with `/sigma-pick-tickets`. `/sigma-pick-tickets` reads the open list through `/sg-create-ticket` and does not write. Its reply is a numbered list of titles. The next command is a fenced code block with only the command inside. One ticket uses `/sigma-implement` and that number. More than one ticket uses `/sigma-build-factory` and every number. A title that uses a word the acceptance line and the requirements line do not use stays out of that list. `/sigma-build-factory` and `/sigma-implement` stop that ticket before the branch and the script. The old git branch stays. A browser write may ask `/sg-create-ticket` for one comment. That comment does not change acceptance lines. Review still runs. `/sigma-merge` still merges the pull request numbers you name. `/sigma-unslop` does not call `/sg-create-ticket`.

## Work repo write-scope

`sigma-init-docs` scaffolds the work repo. That repo holds requirements and API-call scripts. Skills in this pack write only the paths below.

| Path | Skill |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `sigma-unslop` |
| `scripts/**` named in the ticket | `sg-tdd` |
| GitHub issues | `sg-create-ticket` only |

A file that fits no row: ask. Do not invent a path.

## Install path

Document install as a symlink or copy of each skill folder into the skills directory that agent reads. The README lists Claude Code, Cursor, Codex, and `$HOME/.agents/skills`. Do not document install into `~/.cursor/skills-cursor/`.

## Publish

GitHub: https://github.com/sigma-michael/sigma-build-factory. The `sigma-michael` account is a separate `gh` config at `~/.config/gh-sigma-michael`. Push with `GH_CONFIG_DIR=~/.config/gh-sigma-michael` so the default `mskhmelik` login is not used.

## Done

A skill edit is done when its `SKILL.md` names the action, the stop, and the next command, and `sigma-implement` still has no red-then-green steps and no review steps.
