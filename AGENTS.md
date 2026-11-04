# Edit this pack

Edit this pack when you change a skill folder in `sigma-build-factory`. The product is the skill folders. Each folder holds one `SKILL.md`.

Write the action, the stop, and the next command in the skill file. Do not add a path that exists only on one machine. The public README stays the install guide for every agent.

## Files you can add

Write `README.md`, `AGENTS.md`, and each skill's `SKILL.md` or templates. Do not add a doc outside those paths. Ask when a file fits none of them.

Skill folders: `sigma-init-docs`, `sigma-feedback`, `sigma-plan`, `sigma-to-spec`, `sigma-to-tickets`, `sigma-create-ticket`, `sigma-implement`, `sg-tdd`, `sg-pr`, `sg-review`, `sigma-merge`, `sigma-unslop`. Ask before you add a skill folder. Commands the user types use the `sigma-` prefix. Skills only `/sigma-implement` starts use the `sg-` prefix.

## Lane

Each command stops and names the next command. Do not call the next command from inside a skill. Two exceptions exist. `/sigma-feedback` follows exactly one of `/sigma-plan`, `/sigma-to-spec`, `/sigma-to-tickets`, or `/sigma-unslop`. `/sigma-implement` starts one worker that follows `/sg-tdd`, then `/sg-pr`, and a second worker that follows `/sg-review`. Those skills still stop at their own end.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-implement` → `/sigma-merge`

`/sigma-implement` matches one ticket on branch `agent/{N}-{slug}` and opens the worktree. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`. That worker did not write the scripts. The implement chat does not edit scripts and does not judge the diff. `/sigma-merge` stays the command you type.

`/sigma-unslop` is off the lane. It writes a review and stops. It does not file. A pasted note starts at `/sigma-feedback`. New or changed behavior follows `/sigma-plan`, which writes `docs/requirements/REQUIREMENTS.md` and names `/sigma-to-spec`. A clear bug whose spec has no acceptance line follows `/sigma-to-spec`. A clear bug whose spec line already states the behavior follows `/sigma-to-tickets` with that spec number. The ticket copies the line. Drifted scripts follow `/sigma-unslop`.

`/sigma-implement` flags a spec, an epic, or a requirements mismatch and names the command that fixes it. The word overwrite continues that build. Silence does not build. An empty script list still stops.

`/sigma-create-ticket` is the only skill that creates or edits a GitHub issue. `/sigma-to-spec` and `/sigma-to-tickets` hand it a composed body. A browser write asks it for one comment, and `/sigma-implement` prints the warning. The comment does not change acceptance lines. Review still runs, and `/sigma-merge` still merges the numbers you name. `/sigma-unslop` does not.

## Work repo write-scope

`sigma-init-docs` scaffolds the work repo. That repo holds requirements and API-call scripts. Skills in this pack write only the paths below.

| Path | Skill |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `sigma-unslop` |
| `scripts/**` named in the ticket | `sg-tdd` |
| GitHub issues | `sigma-create-ticket` only |

A file that fits no row: ask. Do not invent a path.

## Install path

Document install as a symlink or copy of each skill folder into the skills directory that agent reads. The README lists Claude Code, Cursor, Codex, and `$HOME/.agents/skills`. Do not document install into `~/.cursor/skills-cursor/`.

## Publish

GitHub: https://github.com/sigma-michael/sigma-build-factory. The `sigma-michael` account is a separate `gh` config at `~/.config/gh-sigma-michael`. Push with `GH_CONFIG_DIR=~/.config/gh-sigma-michael` so the default `mskhmelik` login is not used.

## Done

A skill edit is done when its `SKILL.md` names the action, the stop, and the next command, and `sigma-implement` still has no red-then-green steps and no review steps.
