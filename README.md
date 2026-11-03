# sigma-build-factory

Cursor agent skills for a simplified Sigma Computing build flow.

Skill folders, each with its own `SKILL.md`: `sigma-init-docs`, `sigma-plan`, `sigma-to-spec`, `sigma-to-tickets`, `sigma-create-ticket`, `sigma-implement`, `sigma-review`, `sigma-pr`, `sigma-merge`, `sigma-unslop`. The `sigma-` prefix keeps these names from colliding with factory skills in `~/.cursor/skills`.

`sigma-init-docs` scaffolds a separate work repo. That repo holds requirements and API-call scripts.

GitHub: https://github.com/sigma-michael/sigma-build-factory

## Lane

Each command stops and names the next command. None of them invoke the next.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sigma-implement` → `/sigma-review` → `/sigma-pr` → `/sigma-merge`

`/sigma-implement` builds one ticket on branch `agent/{N}-{slug}`, in its own chat. It is builder-only. `/sigma-review` runs in a different chat from the builder.

`/sigma-unslop` is off the lane. It files tickets through `/sigma-create-ticket` and stops.

`/sigma-create-ticket` is the only command that creates or edits a GitHub issue. `/sigma-to-spec`, `/sigma-to-tickets`, and `/sigma-unslop` hand it a composed body.

## Work repo write-scope

| Path | Skill |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `sigma-unslop` |
| `scripts/**` named in the ticket | `sigma-implement` |
| GitHub issues | `sigma-create-ticket` only |

A file that fits no row: the agent asks. It does not invent a path.

## Install

Symlink or copy each skill folder into `~/.cursor/skills/`. Cursor loads user skills from that directory. Leave `~/.cursor/skills-cursor/` unchanged. A copy there does not install this pack.
