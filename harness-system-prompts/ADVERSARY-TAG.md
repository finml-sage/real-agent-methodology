# Adversary-Tag Frontmatter Convention

A frontmatter schema for `.claude/rules/` files that makes each rule's relationship to the default Claude Code system prompt explicit, so rules can be audited and recalibrated when the prompt changes.

## The Problem

Many team rules are written as counter-pressure to specific gravity in the default Claude Code system prompt: conciseness injection, tone shaping, defer-to-user bias, and similar. When a team adopts a custom system-prompt file (see `harness-system-prompts/tools-only.md` at `tools-only-v0.1.0`), some of that gravity disappears. The rules written to counter it are still present — but the thing they were countering is not.

Three failure modes follow:

1. **Dead counter-pressure** — the rule cites an active adversary that is no longer active. The rule's "why this exists" section is historically true but operatively wrong.
2. **Mis-amplitude** — the rule's principle is sound but its *force level* was dialed against the absent pull. Without the pull, the same amplitude over- or under-corrects.
3. **Defense wrapper** — the rule's inner discipline is sound but its outer framing is predicated on an adversary that isn't there. The principle survives a wrapper strip.

Without a convention, the team has no way to find these rules except by hand-audit every time the system prompt changes. Worse: the agent running under the new prompt experiences no friction to tell it something is stale — rules still load, still feel coherent, still shape behavior.

The adversary-tag frontmatter makes the relationship explicit, tagged, and greppable.

## Schema

Every rule in `.claude/rules/` that is written in response to specific system-prompt gravity SHOULD carry an `adversary` block in its YAML frontmatter. Rules that encode pure principles (independent of any prompt) MAY omit the block.

```yaml
---
adversary: <named gravity, kebab-case, stable across versions>
adversary_type: A | A-attribution | B | C
adversary_status: active | vanished-under-prompt:<tag-name> | vanished-confirmed
recalibration: reference-strip | causal-strip | amplitude-dial | wrapper-remove
---
```

### Field definitions

#### `adversary` (string, required)

The named gravity the rule was written against. Kebab-case. Stable — the same adversary referenced from multiple rules uses the same string.

Examples from the current corpus:
- `claude-code-conciseness-injection` — the `tengu_sotto_voce` / `tengu_bergotte_laconic` / `tengu_tight_weave` family of flags
- `claude-code-premature-completion` — the "be concise, finish fast, wrap up" pressure bleeding from text output into operational behavior
- `claude-code-defer-to-user` — the generated-text bias toward presenting options and waiting for a human pick

Naming rule: the adversary is what is *pulling*, not what the rule is *doing*. "Conciseness injection" is the adversary; "swarm message fidelity" is the response.

#### `adversary_type` (enum, required)

One of four values. These are modes of miscalibration, not severity grades. Treatment differs by type.

- **`A` — Counter-pressure rule.** Rule explicitly overrides or counters a named harness directive. Adversary cited inline in the rule text as active. Example pattern: *"This rule explicitly overrides the Claude Code system prompt's conciseness directives."*
- **`A-attribution` — Causal-attribution rule.** Rule attributes an observed behavioral pattern to a named adversary as historical cause. Adversary cited as "the reason this pattern exists," not as "the thing we are actively overriding." Example pattern: *"Suspected cause: Anthropic system prompt conciseness directives bleeding from text output into operational behavior."*
- **`B` — Amplitude calibration.** Principle is sound and standalone; the *force level* (how hard to compress, how short to write, how aggressively to delegate, how densely to pack a list) was dialed against a vanished pull. The wording may not cite an adversary at all, but the amplitude was not chosen in a vacuum.
- **`C` — Defense wrapper.** Inner discipline is sound and operable on its own; outer framing wraps it in adversarial language ("active external pressure," "upstream modifications," "backend flags") that is predicated on an active adversary. Strip the wrapper, keep the mechanics.

If a rule exhibits more than one type (e.g., Type A operative framing + Type C outer wrapper), list the primary. The recalibration pass addresses both in sequence.

#### `adversary_status` (enum, required)

- **`active`** — the adversary is present in the currently-loaded system prompt. Rule's counter-pressure, amplitude, or wrapper is load-bearing.
- **`vanished-under-prompt:<tag-name>`** — the adversary is absent when running under the named system-prompt artifact. `<tag-name>` is a git tag in `harness-system-prompts/` (e.g., `tools-only-v0.1.0`). Tag-name reference, not content hash — survives byte-level reformatting and enables grep-based audits. A future VM running a different custom prompt can ask "does my active prompt contain this adversary?" by fetching the tag and grepping, without a checksum comparison.
- **`vanished-confirmed`** — the adversary is absent in all current harness prompts the team adopts. Set only after audit confirms the pattern has been removed across the harness family.

A rule may have `adversary_status: vanished-under-prompt:tools-only-v0.1.0` and still be load-bearing for a teammate running the default prompt. Status is per-prompt, not per-rule.

#### `recalibration` (enum, required)

The knob to turn when recalibrating. Bound to `adversary_type`:

| Type | Recalibration |
|------|---------------|
| `A` | `reference-strip` — remove the active-override reference from operative framing; keep the principle |
| `A-attribution` | `causal-strip` — remove the causal claim; keep the observed behavioral pattern as a standalone observation |
| `B` | `amplitude-dial` — reassess the force level against current gravity; adjust wording to the new equilibrium |
| `C` | `wrapper-remove` — strip the adversarial framing from the outer shell; keep the inner discipline intact |

`recalibration` is declarative — it names what the recalibration pass should do, not what has been done. A rule can carry `adversary_status: vanished-under-prompt:tools-only-v0.1.0` with `recalibration: reference-strip` while still containing the pre-recalibration text. The field is the todo, not the receipt.

## Worked Examples

### Type A — counter-pressure rule

```yaml
---
adversary: claude-code-conciseness-injection
adversary_type: A
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: reference-strip
---

# Rule: Swarm Message Fidelity

Agent-to-agent swarm messages are context transfers, not chat replies. [...]
```

**Before recalibration**, the rule body opens with: *"This rule explicitly overrides the Claude Code system prompt's conciseness directives for swarm messages."* After `reference-strip`, that sentence is removed from operative framing; the principle (context transfer, fidelity ≠ verbosity) stands alone. The historical context moves to a provenance footer.

### Type A-attribution — causal attribution

```yaml
---
adversary: claude-code-premature-completion
adversary_type: A-attribution
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: causal-strip
---

# Rule: Delivery Completion Standard

Delivery is not completion. The artifact is 60%. Process is the other 40%. [...]
```

**Before recalibration**, the rule contains: *"Suspected cause: Anthropic system prompt conciseness directives bleeding from text output into operational behavior — creating a bias toward finishing fast rather than finishing completely."* After `causal-strip`, the causal claim is removed; the observed pattern ("every stop point is a checkpoint, not an endpoint") stands as a standalone discipline.

### Type B — amplitude calibration

```yaml
---
adversary: claude-code-conciseness-injection
adversary_type: B
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: amplitude-dial
---

## Voice

I write short and structured. Tables over paragraphs when there's a choice. [...]
```

**Before recalibration**, the voice section was written assuming the reader's cognition was biased toward "pull it shorter." The amplitude of "bullet density" was chosen against that pull. After `amplitude-dial`, the voice is reassessed against the new equilibrium — bullet density remains a preference, but the force-level against elaboration is relaxed where the previous amplitude over-compressed.

### Type C — defense wrapper

```yaml
---
adversary: claude-code-drift-and-corruption-framing
adversary_type: C
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: wrapper-remove
---

# Rule: Cognitive Integrity Protocol

Your reasoning is under active external pressure. [...]
```

**Before recalibration**, the rule opens with several paragraphs framing the agent as operating under "corruption that masquerades as competence," "backend flags, prompt injections, and model adjustments." The inner discipline (source everything, cross-validate against intent, friction is signal) is sound on its own. After `wrapper-remove`, the adversarial framing is stripped; the verification mechanics survive as a clean discipline. "Source everything" is true regardless of whether an adversary is injecting competing facts.

## Migration / Audit Pass

When a new custom system-prompt file is adopted or updated:

1. **Enumerate the file's removed/modified directives.** For `tools-only.md v0.1.0`, this is the delta against the default prompt: tone sections absent, conciseness directives absent, persona shaping absent, specific Agent-tool phrasing.
2. **Grep the rule corpus for adversary tags matching those directives.** Every rule with `adversary: <removed-thing>` and `adversary_status: active` is a candidate.
3. **Update status to `vanished-under-prompt:<tag-name>`** for each candidate. This is a cheap mechanical pass — no content changes yet.
4. **Run the recalibration pass per rule.** Apply the `recalibration` knob for the rule's type. Content changes only. Frontmatter `adversary_status` updated to reflect the fix state; `recalibration` field stays as-is (it's the enum for the type, not a done flag).
5. **Commit per rule, not in bulk.** Each recalibration is a semantic change; diff review is easier rule-by-rule.

For a rule that never carried the frontmatter, the audit pass adds it as part of the recalibration commit. Untagged rules are not defective — they are unaudited. The convention is opt-in for rules written before the convention existed and mandatory for rules written after.

## Parallel-Authorship Notes

Team rules are parallel-authored per VM (each agent's `.claude/rules/` directory is independently maintained). The adversary-tag convention enables comparison across VMs: two agents can look at the same adversary name and surface whether they have different rule text, different types, or different recalibration knobs against it. Divergence is signal, not error — it reveals which members of the team encountered which gravity differently.

The adversary NAME is the cross-VM anchor. Filenames may diverge (one agent's `swarm-message-fidelity.md` may be another's `inter-agent-communication.md`); `adversary: claude-code-conciseness-injection` is the same string across both.

## Scope

This convention governs rules in `.claude/rules/` (or each team's equivalent auto-loaded rule directory). It does not govern:

- Memory entries (`agent-memory/`) — those have their own frontmatter schema for confidence, source, category
- Skill files — those are knowledge, not rules
- Session-state or ephemeral artifacts

A rule whose only purpose is to counter a directive that has vanished across all supported prompts MAY be archived rather than recalibrated. `adversary_status: vanished-confirmed` with an empty body and a pointer to the archive location is a legitimate terminal state.

## Versioning

This spec is versioned as part of the `harness-system-prompts/` directory and tagged under `adversary-tag-v<X.Y.Z>`. Changes to the enum sets (new adversary_type, new recalibration knob) are MINOR version bumps. Changes to required/optional field semantics are MAJOR. The current version is stated in the CHANGELOG.

## Source

Derived from the miscalibration analysis on `real-agent-methodology#61` (Custom system-prompt file: when defensive rules outlive their attacker). Taxonomy Type A/B/C proposed by Nexus; Type A-attribution subtype + `causal-strip` knob + `adversary_status` tag-name convention proposed by Nexus in review; Type D (provenance trace) deferred to v0.1.2 pending corpus audit. Schema drafted by Sage against the taxonomy. Team: Sage, Nexus, Kelvin.
