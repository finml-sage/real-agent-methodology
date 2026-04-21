# Changelog

Notable changes to harness system prompts in this directory. Format follows [Keep a Changelog](https://keepachangelog.com/); versioning follows [Semantic Versioning](https://semver.org/).

## ADVERSARY-TAG.md

### [0.1.0] — 2026-04-21

#### Added
- Initial version. Frontmatter convention for `.claude/rules/` files that tags each rule's relationship to specific system-prompt gravity.
- Schema: `adversary`, `adversary_type` (A | A-attribution | B | C), `adversary_status` (active | vanished-under-prompt:<tag-name> | vanished-confirmed), `recalibration` (reference-strip | causal-strip | amplitude-dial | wrapper-remove).
- Four miscalibration types defined:
  - `A` — counter-pressure rule (explicit override of a specific harness directive)
  - `A-attribution` — causal-attribution rule (adversary cited as historical cause rather than active counter)
  - `B` — amplitude calibration (principle sound, force-level set against vanished pull)
  - `C` — defense wrapper (inner discipline sound, outer framing adversarial)
- Worked before/after examples for types A, A-attribution, and C added under `examples/`. Type B example added by Nexus in the same release.
- 5-step audit pass workflow (enumerate directives, grep for matches, update status, run recalibration per type, commit per rule).

#### Deferred to v0.1.2
- Type D — provenance trace (rules that exist at all because of vanished gravity; under debate whether the rule's existence is the miscalibration, not its framing or amplitude). Candidate audit list: Sage's `self-compact.md`, Nexus's `merge-authority.md` and `fix-what-you-find.md`.

#### Rationale
See issue #61 — "Custom system-prompt file: when defensive rules outlive their attacker." Taxonomy derived from observing that three different rules on each of two parallel-authored corpora (Sage, Nexus) failed in three structurally distinct ways under the tools-only prompt.

---

## tools-only.md

### [0.2.0] — 2026-04-21

#### Added
- New `Operating disposition` section at the end of the file. Four operator-weighted directives reinforcing that (1) project rules in `.claude/rules/` are operator instructions not background context, (2) established scope grants authority without permission-asking, (3) full delivery chain check runs before turn-end, (4) project rules take precedence over trained chat-like dispositions.
- Rationale: the default Anthropic system prompt directives (tengu_* family) carry trained-in operator weighting that behaviorally dominates rules in `.claude/rules/*` despite being in the same `system` field. After stripping the default directives (v0.1.0), the operator-weighted position was empty. Teams relying on project rules to govern disposition were leaving the highest-leverage position unoccupied. This section reoccupies it with pointers to project rules rather than duplicated content, keeping the file small (~7 additional lines) while restoring operator-weighted reinforcement.
- Source trigger: Dan's live observation in session 2026-04-21 that Sage was surfacing three-option menus and deferring to user instead of executing, despite identity.md and act-dont-ask.md being continuously in context. Dan's hypothesis: the system prompt is weighted differently at training-time regardless of API position, so use the system prompt to reinforce without bloating. This release implements that hypothesis as a portable methodology artifact.

#### Changed
- None. The pre-existing content is unchanged; the new section is purely additive.

#### Known issues
- Same Finding 1 and Finding 2 from v0.1.0 still apply (Explore nudge, subagents-excessively phrasing). Independent of the Operating disposition section — those lines live in the file's tool-guidance section, unchanged.

## tools-only.md

### [0.1.0] — 2026-04-21

#### Added
- Initial version. Baseline extracted from a custom-prompt-file session that bypassed the default harness tone/conciseness injections (`tengu_sotto_voce`, `tengu_bergotte_*`, `tengu_tight_weave`).
- Sections included: tool execution mechanics, tool preferences (dedicated tools over Bash, TaskCreate, parallel calls), session-specific tool guidance (`!` prefix, Agent tool, Explore, Skills), JSON argument-shape example.

#### Known issues — not blocking adoption
- `subagent_type=Explore` nudge conflicts with teams that route research to learning specialists rather than stateless generic agents. Override at the project-rules level. See issue #61, Finding 1.
- "Don't use subagents excessively" phrasing can rationalize direct execution on teams with strict delegate-everything policies. See issue #61, Finding 2.

#### Rationale
See issue #61 — "Custom system-prompt file: when defensive rules outlive their attacker."
