---
name: plan-to-tickets
description: Turn an idea into an approved design, a separately approved plan and ticket breakdown, then published implementation tickets.
disable-model-invocation: true
---

# Plan to tickets

Default to design and ticket publication, not implementation. Resume from the
last approved checkpoint: inspect existing plans, issues, branches, and worktrees
before creating duplicates. Apply repository and harness instructions first;
this skill grants no permissions that they withhold.

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

This section is authoritative for session placement and delegation throughout
the workflow, including optional implementation.

- **Place once, then anchor.** Before starting task work, inspect the parent’s
  current directory, Git state, and existing worktrees. Read-only discovery and
  dependency preflight may precede placement. If already in a suitable dedicated
  same-task worktree, reuse it without moving. Otherwise inspect and reuse a
  suitable existing same-task worktree, or create one under the rules below, then
  move the coordinating parent into it once with supported session tooling.
  Verify suitability from task ownership, branch, prerequisites, and current
  changes; a matching directory name alone is insufficient.
  Verify and record the task path and branch before proceeding, including before
  any repository edit (code, tests, configuration, or docs). Keep the parent
  anchored there for the rest of the task, including ticket execution; ticket
  worktrees and other repositories are worker destinations, not parent moves.
- **Preserve starting work.** Inspect branch changes and open PRs before initial
  placement. For unfamiliar uncommitted changes or an open-PR starting checkout,
  offer to move this session into a new task worktree and wait for approval where
  required by the harness. An explicit worktree request permits proceeding
  without asking again, subject to all remaining safety constraints. Inspect
  whether move tooling carries changes; preserve unfamiliar work without
  discarding, overwriting, or silently carrying it into this task. Report unsafe
  or unsupported placement as a blocker rather than editing in the wrong checkout.
- **Nest same-repository work.** Use native nested child subagents that return
  to the anchored parent for same-repository tickets, including tickets assigned
  isolated worktrees. Verify that the harness supports scoping each child to its
  exact assigned worktree. A top-level `session.create` is not a nested subagent;
  an isolated same-repository worktree does not justify a top-level session.
- **Cross only repository/project boundaries.** A separate top-level worker is
  permitted only when the task actually spans multiple repositories/projects and
  authorized file-changing work needs that other repository/project. A different
  folder or worktree in the same repository, task size, read-only investigation,
  preferred model, or concurrency alone is not a reason. Prefer a scoped nested
  child across repositories too, if supported. Top-level dispatch still requires
  all user authorization and harness permissions; ask for an explicit user request
  first when required. This is permission, not automatic dispatch.
- **Keep workers bounded.** Give each worker an exact workspace and assignment
  while leaving the parent anchored. Once placed, a worker reuses that workspace
  for its assignment; it must not recursively spawn replacement sessions/worktrees
  for that assignment. It may use native nested children within its assigned scope.
  Report unavailable nesting, worktree scoping, or other capabilities and ask for
  a supported handoff or execution path. A same-repository capability blocker is
  not permission to create a top-level worker or relocate the parent. Arrange
  results back to the parent when supported, and record worker IDs, branches,
  and paths for the user.
- **Base deliberately.** Resolve the original checkout from Git common/worktree
  metadata; use its sibling `<repo>-worktrees/<filesystem-safe-slug>`, never a
  nested or singular `-worktree` container. Inspect the configured default remote
  and branch instead of assuming `origin/main`. Before creating each new task or
  ticket worktree, fetch that remote's default branch and stop on fetch failure.
  Branch from the freshly updated remote-tracking ref. Use an explicit
  `git worktree add` destination if tools cannot honor the path. Integrate and
  verify required prerequisite commits before assigning dependent work; ticket
  closure alone does not put code on a branch. Stop on conflicts and report them.

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

- Use one writer at a time in a shared task worktree. Parallel file-changing
  tickets require separate isolated ticket worktrees, even for different files;
  parallelize only independent work using supported, worktree-scoped native
  children for same-repository tickets. Worktree isolation does not authorize
  top-level sessions. Integrate returned commits serially into the anchored
  task branch and rerun relevant combined checks before unblocking dependents.
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
