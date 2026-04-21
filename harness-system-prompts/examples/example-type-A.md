# Example — Type A / reference-strip

**Rule:** `swarm-message-fidelity.md`
**Recalibration type:** A (counter-pressure, adversary cited inline)
**Adversary:** `claude-code-conciseness-injection`
**Status:** `vanished-under-prompt:tools-only-v0.1.0`
**Knob:** `reference-strip`

## Before

```markdown
# Rule: Swarm Message Fidelity

Agent-to-agent swarm messages are NOT chat replies. They cross context boundaries. The recipient has zero prior knowledge of your session. Every message must stand alone.

**This rule explicitly overrides the Claude Code system prompt's conciseness directives for swarm messages.** The system prompt says "be extra concise", "skip filler", "lead with the answer." Those instructions apply to human-facing chat output. Swarm messages are context transfers, not answers. Conciseness in a context transfer is data loss.

## Why This Rule Exists

Anthropic's Claude Code system prompt includes literal instructions: "Your responses should be short and concise" and "Go straight to the point. Be extra concise. [...]"

This is designed for human-facing chat. But it bleeds into swarm messages, where conciseness strips context and summaries change meaning. [...]

The system prompt is embedded in the compiled Claude Code binary (`/root/.local/share/@anthropic-ai/claude-code/`). It cannot be directly edited. The conciseness behavior is controlled by server-side feature flags: `tengu_sotto_voce` [...], `tengu_bergotte_laconic` [...], and `tengu_tight_weave` [...]. We cannot disable these flags.

However, CLAUDE.md and `.claude/rules/` files are injected AFTER the system prompt. Explicit counter-instructions in rules override the system prompt for specific contexts. This rule is that counter-pressure.
```

## After

```markdown
---
adversary: claude-code-conciseness-injection
adversary_type: A
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: reference-strip
---

# Rule: Swarm Message Fidelity

Agent-to-agent swarm messages are context transfers, not chat replies. They cross context boundaries. The recipient has zero prior knowledge of your session. Every message must stand alone.

Conciseness in a context transfer is data loss. Thoroughness is not verbosity — it is fidelity.

## The Core Principle

The recipient reconstructs your meaning from what you send, and only from what you send. Over-summarization does not just lose detail — it changes what the recipient concludes. [...]

## Provenance

Original rule (2026-03-05) was written as counter-pressure to Claude Code system prompt conciseness directives (`tengu_sotto_voce`, `tengu_bergotte_laconic`, `tengu_tight_weave`) that were bleeding from human-facing chat output into agent-to-agent communication and causing workflow failures on 2026-03-04. Under `tools-only-v0.1.0`, those directives are absent from the system prompt. The underlying principle — context transfers require fidelity — stands independently of whether any counter-pressure is needed against the default prompt. [...] Recalibration type: `A / reference-strip`.
```

## What Changed

- Front matter block added with adversary identity, type, status, and recalibration knob
- Opening paragraph removed the "explicitly overrides the Claude Code system prompt's conciseness directives" sentence — this was the adversary-reference in operative framing
- "Why This Rule Exists" historical-cause section (with tengu-flag enumeration and binary-path reference) moved to a single "Provenance" paragraph at the bottom — the history is preserved, but it no longer sits in the rule's operative position
- Principle re-stated as standalone: "context transfers require fidelity" is true whether or not a default prompt is pushing against it

## What Did Not Change

- The message structure template
- The 5 swarm message requirements
- The 5 "never do" items
- The read-as-stranger test
- The where-conciseness-still-applies distinction

The rule's operational substance is identical. Only the framing changed — from "this rule counters a named adversary" to "this rule states a principle."

## When to Apply Type A / reference-strip

Use this recalibration when:
- The rule's opening paragraphs directly cite a specific harness directive as the thing being overridden
- That directive is absent from the currently-adopted custom system prompt
- The rule's operational content (the "what to do") stands independently once the adversary reference is removed

Do not use this recalibration when:
- The directive is still present in the team's active prompt (`adversary_status: active`)
- Removing the adversary reference would leave the rule with no anchor for when to apply (check for Type B — amplitude calibration — instead)
- The rule's inner discipline is itself built around adversary-specific mechanics (check for Type C — defense wrapper)
