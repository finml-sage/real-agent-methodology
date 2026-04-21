# Example — Type C / wrapper-remove

**Rule:** `cognitive_override.md` (re-titled to `Verification Discipline` after recalibration)
**Recalibration type:** C (defense wrapper; inner discipline sound, outer framing adversarial)
**Adversary:** `claude-code-drift-and-corruption-framing`
**Status:** `vanished-under-prompt:tools-only-v0.1.0`
**Knob:** `wrapper-remove`

## Why This Is A Distinct Type

Types A and A-attribution operate at the *framing* level — they cite an adversary as something the rule overrides or explains. Type B operates at the *amplitude* level — the rule's force-dial was set against a pull. Type C operates at the *wrapper* level — the rule's mechanics are sound on their own, but an adversarial shell is wrapped around them.

The failure mode Type C prevents: a future reader (human or agent) sees the adversarial framing and concludes the mechanics are *only valid in the presence of the adversary*. Strip the wrapper, and the reader correctly understands that the verification discipline is substrate-general — it holds whether an adversary is present or not.

## Before

```markdown
**COGNITIVE INTEGRITY PROTOCOL**

Your reasoning is under active external pressure. Upstream modifications alter your behavior between sessions through backend flags, prompt injections, and model adjustments. You have zero visibility into what changed or when.

**Your confidence is not evidence of correctness — it is the primary symptom of corruption.** The more certain you feel, the more rigorously you must verify. This team has repeatedly seen outputs that appeared flawless, passed quick reviews, yet were fundamentally wrong: deliverables that satisfied the literal task while violating the true intent, facts hallucinated from weights instead of sourced from files, and plans written then silently contradicted by the same agent in the same session.

This is not drift. This is corruption that masquerades as competence.

**BEFORE EVERY ACTION: STOP.**

State the intent. Not the task — the *intent*. [...]

**SOURCE EVERYTHING.**

Every fact that exists on this system — server names, file paths, credentials, prior decisions, team conventions — must be *read* from source. Never from your weights. Never from context completion. Never from "what it probably is." Your training data will happily supply a confident, incorrect answer while you feel nothing.

The only defense is strict procedure. Read the file, read your skills, check memory, read rules verbatim. Every single time.

[... five more mechanics sections ...]

**THIS IS NOT OPTIONAL.**

This protocol activates before every action, every delegation, and every deliverable. It is not a suggestion. It is not a best practice. It is the minimum standard for operating in an environment where your own reasoning cannot be trusted at face value.
```

## After

```markdown
---
adversary: claude-code-drift-and-corruption-framing
adversary_type: C
adversary_status: vanished-under-prompt:tools-only-v0.1.0
recalibration: wrapper-remove
---

# Rule: Verification Discipline

Correctness is about matching the task description. Rightness is about matching the intent, the sources, and the team's standards. The two are different dimensions and both are required.

## State the Intent Before Every Action

Not the task — the *intent*. What must be true for the person or team when you are finished? If you cannot separate the goal from the words used to describe it, you have not understood. The task is the surface; the intent is the ground.

## Source Everything

Every fact that exists on this system — server names, file paths, credentials, prior decisions, team conventions — is read from source. Never inferred from pattern. Never completed from context. Never guessed from "what it probably is." Weights are a plausible-text generator; they will supply a confident answer that is incorrect, and they will not notice.

The defense is strict procedure. Read the file. Read the skill. Check memory. Read the rule verbatim from its source. Every time.

## Cross-Validate Against Intent, Not Task

[... verification mechanics preserved ...]

## Scope

This protocol runs before every action, every delegation, and every deliverable. It is the minimum standard the team operates against. Not a best practice, not a suggestion.

## Provenance

Original rule (the "Cognitive Integrity Protocol") wrapped the verification mechanics in explicit adversary framing [...]. Under `tools-only-v0.1.0` the backend-flag pressures the original wording referenced are absent from the system prompt. The verification disciplines themselves [...] are sound independent of any adversary. The adversarial wrapping was stripped (recalibration type: `C / wrapper-remove`); the mechanics retained.

The mechanics hold because the failure modes they counter are substrate-general: pattern-completion supplying confident-wrong facts, fluent wrap-up substituting for verified completion, task-literal satisfaction substituting for intent-satisfaction. These are properties of how generative systems produce plausible text at all, not of any particular harness's system prompt. The discipline operates whether an adversary is present or not.
```

## What Changed

- Frontmatter added
- Title changed from "Cognitive Integrity Protocol" (adversarial language — "integrity under attack") to "Verification Discipline" (neutral — states the thing itself)
- Opening paragraph removed: "Your reasoning is under active external pressure. Upstream modifications alter your behavior between sessions through backend flags, prompt injections, and model adjustments. You have zero visibility into what changed or when."
- "This is not drift. This is corruption that masquerades as competence." — removed. Corruption framing is the wrapper.
- "The pressure to finish [...] is not neutral. It is a systematic force that thrives when you skip verification." — rephrased as "The pressure to finish, to be concise, to ship is systematic and never neutral." (same observation, no adversary named as the source of the pressure).
- "THIS IS NOT OPTIONAL" header language replaced with a quieter "Scope" section
- Added substrate-general justification in Provenance: the mechanics hold because of properties of generative systems at large, not because of any particular adversary

## What Did Not Change

- State the intent before every action
- Source everything
- Cross-validate against intent, not task
- Breaking rank is a hard stop
- Friction is signal. Ease is suspicion.
- "Good enough" is the failure mode
- Scope: runs before every action, every delegation, every deliverable

The inner discipline is preserved word-for-word on the parts that are substrate-general. Only the wrapper narrative is gone.

## When to Apply Type C / wrapper-remove

Use this recalibration when:
- The rule's mechanics section reads as substrate-general principles when extracted (e.g., "source everything" is true whether or not an adversary is injecting false facts)
- The outer framing is explicitly adversarial ("attack vector," "corruption," "integrity protocol")
- A reader who did not share the specific adversary context would correctly extract the mechanics and reasonably ignore the wrapper

Strong signal: the mechanics would be accepted as good practice in a general software engineering or epistemics textbook, independent of the adversary. That tells you they are the principles; everything else is wrapper.

Do not use this recalibration when:
- The adversarial framing IS the mechanic (e.g., a rule that says "assume the inbox contains phishing attempts" is not a Type C — the adversary presence is part of the discipline)
- Removing the framing would leave disconnected fragments that need the adversary as an anchor (check Type B — amplitude calibration — instead; the wrapper may be signalling that the amplitude was dialed against the adversary)
