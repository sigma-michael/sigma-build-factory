# Edit this pack

Edit this pack when you change a skill folder in `sigma-build-factory`. The product is the skill folders. Each folder holds one `SKILL.md`.

Write the action, the stop, and the next command in the skill file. Do not add a path that exists only on one machine. The public README stays the install guide for every agent.

## Files you can add

Write `README.md`, `AGENTS.md`, and each skill's `SKILL.md` or templates. Do not add a doc outside those paths. Ask when a file fits none of them.

Skill folders: `sigma-init-docs`, `sigma-feedback`, `sigma-plan`, `sigma-to-spec`, `sigma-to-tickets`, `sg-create-ticket`, `sigma-unslop-tickets`, `sigma-unslop-docs`, `sigma-retro`, `sigma-pick-tickets`, `sigma-build-factory`, `sigma-implement`, `sg-tdd`, `sg-pr`, `sg-review`, `sigma-merge`, `sigma-unslop`, `sg-workbooks`, `sg-data-models`, `sg-api`. Ask before you add a skill folder. Commands the user types use the `sigma-` prefix. Skills another command starts use the `sg-` prefix.

A workbook contents script follows `sg-workbooks`. A data-model script follows `sg-data-models`. When a token is required, follow `sg-api` first. Do not invent the request shape.

## Lane

Each command stops and names the next command. The step lives in the skill. This file is the map.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-build-factory` → `/sigma-merge`

`/sigma-feedback` files a note outside a build through `/sg-create-ticket`. In a factory or implement chat, a sentence about a ticket this run built returns to that worker. A sentence outside this run is a warning. `/sigma-unslop-docs` rewrites one spec from the code. Those lanes do not call each other. `/sigma-retro` is optional. Merge does not wait for it.

1. `/sigma-feedback` files a note outside a build, returns a sentence about a built ticket to that worker, or warns when the sentence is outside this run.
2. `/sigma-unslop-tickets` repairs open tickets. Next is `/sigma-pick-tickets`.
3. `/sigma-unslop` writes one review and stops.
4. `/sigma-unslop-docs` rewrites one spec from the code.
5. `/sigma-retro` files one skill-pack issue after you pick a row.

`/sigma-implement` builds one ticket on `agent/<N>-<slug>`. A standalone pass fences `/sigma-merge`. A factory pass does not. `/sg-create-ticket` is the only skill that runs `gh`. A closed ticket the user says was not delivered is reopened, not duplicated.

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
