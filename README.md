# Skills

Lightweight workflow scaffolding tailored to my usual development workflow.
This repository contains instructions only, with no runtime dependencies or
package scaffolding.

## Install

```sh
# Inspect available skills without installing
npx skills add mujak27/skills --list

# Install into the current project
npx skills add mujak27/skills --skill plan-to-tickets

# Optional: install globally instead
npx skills add mujak27/skills --skill plan-to-tickets --global
```

## Use

Explicitly invoke `plan-to-tickets` in your agent, for example:

> Use plan-to-tickets to design this feature and publish implementation tickets.

The skill establishes context, grills the design, saves a plan for approval,
gets separate approval of the ticket breakdown, publishes tickets, then **stops**.
Implementation or agent dispatch requires an explicit request. Optional companion
skills improve individual stages; inline fallbacks keep the workflow standalone.

The parent stays in its starting workspace. Small-ticket delegation uses nested
children; a separate worker session is permitted only for an authorized
file-changing workspace boundary, subject to user permission and harness rules.
See the authoritative [session and workspace policy](skills/plan-to-tickets/SKILL.md#session-and-workspace-policy)
for the full rules.

These are skill instructions, not enforced sandboxing. Session, subagent, Project
knowledge, and issue-tracker integrations require support from the agent harness;
missing capabilities must be reported rather than simulated.

## Attribution

Credit to [Matt Pocock’s agent skills](https://github.com/mattpocock/skills) for
the underlying workflows. `plan-to-tickets` adds scaffolding to coordinate those
skills around my usual planning, approval, and delegation process, with inline
fallbacks when companion skills are unavailable.
