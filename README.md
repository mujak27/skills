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
npx skills add mujak27/skills --skill agent-dispatch

# Optional: install globally instead
npx skills add mujak27/skills --skill plan-to-tickets --global
npx skills add mujak27/skills --skill agent-dispatch --global
```

Install both repositories into the same agent scope; for global use, add
`--global` to the upstream command too. Select at least `grilling`,
`domain-modeling`, `to-tickets`, `tdd`, `code-review`, `codebase-design`, and
`setup-matt-pocock-skills` from upstream. The first five are hard dependencies,
even when implementation is not requested. The latter two support upstream's
conditional interface-design and repository-setup requirements. Keep any further
dependencies required by the installed upstream versions available as well.

## Use

### Plan to tickets

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

`plan-to-tickets` also requires the companion `agent-dispatch` from this repository,
including for planning-only workspace placement. Install both into the same scope.
The upstream dependencies above belong to `plan-to-tickets`; `agent-dispatch`
can be installed and used independently without them.

### Agent dispatch

> Use agent-dispatch to coordinate these authorized workers and their follow-ups.

This shared skill owns parent anchoring, workspace isolation, nested children
versus authorized cross-repository sessions, result delivery, and worker context
rollover. It can be loaded directly or by another skill; loading it does not
authorize dispatch. See its authoritative
[session and workspace policy](skills/agent-dispatch/SKILL.md#session-and-workspace-policy).

When current worker context reaches **70% of the model's context window or
250,000 tokens, whichever comes first**, the parent requests a compact handoff,
waits for the worker to stop editing, and starts a fresh equivalent worker in the
same workspace. The old worker is retired, or closed/archived only if supported.
Unavailable usage metrics require disclosure and checkpoint-based rollover,
not invented token counts. See the [rollover procedure](skills/agent-dispatch/SKILL.md#context-rollover).

These are skill instructions, not enforced sandboxing. Session, subagent, Project
knowledge, and issue-tracker integrations require support from the agent harness;
missing capabilities must be reported rather than simulated.

## Attribution

Credit to [Matt Pocock’s agent skills](https://github.com/mattpocock/skills) for
the underlying workflows. `plan-to-tickets` adds scaffolding to coordinate those
skills around my usual planning, approval, and delegation process.
