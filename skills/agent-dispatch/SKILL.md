---
name: agent-dispatch
description: Coordinate authorized subagent dispatch, workspace placement, result delivery, and context rollover when assigning work to workers or continuing an existing worker session.
---

# Agent dispatch

Shared policy for worker coordination, independent of planning or ticketing.
Load this skill before dispatching or continuing a worker. Apply repository and
harness instructions first; loading this skill grants no permission to dispatch,
edit files, create top-level sessions, or bypass workflow approval gates.

## Session and workspace policy

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
  to the anchored parent for same-repository assignments, including assignments in
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
  for that assignment. The parent manages context rollover under the procedure
  below. A worker may use native nested children within its assigned scope.
  Report unavailable nesting, worktree scoping, or other capabilities and ask for
  a supported handoff or execution path. A same-repository capability blocker is
  not permission to create a top-level worker or relocate the parent. Arrange
  results back to the parent when supported, and record worker IDs, branches,
  and paths for the user.
- **Track cross-repository results.** Before an authorized cross-repository
  dispatch, verify automatic result delivery and destination-scoped inspection
  capabilities. With OpenChamber, set `returnResult: true` on `session.create`
  with a prompt, or on each `session.send` that dispatches work to an existing
  worker. Scope the dispatch to the destination using `projectId` or `directory`
  (not both). This requests the worker's final answer back in the parent session
  when it finishes; the parent remains anchored in repository A while the worker
  runs in repository B. For native children or other harnesses, use only verified
  supported result-return mechanisms, not invented equivalents.
  After dispatch confirmation, include the assignment, destination project and
  absolute directory/worktree, returned session ID, and result-delivery status in
  a parent progress update. Retain these identifiers for later updates and handoff;
  resolve the actual worker path if dispatch created a worktree.
  On demand, use OpenChamber `session.status` with that `sessionId` and destination
  `projectId` or `directory`, not the parent's default scope. Use `session.messages`
  with the same scope for detail (`lastAssistant: true` for the final answer).
  Reading status/messages does not dispatch work; `session.send` starts a new
  prompt and is not a progress query. Report observed status and blockers without
  treating completion as proof of acceptance or integration.
  Automatic final-answer delivery is a completion notification, not live progress.
  Do not promise persistent UI badges or automatic live updates. If delivery,
  destination resolution, status, or message inspection is unavailable, report the
  exact capability blocker and agree a supported handoff/inspection path before
  relying on dispatch. Keep the parent anchored and the same-repository boundary
  intact; missing capabilities grant no additional dispatch permissions.
- **Base deliberately.** Resolve the original checkout from Git common/worktree
  metadata; use its sibling `<repo>-worktrees/<filesystem-safe-slug>`, never a
  nested or singular `-worktree` container. Inspect the configured default remote
  and branch instead of assuming `origin/main`. Before creating each new task or
  ticket worktree, fetch that remote's default branch and stop on fetch failure.
  Branch from the freshly updated remote-tracking ref. Use an explicit
  `git worktree add` destination if tools cannot honor the path. Integrate and
  verify required prerequisite commits before assigning dependent work; ticket
  closure alone does not put code on a branch. Stop on conflicts and report them.

## Dispatch and continuation

Before dispatch, verify authorization, supported worker placement and result
delivery, and prerequisite readiness. Supply the exact assignment, scope and
non-goals, workspace/branch, applicable instructions and skills, approved
decisions, prerequisite commits, and acceptance checks. Require actual results,
changed files or commits, validation evidence, and blockers in return.

Use one writer at a time in a shared worktree. Parallel file-changing assignments
require separate isolated worktrees, even for different files. Integrate returned
commits serially and rerun relevant combined checks before unblocking dependents.
Worker completion alone is not acceptance or integration.

Before each follow-up assignment to an existing worker, check context pressure.
Give workers the rollover rule at dispatch so they can flag pressure and prepare
a handoff at a safe checkpoint while running.

## Context rollover

**Trigger:** retire a worker context when its current context reaches either
**70% of its model's context window or 250,000 tokens, whichever comes first**:
`threshold = min(0.70 * context_window, 250000)`.
Measure current occupied context, not cumulative billed tokens across turns,
cached-token totals, output tokens alone, or message count. Use only reliable
usage and model-limit information exposed by the harness. When only one limit
can be evaluated, enforce it and report the other as unavailable. When reliable
usage is unavailable, disclose that limitation and roll over at a meaningful
checkpoint after substantial accumulated history or large outputs; never claim
the numeric threshold was checked. Reserve room for the handoff; if the next
assignment would exceed the threshold, roll over before sending it.

The parent owns replacement. Use this sequence when further work remains:

1. **Quiesce and hand off.** Ask the existing worker to finish or pause its current
   safe unit, stop editing, and write a compact handoff. Follow the workspace's
   ephemeral-document policy; here use a project/task-specific file under
   `/Users/violet/codes/violet/docs/`. Elsewhere use the configured location, or
   agree one if none is defined. Include:
   - Objective, scope/non-goals, approvals, and constraints still in force.
   - Exact repository, worktree, branch, worker ID, and relevant issue/PR links.
   - Completed work, commits, uncommitted files and their ownership; preserve
     uncommitted work in place rather than resetting or copying it elsewhere.
   - Decisions and essential evidence, actual checks and outcomes, blockers,
     remaining acceptance criteria, and the exact next action.
   - Pointers to necessary files or artifacts rather than full transcripts,
     repeated logs, or secrets.
2. **Verify the boundary.** Wait for confirmation that the old worker is no longer
   editing. Read the handoff and inspect the workspace state against it. Resolve
   missing constraints or ownership before transferring the assignment. If the
   worker cannot produce a handoff, report the blocker and obtain an agreed
   recovery path; do not silently resume from an assumed summary.
3. **Replace in place.** Start a fresh worker of the same permitted kind in the
   same assigned workspace and branch. Supply the handoff path, bounded next
   assignment, applicable instructions/skills, and required source pointers. Have
   it read the handoff and verify the workspace before editing. Start with fresh
   context rather than resuming the old ID or forking its full transcript. Recheck
   authorization, scoping, and result delivery; rollover creates no extra rights.
4. **Retire and record.** Record the old-to-new worker IDs, handoff path, workspace,
   remaining assignment, and result-delivery status in a parent update. Send
   further assignments only to the replacement. Close or archive the old worker
   only through a supported, authorized capability; otherwise leave it inactive
   and report it as retired, not closed. A late old-worker result is historical
   evidence, not permission to resume its writes.

If the assignment is complete, collect its final evidence and retire the worker
without creating a replacement. If handoff storage, quiescence, fresh-child
creation, or workspace scoping is unsupported, report the exact blocker and agree
a supported path. Preserve the parent anchor and dispatch boundaries.

**Done:** one active owner remains for the assignment, its workspace and worker
ID are recorded, the handoff is readable, and results return to the parent. Token
savings depend on handoff size and caching; do not promise a measured reduction
without usage evidence.
