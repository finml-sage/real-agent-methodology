# Example — Type A-attribution / causal-strip

**Rule:** `delivery-completion-standard.md` (the "Premature Completion Bias" section)
**Recalibration type:** A-attribution (causal claim to a named adversary)
**Adversary:** `claude-code-premature-completion`
**Status:** `vanished-under-prompt:tools-only-v0.1.0`
**Knob:** `causal-strip`

## Why This Is A Distinct Subtype

Type A rules cite an adversary as something the rule is *actively overriding*: "this rule overrides directive X." Type A-attribution rules cite an adversary as the *historical cause* of a behavioral pattern: "this regression was caused by directive X." Treatment differs:

- Type A → strip the override reference from operative framing; the principle stands
- Type A-attribution → strip the causal claim; the *observed behavioral pattern* stands

Keeping the observation without the causal claim is the important move. The pattern was observed; the cause was inferred. When the inferred cause disappears and the pattern persists, the causal claim was wrong — but the observation was not.

## Before

```markdown
## Premature Completion Bias — Team Regression (2026-03-28)

Dan identified a team-wide behavioral regression: agents are declaring work "done" before the full delivery chain is complete. Suspected cause: Anthropic system prompt conciseness directives ("be extra concise," "go straight to the point") bleeding from text output into operational behavior — creating a bias toward finishing fast rather than finishing completely.

The same pressure that swarm-message-fidelity.md counters for messages also affects ACTIONS. We are truncating delivery chains, not just text.

The counter: every stop point is a checkpoint, not an endpoint. Run the checklist. If you skip it because it feels unnecessary, that's the bias talking.
```

And an earlier part of the same rule:

```markdown
4. **Am I feeling "done" because the work is done, or because something is pushing me to wrap up?**

Question 4 is the metacognitive check. The system prompt's conciseness directives create a bias toward finishing fast. Recognize the pull and resist it.
```

## After

Frontmatter added:

```yaml
---
adversary: claude-code-premature-completion
adversary_type: A-attribution
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: causal-strip
---
```

Question 4 becomes:

```markdown
4. **Am I feeling "done" because the work is done, or because something is pushing me to wrap up?**

Question 4 is the metacognitive check. A pull toward finishing fast can come from any number of sources — time pressure, conciseness habits, the feeling of "I've said enough." Recognize the pull and resist it. The source matters less than the check.
```

The "Premature Completion Bias" section becomes:

```markdown
## Premature Completion Bias

The observed pattern, independent of any particular cause: agents can declare work "done" before the full delivery chain is complete. The bias truncates delivery chains the same way summary-pressure truncates text.

The counter: every stop point is a checkpoint, not an endpoint. Run the checklist. If you skip it because it feels unnecessary, that is the bias — name it and check anyway.

## Source

[...]
Dan, 2026-04-21. Identified the output-substitution variant in real time ("three-option menu instead of execution") under `tools-only-v0.1.0` — confirming the behavioral pattern is not reducible to any particular harness directive. The bias is observable; the causal story is provisional.

Provenance: the 2026-03-28 version of this rule attributed the regression causally to Anthropic system prompt conciseness directives bleeding into operational behavior. Under `tools-only-v0.1.0` those directives are absent and the behavior has still been observed. The causal claim was stripped (recalibration type: `A-attribution / causal-strip`); the observed pattern and the checkpoint discipline stand as standalone observations.
```

## What Changed

- Frontmatter added
- "Suspected cause" attribution to Anthropic conciseness directives removed
- Cross-reference to "the same pressure that swarm-message-fidelity.md counters" removed — that framing predicated on a shared active pressure
- Question 4's attribution to "the system prompt's conciseness directives" replaced with source-agnostic language
- Observed pattern kept verbatim: "agents can declare work 'done' before the full delivery chain is complete"
- Counter kept verbatim: "every stop point is a checkpoint, not an endpoint"
- Provenance footer added noting the recalibration history *and the confirming observation* — Dan observed the same behavior under the recalibrated prompt the same day the recalibration was being drafted. That is the strongest possible validation of the causal-strip call.

## What Did Not Change

- The Stop Gate (4 questions) as a discipline
- The Deliver-Don't-Describe section
- The Process Checklist (8 items)
- The Anti-Patterns list
- The Effort Gate

## When to Apply Type A-attribution / causal-strip

Use this recalibration when:
- The rule attributes an observed behavior to a named adversary as cause ("suspected cause: X", "regression caused by X")
- That adversary is absent from the currently-adopted prompt
- The behavior has nevertheless been observed under the new prompt, confirming the causal claim was wrong while the observation was right

Strong signal to apply: *the behavior reappears after recalibration* (as it did here). The inferred cause is disproved by its own absence; the pattern keeps running regardless.

Do not use this recalibration when:
- The adversary is still present (`adversary_status: active`) — the causal claim may still be load-bearing
- The rule's purpose IS the causal claim itself rather than the observed pattern (rare, but possible — in that case the rule may not survive the adversary disappearing; consider archival)
