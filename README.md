# Skills

Lightweight workflow scaffolding tailored to my usual development workflow.
This repository contains instructions only, with no runtime dependencies or
package scaffolding.

## Install

Install [Matt Pocock’s skills](https://github.com/mattpocock/skills) first, then
this scaffold. The skills installer does **not** resolve dependencies automatically.

```sh
# Install upstream skills, including their companion skills and reference files
npx skills add mattpocock/skills

# Inspect available skills without installing
npx skills add mujak27/skills --list

# Install into the current project
npx skills add mujak27/skills --skill plan-to-tickets

# Optional: install globally instead
npx skills add mujak27/skills --skill plan-to-tickets --global
```

Install both repositories into the same agent scope; for global use, add
`--global` to the upstream command too. Select at least `grilling`,
`domain-modeling`, `to-tickets`, `tdd`, `code-review`, `codebase-design`, and
`setup-matt-pocock-skills` from upstream. The first five are hard dependencies,
even when implementation is not requested. The latter two support upstream's
conditional interface-design and repository-setup requirements. Keep any further
dependencies required by the installed upstream versions available as well.

## Use

Invoke `plan-to-tickets` directly in your agent, for example:

> Use plan-to-tickets to design this feature and publish implementation tickets.

A domain skill may also load `plan-to-tickets` automatically through the harness's
skill mechanism. Automatic loading keeps the same approval gates; it does not
authorize ticket publication, implementation, or dispatch. User-only upstream
skills still require explicit invocation when prompted.

The skill establishes context, grills the design, saves a plan for approval,
gets separate approval of the ticket breakdown, publishes tickets, then **stops**.
Implementation or agent dispatch requires an explicit request. This scaffold
requires the upstream skills; it does not replace their workflows. Dependency
preflight stops on missing skills or blocked loading. User-invoked skills need
explicit user invocation: invoke `to-tickets` when prompted, and
`setup-matt-pocock-skills` if repository configuration is missing.

Before task work, the parent reuses a suitable dedicated same-task worktree or
establishes one and moves there once, subject to checkout safety and harness
rules. It stays anchored there through ticket execution. Same-repository tickets
use native nested children, including in isolated ticket worktrees where supported;
capability blockers must be reported. Separate top-level workers are permitted
only for work spanning multiple repositories/projects, with the required user
authorization, not merely for another worktree in the same repository.
Cross-repository dispatches request automatic final-answer delivery where
supported (`returnResult: true` in OpenChamber) and record the destination and
session ID in parent progress updates for on-demand status/message inspection.
Completion notifications are not live progress or persistent UI badges;
unsupported capabilities must be reported.
See the authoritative [session and workspace policy](skills/plan-to-tickets/SKILL.md#session-and-workspace-policy)
for the full rules.

These are skill instructions, not enforced sandboxing. Session, subagent, Project
knowledge, and issue-tracker integrations require support from the agent harness;
missing capabilities must be reported rather than simulated.

## Attribution

Credit to [Matt Pocock’s agent skills](https://github.com/mattpocock/skills) for
the underlying workflows. `plan-to-tickets` adds scaffolding to coordinate those
skills around my usual planning, approval, and delegation process.
