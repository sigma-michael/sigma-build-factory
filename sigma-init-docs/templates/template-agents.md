# {{PROJECT_NAME}} - agent notes

The lane, the code home, and the write-scope live in [`docs/README.md`](docs/README.md). Requirements live in [`docs/requirements/REQUIREMENTS.md`](docs/requirements/REQUIREMENTS.md). `/sigma-plan` fills that file. Read it before you add a script.

A question is asked in the chat and is not a question card: before the question, the reply says what this step does, then `We have:` and `We will write:` when a line changes, the question is the last line, and an id does not stand in for the sentence.

## Lane

A next command is a fenced code block with only the command inside.

`/sigma-init-docs` → `/sigma-plan` → `/sigma-to-spec` → `/sigma-to-tickets` → `/sg-create-ticket` → GitHub → `/sigma-pick-tickets` → `/sigma-implement` → `/sigma-merge`

`/sigma-unslop-tickets` also points at `/sg-create-ticket`. It is optional. It is not a required hop between `/sigma-to-tickets` and `/sigma-pick-tickets`.

Four entrances are parallel. Feedback and document repair do not call each other.

1. `/sigma-feedback` files the note as a ticket or an epic. It does not edit requirements or a spec. A factory ticket is handed to the workers. An epic records `Next skill: /sigma-plan`.
2. The user types `/sigma-unslop-tickets` to repair open tickets. A yes writes through `/sg-create-ticket`. The reply names `/sigma-pick-tickets`.
3. The user types `/sigma-unslop`. That command writes one review and stops.
4. The user types `/sigma-unslop-docs`. That command rewrites one spec from the code and the passing tests. A closed ticket with no code is left out.

## Write scope

| Path | Owner |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `/sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `/sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `/sigma-unslop` (not created up front) |
| `scripts/**` named in the ticket | `/sg-tdd` |
| GitHub issues | `/sg-create-ticket` only |

If nothing fits, ask. Do not invent a path. Do not create a loose file at `docs/` root, a new top-level doc folder, or a `-vN` filename.

## Code

`scripts/` is the only code home. Each file is an API call named in the ticket.
