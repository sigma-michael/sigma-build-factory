# sigma-build-factory

Agent skills for building Sigma work from a requirements document through to a merged pull request. Claude Code, Cursor, Codex, and any other agent that reads a `SKILL.md` folder can run them.

You keep the product decisions in one requirements file. The agent writes API scripts only where a ticket says it may. Each command finishes, tells you the next command, and waits. Review runs in a different chat from the chat that wrote the scripts.

The `sigma-` prefix keeps these commands distinct from other skill packs already installed on your machine.

Free to use under the [MIT license](LICENSE).

## Install

Clone the pack once, then link it into the skills folder your agent reads. Run the loop once for each agent you use.

| Agent | Set `SKILLS_DIR` to |
|---|---|
| Claude Code | `$HOME/.claude/skills` |
| Cursor | `$HOME/.cursor/skills` |
| Codex | `$HOME/.codex/skills` |
| Another agent that follows the shared layout | `$HOME/.agents/skills` |

```bash
git clone https://github.com/sigma-michael/sigma-build-factory.git "$HOME/sigma-build-factory"
export SKILLS_DIR="$HOME/.claude/skills"   # change this row to match the table
mkdir -p "$SKILLS_DIR"
for dir in "$HOME/sigma-build-factory"/sigma-*; do
  [ -f "$dir/SKILL.md" ] || continue
  ln -sfn "$dir" "$SKILLS_DIR/$(basename "$dir")"
done
```

Start a new session so the agent reloads skills. In Claude Code or Cursor, type the command, for example `/sigma-plan`. In an agent without slash commands, name the skill and ask it to follow that file.

To share the pack with a project instead of your user account, point the same loop at `.claude/skills`, `.cursor/skills`, or `.agents/skills` inside that repo.

The steps in each `SKILL.md` are the contract. Extra frontmatter such as `disable-model-invocation` and `allowed-tools` is a hint for agents that understand it. An agent that ignores those keys still follows the steps. If it cannot start another skill, and a step says to hand the body to `/sigma-create-ticket`, follow that skill’s file. Do not create the GitHub issue from the calling skill.

## When to run which command

Start at `/sigma-init-docs` once, on a new work repo. A new feature then follows one path. A note from a manual review or from `/sigma-unslop` joins that path at the first command that matches the note. It does not become a ticket on its own.

```mermaid
flowchart TD
  init["/sigma-init-docs<br/>new work repo"] --> plan["/sigma-plan<br/>confirm the requirements"]
  plan --> spec["/sigma-to-spec<br/>lock one feature"]
  spec --> tickets["/sigma-to-tickets<br/>epic or ticket"]
  tickets --> impl["/sigma-implement<br/>one ticket, its own chat"]
  impl --> review["/sigma-review<br/>a different chat"]
  review --> pr["/sigma-pr"]
  pr --> merge["/sigma-merge"]
```

`/sigma-create-ticket` is the filing vehicle inside `/sigma-to-spec` and `/sigma-to-tickets`. Nothing else files an issue. A normal build uses a ticket whose acceptance line was copied from a spec. Say overwrite and `/sigma-implement` still builds. Silence does not build.

### Where a note goes

```mermaid
flowchart TD
  manual["Manual review"] --> note["Note"]
  unslop["/sigma-unslop"] --> note
  note --> kind{"What is it?"}
  kind -->|"New or changed behavior"| planNote["/sigma-plan"]
  kind -->|"Clear bug, spec has no line"| specNote["/sigma-to-spec"]
  kind -->|"Clear bug, spec already states it"| ticketNote["/sigma-to-tickets"]
  planNote --> specNote
  specNote --> ticketNote
  ticketNote --> implNote["/sigma-implement"]
```

A manual review and `/sigma-unslop` both produce a note. The note goes to `/sigma-plan`, `/sigma-to-spec`, or `/sigma-to-tickets`.

- `/sigma-unslop` writes `docs/reviews/<date>-<topic>.md` and stops. Each finding you accept is a note. It does not file an issue.
- New or changed behavior, including a role the requirements do not mention, goes to `/sigma-plan`.
- A clear bug whose spec has no line for that behavior goes to `/sigma-to-spec`.
- A clear bug whose spec line already states that behavior goes to `/sigma-to-tickets` with that spec number. The ticket copies the line.
- If you still want the build, say overwrite and `/sigma-implement` continues. Silence does not build.

`/sigma-implement` flags an epic and names `/sigma-to-tickets`. Say overwrite and it asks once for the script paths. A fix after review goes back to `/sigma-implement` on the same branch, still in the builder chat.

## Skills

**You** means you name the command. On agents that support it, the agent does not start that skill by itself. **Agent** means another skill hands over a finished issue body, and the agent files it. You can still run `/sigma-create-ticket` yourself.

| Skill | Started by | When | What it does |
|---|---|---|---|
| `/sigma-init-docs` | You | The work repo has no layout yet | Copies the requirements file, `scripts/`, and the agent notes. Does not fill in the product. |
| `/sigma-plan` | You | A new behavior, or a note that is not clear yet | Asks one question at a time and writes `docs/requirements/REQUIREMENTS.md` after you say yes. |
| `/sigma-to-spec` | You | Requirements are confirmed, or a clear bug needs a new acceptance line | Files or updates one spec, after you accept a short digest. |
| `/sigma-to-tickets` | You | A spec is ready to split, or a clear bug already matches a spec line | Copies those lines onto tickets or epics and files them through `/sigma-create-ticket`. |
| `/sigma-create-ticket` | Agent | Called by `/sigma-to-spec` or `/sigma-to-tickets` | Files the issue those commands already confirmed, then returns to them. |
| `/sigma-implement` | You | A ticket has a number | Builds that ticket on branch `agent/{N}-{slug}`, in its own chat, and pushes. It does not review its own work. |
| `/sigma-review` | You | The branch is pushed, and this is not the chat that built it | Reads the ticket, the spec, and the diff. Ready names `/sigma-pr`. Not ready sends you back to `/sigma-implement`. |
| `/sigma-pr` | You | Review says ready | Opens one pull request: `Closes #N`, a short summary, and what to look at. |
| `/sigma-merge` | You | You name the pull request numbers | Merges those pull requests, deletes the branch, and removes a clean worktree. |
| `/sigma-unslop` | You | API calls are duplicated, a wrapper hides the endpoint, or the requirements text is hard for an agent to follow | Produces notes for `/sigma-plan`, `/sigma-to-spec`, or `/sigma-to-tickets`. It does not file and it does not build. |

## Where files go

The work repo is the project you are building. This pack does not hold that project’s requirements or scripts.

| Path | Who writes it |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `/sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `/sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `/sigma-unslop` |
| `scripts/**` named in the ticket | `/sigma-implement` |
| GitHub issues | `/sigma-create-ticket` |

If a file fits none of these, the agent asks. It does not invent a new document.

## License

[MIT](LICENSE). Copyright (c) 2026 Michael Khmelik. You can use, copy, and change this pack, including in commercial work. Keep the copyright notice with your copy.
