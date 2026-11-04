# sigma-build-factory

Agent skills for building Sigma work from a requirements document through to a merged pull request. Claude Code, Cursor, Codex, and any other agent that reads a `SKILL.md` folder can run them.

You keep the product decisions in one requirements file. The agent writes API scripts only where a ticket says it may. Each command finishes, tells you the next command, and waits. One worker writes the scripts and opens the pull request. A second worker reviews.

Commands you type use the `sigma-` prefix. `/sigma-implement` starts `/sg-tdd`, `/sg-pr`, and `/sg-review`. `/sigma-create-ticket` files issues for the commands that need one, and you can still run it yourself.

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
for dir in "$HOME/sigma-build-factory"/sigma-* "$HOME/sigma-build-factory"/sg-*; do
  [ -f "$dir/SKILL.md" ] || continue
  ln -sfn "$dir" "$SKILLS_DIR/$(basename "$dir")"
done
```

Start a new session so the agent reloads skills. In Claude Code or Cursor, type the command, for example `/sigma-plan`. In an agent without slash commands, name the skill and ask it to follow that file.

To share the pack with a project instead of your user account, point the same loop at `.claude/skills`, `.cursor/skills`, or `.agents/skills` inside that repo.

The steps in each `SKILL.md` are the contract. Extra frontmatter such as `disable-model-invocation` and `allowed-tools` is a hint for agents that understand it. An agent that ignores those keys still follows the steps. If it cannot start another skill, and a step says to hand the body to `/sigma-create-ticket`, follow that skill’s file. Do not create the GitHub issue from the calling skill.

## When to run which command

Start at `/sigma-init-docs` once, on a new work repo. A new feature then follows one path. A pasted note starts at `/sigma-feedback`. That command follows the matching skill in the same chat. The note does not become a ticket on its own.

```mermaid
flowchart TD
  init["/sigma-init-docs"] --> plan["/sigma-plan"]
  plan --> spec["/sigma-to-spec"]
  spec --> tickets["/sigma-to-tickets"]
  tickets --> impl["/sigma-implement"]
  impl --> merge["/sigma-merge"]
  spec --> file["/sigma-create-ticket"]
  tickets --> file
```

`/sigma-to-spec` and `/sigma-to-tickets` hand the body to `/sigma-create-ticket`. Nothing else files an issue. A normal build uses a ticket whose acceptance line was copied from a spec. Say overwrite and `/sigma-implement` still builds. Silence does not build.

### Where a note goes

```mermaid
flowchart TD
  feedback["/sigma-feedback"] -->|New behavior| plan["/sigma-plan"]
  feedback -->|No spec line| spec["/sigma-to-spec"]
  feedback -->|States it| tickets["/sigma-to-tickets"]
  feedback -->|Drifted scripts| unslop["/sigma-unslop"]
  plan --> spec
  spec --> tickets
  tickets --> impl["/sigma-implement"]
  spec --> file["/sigma-create-ticket"]
  tickets --> file
```

A pasted note starts at `/sigma-feedback`. That command reads the requirements and the specs, then follows one skill in the same chat.

- New behavior, including a role the requirements do not mention, follows `/sigma-plan`. Plan asks the questions.
- A clear bug whose spec has no line follows `/sigma-to-spec`.
- A clear bug whose spec line already states that behavior follows `/sigma-to-tickets` with that spec number. The ticket copies the line.
- Drifted scripts follow `/sigma-unslop`. It writes `docs/reviews/<date>-<topic>.md` and stops. It does not file an issue.
- If you still want the build, say overwrite and `/sigma-implement` continues. Silence does not build.

`/sigma-implement` flags an epic and names `/sigma-to-tickets`. Say overwrite and it chooses the script. When the ticket names no `scripts/` path, it picks the closest file, or one new file, and prints that path. A wrong file is a later `/sigma-unslop` review. That review does not edit the script.

### How a build finishes

```mermaid
flowchart TD
  impl["/sigma-implement"] --> tdd["/sg-tdd"]
  tdd --> pr["/sg-pr"]
  pr --> review["/sg-review"]
  tdd -->|"Browser write"| file["/sigma-create-ticket"]
  review --> verdict{Verdict}
  verdict -->|Ready| done["Implementation complete"]
  done --> merge["/sigma-merge"]
  verdict -->|Not ready| lines["You name the lines"]
  lines --> tdd
```

You type `/sigma-implement`. It matches the ticket and opens the worktree. One worker follows `/sg-tdd`, then `/sg-pr`. A second worker follows `/sg-review`. The implement chat does not edit scripts and does not judge the diff.

- Ready prints implementation complete and names `/sigma-merge` with the pull request number.
- Not ready lists each unmet line. You name the lines to send back. Silence does not start a fix. The same worker pattern runs again on the same branch, the pull request updates, and a new fresh review runs.

A browser write adds a comment through `/sigma-create-ticket`, and `/sigma-implement` prints the warning. The comment does not change acceptance lines. Review still runs. A ready verdict still names `/sigma-merge`, and that merge still merges the numbers you name.

## Skills

The first column is the command you type. A row with `none` describes that command. The next rows name each skill that command starts, and describe that skill.

| You type | System skill | What it does |
|---|---|---|
| `/sigma-init-docs` | none | Copies the requirements file, `scripts/`, and the agent notes. Does not fill in the product. |
| `/sigma-feedback` | none | Reads the requirements and the specs, then follows one of `/sigma-plan`, `/sigma-to-spec`, `/sigma-to-tickets`, or `/sigma-unslop` in this chat. |
| `/sigma-feedback` | `/sigma-create-ticket` | Returns the spec list or one issue. Does not file. |
| `/sigma-plan` | none | Asks one question at a time and writes `docs/requirements/REQUIREMENTS.md` after you say yes. |
| `/sigma-to-spec` | none | Writes one spec digest and waits for yes. |
| `/sigma-to-spec` | `/sigma-create-ticket` | Files or updates that spec. |
| `/sigma-to-tickets` | none | Copies the spec lines onto tickets or epics and waits for one confirm. |
| `/sigma-to-tickets` | `/sigma-create-ticket` | Files those issues. |
| `/sigma-create-ticket` | none | You can run it. It files a SPEC, EPIC, or TICKET whose body is already confirmed. |
| `/sigma-unslop` | none | Writes one review and stops. It does not file and it does not build. A pasted product note starts at `/sigma-feedback`. |
| `/sigma-implement` | none | Matches one ticket, opens the worktree, and prints the decision. When the ticket names no script, it chooses the closest file or one new path. It does not edit scripts and it does not judge the diff. |
| `/sigma-implement` | `/sg-tdd` | Runs red, then green, commits, and pushes. A rows-route 404 writes in the browser and the run continues. |
| `/sigma-implement` | `/sg-pr` | Opens one pull request on the pushed branch. Same worker as `/sg-tdd`. It does not wait for review. |
| `/sigma-implement` | `/sg-review` | A second worker reads the ticket, the spec, and the diff. It returns ready or not ready. An API-failed comment is not an unmet line. |
| `/sigma-implement` | `/sigma-create-ticket` | Posts one comment when the write used the browser. The comment does not replace the body. Review and merge still run. |
| `/sigma-merge` | none | Merges the pull request numbers you name, using the repository default method. It deletes the branch and removes a clean worktree. An API-failed comment does not drop a named number. |

## Where files go

The work repo is the project you are building. This pack does not hold that project’s requirements or scripts.

| Path | Who writes it |
|---|---|
| `AGENTS.md`, `docs/README.md`, `.gitignore` | `/sigma-init-docs` |
| `docs/requirements/REQUIREMENTS.md` | `/sigma-plan` |
| `docs/reviews/<date>-<topic>.md` | `/sigma-unslop` |
| `scripts/**` named in the ticket | `/sg-tdd` |
| GitHub issues | `/sigma-create-ticket` |

If a file fits none of these, the agent asks. It does not invent a new document.

## License

[MIT](LICENSE). Copyright (c) 2026 Michael Khmelik. You can use, copy, and change this pack, including in commercial work. Keep the copyright notice with your copy.
