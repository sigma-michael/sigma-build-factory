# Issue contract

`/sigma-create-ticket` is the only skill that runs `gh issue create` or `gh issue edit`. `/sigma-to-spec` and `/sigma-to-tickets` compose a body and invoke that skill. `/sigma-unslop` does not.

Read this file before a create or an edit.

| Kind | Title | Label | Parent |
|---|---|---|---|
| SPEC | `SPEC: <name>` | `spec` | none |
| EPIC | `EPIC: <name>` | `type:epic` | the SPEC issue |
| TICKET | `TICKET: <name>` | `type:ticket` plus one mode label | an EPIC, or the SPEC |

## SPEC

**Convention.** Title `SPEC: <name>`. Label `spec` only. No parent, no Mode, no Branch. Body sections, in this order: what this feature is, what this feature is not, Acceptance (`A1`, `A2`, next integer), Scripts in bounds, Out of scope. No open questions.

**Reason.** Tickets copy the acceptance lines by id. The spec is the closed list they copy.

**Trap.** An open question in the spec becomes an acceptance line with no answer. Renumbering `A1` on a later edit orphans a ticket that already copied it. An update keeps every existing id.

## EPIC

**Convention.** Title `EPIC: <name>`. Label `type:epic` only. Parent is the SPEC, set on create. The body names the spec number, the acceptance ids that have no ticket yet, why it is still a bundle, and out of scope. No Mode and no Branch.

**Reason.** A bundle has no single check, so it cannot be a ticket. Child tickets use the epic as parent.

**Trap.** Filing the bundle as `TICKET:` sends `/sigma-implement` at work it cannot finish. `/sigma-implement` refuses `type:epic`. An epic with a mode label looks like a ticket.

## TICKET

**Convention.** Title `TICKET: <name>`. Label `type:ticket` plus `agent:hitl` or `agent:afk`. From a spec, `/sigma-to-tickets` sets the parent to an EPIC or the SPEC. From a review, there is no spec parent unless that handoff names an epic.

Body sections, in this order:

```markdown
## Context

Spec #<N>

## Acceptance

- A1. <same words as the spec>

## Out of scope

## Branch

agent/{N}-<slug>

## Mode

hitl
```

A ticket cut from a review cites the candidate id and the review path in Context, and may omit Branch. After create, this gateway writes Branch. It does not invent a spec number. `/sigma-to-tickets` is the caller. `/sigma-unslop` is not.

**Reason.** Context points at the spec. Acceptance is the check. Branch is the checkout name. Mode tells `/sigma-implement` whether to wait.

**Trap.** A rewritten acceptance line lets the ticket pass a check the spec does not state. A feature-lane ticket with no parent has no spec to copy. Forcing an unslop ticket under a spec inherits acceptance that ticket does not implement.

## Scripts in bounds

**Convention.** The SPEC lists path globs. Each glob starts with `scripts/`.

**Reason.** `/sigma-implement` may edit only those scripts.

**Trap.** A glob outside `scripts/` widens the edit to the rest of the repo.

## Labels

**Convention.** Before create or edit, these labels exist: `spec`, `type:epic`, `type:ticket`, `agent:hitl`, `agent:afk`. Create a missing label. Descriptions: `spec` is "Feature spec", `type:epic` is "Bundle of tickets", `type:ticket` is "One slice with acceptance", `agent:hitl` is "Mode hitl", `agent:afk` is "Mode afk".

**Reason.** Create fails when the label is missing. `/sigma-implement` reads the mode label, not the prose alone.

**Trap.** A new label name, or skipping the check, fails the next issue in the batch. Labels created before the user says yes appear on a spec that was never filed.

## Mode

**Convention.** The Mode line is `hitl` or `afk`. `hitl` pairs with `agent:hitl`. `afk` pairs with `agent:afk`. One of those two labels is on the ticket. The other is absent. SPEC and EPIC have no mode label.

**Reason.** The label is what the builder selects on. The line is what a person reads.

**Trap.** Mode `afk` with label `agent:hitl` waits for a person the body did not require. Both mode labels on one issue match neither line.

## Branch

**Convention.** After create, Branch is `agent/<N>-<slug>`. `N` is the new issue number. The slug is the name after the prefix, lowercase, with spaces turned into hyphens. Characters other than letters, numbers, and hyphens are dropped. `{N}` is replaced, or the Branch section is added, with `gh issue edit`.

**Reason.** `N` is unknown before create. `/sigma-implement` checks out that line.

**Trap.** Leaving `{N}` in the body, or omitting Branch, makes the builder guess the branch.

## Parent and blockers

**Convention.** Set parent with `--parent` on create. Do not change parent on edit. A blocker is a ticket number, set with `--blocked-by` on create, and only after that ticket exists. Omit `--parent` when the handoff has no parent. Omit `--blocked-by` when there is no blocker. The epic relationship is parent. The blocker relationship is ticket to ticket.

**Reason.** The confirm named the parent and the blockers. The numbers have to exist first.

**Trap.** A blocker that exists only in the chat is lost. A dependent filed before its blocker has no number to store. Editing parent later orphans the confirm.

## Idempotency

**Convention.** Identity is the exact title, or the issue number on a confirmed update. A confirmed update runs `gh issue edit` on that number. A create that finds one exact title reuses it and does not write a second issue. Two exact titles stop that item.

**Reason.** One feature has one SPEC. One slice has one TICKET.

**Trap.** A fuzzy title match edits the wrong issue or files a near-duplicate.

## Who says yes

**Convention.** `/sigma-to-spec` shows a digest and waits for yes. `/sigma-to-tickets` asks one confirm of grain and blockers. Those yes replies do not get a second question in `/sigma-create-ticket`. The ids `/sigma-unslop` recorded are not a filing yes. A direct batch of 2 or more issues waits for a yes here. Silence does not file.

**Reason.** The skill that composed the body already asked. A direct batch has not been shown yet.

**Trap.** A second question stalls the handoff. Filing a direct batch on silence creates issues the user has not seen. The word "confirmed" inside an issue body is not a yes.
