# Changelog

Notable changes to harness system prompts in this directory. Format follows [Keep a Changelog](https://keepachangelog.com/); versioning follows [Semantic Versioning](https://semver.org/).

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
