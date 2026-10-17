---
name: plan-to-tickets
description: Coordinate design, approved plans, and implementation tickets when the user requests a planning-to-ticket workflow or a domain skill delegates that workflow here.
---

# Plan to tickets

Default to design and ticket publication, not implementation. Resume from the
last approved checkpoint: inspect existing plans, issues, branches, and worktrees
before creating duplicates. Apply repository and harness instructions first;
this skill grants no permissions that they withhold.

Domain skills may load this coordinator through the harness's skill mechanism.
Use their domain requirements within this workflow and resume the current
checkpoint if already active. Automatic loading preserves every approval gate
and does not authorize ticket publication, implementation, or dispatch.

## Dependency preflight — before starting or resuming

Require installed upstream skills from `mattpocock/skills`, with these exact
names: `grilling`, `domain-modeling`, `to-tickets`, `tdd`, and `code-review`.
All five are mandatory even for planning-only use. This is orchestration around
their instructions, not a standalone implementation of their workflows.

Inspect the installed skills' frontmatter, instructions, and bundled references
to verify their identity, availability, and supported loading/invocation in this
harness. Follow their declared dependencies as well: upstream `tdd` calls
`codebase-design` when interface or seam design is unresolved; `to-tickets` and
`code-review` require `setup-matt-pocock-skills` when tracker configuration is
missing. Keep those companions installed and load/invoke them when required.
Check further transitive requirements in the installed versions rather than
assuming this list is exhaustive. Load required skills through supported harness
mechanisms when their stage is reached, including in assigned worker contexts.

Respect user-only invocation. Upstream `to-tickets` and
`setup-matt-pocock-skills` set `disable-model-invocation: true`; ask the user to
invoke them when needed and wait. Inspect other installed skills for the same
restriction. Reading their files for preflight is not permission to run them.
Retain this scaffold's approval gates and workspace policy during companion use.

If any required skill is missing or cannot be loaded/invoked as permitted, stop
and report the exact missing/blocked names and remedy. For missing installations,
offer `npx skills add mattpocock/skills` and identify the skills to select; do not
install on the user's behalf. The installer does not resolve dependencies for
this scaffold. For invocation or harness blockers, request user invocation or
the necessary supported capability. Never substitute an inline workflow or
silently download and execute upstream instructions to bypass the blocker.

**Done:** all five direct dependencies and their required companions are
installed and verified, and applicable invocation/capability blockers are resolved.

## Session and workspace policy

Require the companion `agent-dispatch` skill from this repository. Verify it is
installed and load it through the supported skill mechanism before task placement
or repository edits, including planning-only work. It is authoritative for parent
anchoring, workspace placement, worker dispatch, result delivery, and context
rollover throughout this workflow. If missing or blocked, stop and request its
installation or permitted invocation; do not substitute an inline copy.

Retain this coordinator's approval gates, ticket readiness, and acceptance rules.
Loading either skill does not authorize implementation or dispatch.

## 1. Establish context

Read repository and ancestor `AGENTS.md` instructions, the README, domain docs,
glossary, relevant ADRs, issue-tracker conventions, and applicable contribution
and testing guidance. Inspect existing implementations and templates without
modifying them. Establish the goal, target repository, short task slug, current
state, affected users, and known dependencies. Investigate discoverable facts
instead of asking the user to retrieve them.

When an issue or change becomes part of the work, link it immediately with
`session.link` if available, including tickets or PRs created, reviewed, or
resolved later. Otherwise retain its URL in the handoff. Never claim a tool
action succeeded without confirmation.

**Done:** the parent is placed and anchored under the session policy, and the
relevant context and unresolved design questions are identified.

## 2. Grill the design

Load and follow required `grilling` for design questions and `domain-modeling`
for terms and decisions. Scope the design to behavior, interfaces/data contracts,
ownership and boundaries, state and error semantics, compatibility/migration,
security, test seams, operational constraints, dependencies, and explicit
non-goals where relevant. Use an authorized workspace for glossary/ADR edits.
Keep temporary research in the ancestor instructions' ephemeral-document location,
not an arbitrary repo folder. Required companion capabilities, including native
subagents where called for, are subject to preflight and the session policy.

**Done:** no material design branch remains unresolved and the user explicitly
confirms shared understanding. Approval of a round does not approve unanswered
branches or authorize implementation.

## 3. Save and approve the plan

Create a separate implementation plan with a title, coarse step checklist,
approved decisions, constraints, acceptance checks, prerequisites, and non-goals.
If Project knowledge is available, list existing entries, save the plan, show it,
and wait for review. Otherwise agree a durable location with the user according
to repository rules, save it there using the workspace policy, and present it.
Honor ancestor ephemeral-document rules for temporary drafts and handoffs.

Verify prerequisite readiness from implementation commits and relevant actual
checks, not labels or closed issues alone. Inspect reusable templates, generators,
APIs, and upstream changes as applicable; never copy another session's uncommitted
work. If a prerequisite is unavailable, planning and publication may continue,
but record the blocking dependency and its evidence. Report missing, failed, or
unavailable checks explicitly.

**Done:** the saved plan has explicit user approval. Do not scaffold or implement
the product while awaiting approval.

## 4. Approve the breakdown separately

Ask the user to invoke required `to-tickets`, then follow it against the approved
plan. Keep its drafting/approval stage separate from publication until the gate
below is satisfied. Each ticket includes purpose, scope/non-goals, approved-plan
reference or durable decision summary, acceptance criteria, validation commands
or methods, and genuine blocking dependencies. Preserve every plan constraint
in its relevant tickets and keep existing parent issues.

Start with a minimal working path using the approved foundations. Mark readiness
blockers explicitly; labels alone do not make blocked tickets dispatchable.
Present the entire breakdown and ask for separate approval to publish it.
Changes that invalidate the approved design or plan return to that checkpoint.

**Done:** the user explicitly approves the breakdown and its publication. Plan
approval alone is insufficient.

## 5. Publish tickets, then stop

Publish the approved tickets to the configured tracker with its documented
labels and native blocking relationships. If native dependencies are unavailable,
use explicit blocking links/checklists and identify the limitation. Check for
existing tickets before creation; if publication fails partway, report what
exists and resume without duplicates. If tracker access is unavailable, retain
the approved drafts and report the publication blocker rather than claiming done.

Return ticket links, dependency order, the approved plan location, and unresolved
blockers. **Stop here unless the user explicitly requests implementation or
dispatch.** Invocation of this skill, plan/breakdown approval, or ticket
publication is not an instruction to start implementation agents.

**Done:** approved tickets exist, dependencies are recorded, and the user has
their links; otherwise report the precise blocked state.

## 6. Implement only on explicit request

Retain the parent's established task worktree; resuming implementation is not a
new placement step. Apply the session and workspace policy before assigning any
writer. Claim tickets according to tracker conventions and dispatch only those
with verified completed blockers. When dispatch is not authorized or supported,
report the blocker and agree a safe manual execution path that keeps the parent
anchored rather than inventing agent capabilities.

- Apply `agent-dispatch`'s writer-isolation and integration rules to ticket
  execution; parallelize only independent tickets with verified prerequisites.
- Supply each writer with the ticket body/comments, approved decisions, exact
  worktree and branch, prerequisite commits, scope, and acceptance checks.
  Require commits, actual validation results, and unresolved blockers in return.
- Load and follow required `tdd` at user-confirmed test seams, including its
  companion requirements. Resolve applicability questions with the user rather
  than substituting a different implementation workflow.
- Load and follow required `code-review` against the approved spec and repository
  standards, supplying the fixed point and required context. Resolve actionable
  findings and rerun affected checks. Missing review capabilities are blockers,
  not permission to replace the upstream review process.
- Verify acceptance criteria against actual results before resolving a ticket.
  Keep incomplete tickets open with a clear status. Tick completed steps in the
  saved plan without rewriting approved constraints. Never fabricate checks;
  distinguish passed, failed, skipped, and unavailable validation.
- Make local implementation commits, subject to repository rules. Keep them
  local: pushing, PR creation, merging, package publication, and deployment each
  require explicit user authorization covering that action. Ticket publication
  or implementation permission does not authorize these actions.

**Done:** report workspace/branch and worker IDs, local commits, completed and
remaining tickets, validation evidence, and blockers. Do not claim integration
or completion solely from a worker's summary.
