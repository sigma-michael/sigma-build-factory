# Edit this pack

Edit this pack when you change a skill folder in `sigma-build-factory`. The product is the skill folders. Each folder holds one `SKILL.md`.

Before you write a sentence, read:

- `/Users/michael.khmelik/.cursor/skills/writing-for-agents/SKILL.md` for document shape
- `/Users/michael.khmelik/.cursor/skills/unslop-language/SKILL.md` for sentence voice

## Files you can add

Write `README.md`, `AGENTS.md`, and each skill's `SKILL.md` or templates. Do not add a doc outside those paths. Ask when a file fits none of them.

Skill folders: `sigma-init-docs`, `sigma-plan`, `sigma-to-spec`, `sigma-to-tickets`, `sigma-create-ticket`, `sigma-implement`, `sigma-review`, `sigma-pr`, `sigma-merge`, `sigma-unslop`. Ask before you add a skill folder. Keep the `sigma-` prefix so these names do not collide with factory skills in `~/.cursor/skills`.

## Lane

Each command stops and names the next command. Do not call the next command from inside a skill.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-implement` → `/sigma-review` → `/sigma-pr` → `/sigma-merge`

`/sigma-implement` builds one ticket on branch `agent/{N}-{slug}`, in its own chat. It is builder-only. Keep review in `sigma-review`, in a different chat from the builder. Do not add review inside `sigma-implement`.

`/sigma-unslop` is off the lane. It files tickets through `/sigma-create-ticket` and stops.

`/sigma-create-ticket` is the only skill that creates or edits a GitHub issue. `/sigma-to-spec`, `/sigma-to-tickets`, and `/sigma-unslop` hand it a composed body.

## Work repo write-scope

`sigma-init-docs` scaffolds the work repo. That repo holds requirements and API-call scripts. Skills in this pack write only the paths below.

| Path | Skill |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `sigma-unslop` |
| `scripts/**` named in the ticket | `sigma-implement` |
| GitHub issues | `sigma-create-ticket` only |

A file that fits no row: ask. Do not invent a path.

## Install path

Document install as a symlink or copy of each skill folder into `~/.cursor/skills/`. Do not document install into `~/.cursor/skills-cursor/`.

## Publish

GitHub: https://github.com/sigma-michael/sigma-build-factory. The `sigma-michael` account is a separate `gh` config at `~/.config/gh-sigma-michael`. Push with `GH_CONFIG_DIR=~/.config/gh-sigma-michael` so the default `mskhmelik` login is not used.

## Done

A skill edit is done when its `SKILL.md` names the action, the stop, and the next command, and `sigma-implement` still has no review steps.
