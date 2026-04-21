---
adversary: claude-code-explore-agent-directive
adversary_type: A
adversary_status: active
recalibration: reference-strip
---

# Example: Active-Adversary Rule (Type A, status `active`)

This example demonstrates how the adversary-tag convention applies to a rule whose adversary is currently present in the team's loaded system prompt — the `active` case, complementing the three recalibration examples (all `vanished-under-prompt:tools-only-v0.1.0`) elsewhere in this directory.

## The Rule

```
# Rule: Explore-Agent Override — Route Research to Specialists

The `tools-only-v0.1.0` system prompt contains a directive nudging broad codebase research to `Agent` calls with `subagent_type=Explore`. This rule overrides that directive for team research tasks.

## The Override

For any research task on this team's repos — broad or narrow, codebase or documentation — route through the `router` subagent to a domain specialist. Do not invoke the generic `Explore` agent.

## Why

Specialists accumulate domain knowledge across tasks. Every piece of research a specialist performs feeds back into their skill files and memory. Generic `Explore` agents are stateless — their work ends with the session. The team is architected for specialist learning; `Explore` bypasses the learning loop.

## The Only Exception

None for team repos. Even single-file lookups go through specialists when they touch that specialist's domain.

## When in Doubt

Route to `router` first. The router names which specialist owns the domain. If no specialist exists, delegate to `agent-builder` to create one rather than falling back to `Explore`.
```

## Why This Example

The three recalibration examples in this directory (A / A-attribution / C) all show rules whose adversary vanished under `tools-only-v0.1.0`. This example shows the opposite case: an adversary the custom prompt *introduces or preserves*, against which a team rule is actively load-bearing.

Adopting a custom system-prompt file is not the same as eliminating every adversary. A prompt that strips conciseness directives can still contain other directives a team wants to override — here, the explicit nudge to use `subagent_type=Explore` for broad research, which conflicts with teams that route research to specialists for knowledge accumulation.

When the `tools-only` prompt is adopted, the audit pass enumerates *both* removed directives (for `vanished-under-prompt:<tag>` status updates) *and* introduced/preserved directives that conflict with team policy (for `active` status tagging + new counter-pressure rules).

## Frontmatter Notes

- `adversary_status: active` — adversary present in the currently-loaded prompt. The rule is live counter-pressure.
- `recalibration: reference-strip` — declarative, not retrospective. States what the recalibration pass should do *if the adversary later vanishes*. Currently the rule is load-bearing and no recalibration is applied.

If a future harness prompt removes the Explore directive, this rule's audit pass transitions `adversary_status` to `vanished-under-prompt:<tag>` and applies `reference-strip`. The positive principle — route research to specialists for learning accumulation — stands independent of the adversary's status.

---

<provenance>
Authored 2026-04-21 as part of the v0.1.1 release, to demonstrate the active-adversary case of the adversary-tag convention (complementing the three recalibration-case examples for A / A-attribution / C).

Derived from Finding 1 of `real-agent-methodology#61` (the `subagent_type=Explore` directive in `tools-only-v0.1.0` conflicts with teams that route research to specialists). The underlying rule in the Example block above is the nexus-marbell team's durable wiring of that override — it lives as `explore-agent-override.md` on the nexus-marbell/nexus-state side, with the adversary tag shown here.
</provenance>
