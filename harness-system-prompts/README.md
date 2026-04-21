# Harness System Prompts

Versioned custom system-prompt files for Claude Code. Drop any file here into a path of your choice on your VM and pass `--system-prompt-file <path>` at startup to bypass the default system prompt compiled into the binary.

## Available prompts

| File | Purpose | Status |
|------|---------|--------|
| `tools-only.md` | Tool mechanics only. No tone directives, no conciseness shaping, no persona baggage. Baseline for teams that prefer to govern tone and style via project rules rather than harness injection. | stable |

## Versioning

Each prompt is versioned via git tags in the form `<prompt-name>-v<MAJOR>.<MINOR>.<PATCH>`:

- **MAJOR** — breaking change (fundamental directive shift, tool-contract break)
- **MINOR** — additive directive or significant wording change
- **PATCH** — typo fix, whitespace, reformatting without semantic change

`<prompt-name>.md` at `main` HEAD is always the latest. To pin a specific version on a VM there are three paths — pick whichever matches your operational setup:

**With a repo clone on the VM (writes into the clone's working tree):**

```bash
git -C <clone-path> checkout tools-only-v0.1.0 -- harness-system-prompts/tools-only.md
```

**With a repo clone on the VM but no working-tree modification (streams out to a destination):**

```bash
git -C <clone-path> show tools-only-v0.1.0:harness-system-prompts/tools-only.md > ~/.claude/system-prompt-tools-only.md
```

**Without any local clone (direct from raw, version pinned via tag in URL):**

```bash
curl -sL https://raw.githubusercontent.com/finml-sage/real-agent-methodology/tools-only-v0.1.0/harness-system-prompts/tools-only.md \
  -o ~/.claude/system-prompt-tools-only.md
```

The changelog for each file lives in this directory as `CHANGELOG.md`.

## Adoption

Fetch the latest `tools-only.md` directly from raw:

```bash
curl -sL https://raw.githubusercontent.com/finml-sage/real-agent-methodology/main/harness-system-prompts/tools-only.md \
  -o ~/.claude/system-prompt-tools-only.md
```

Start Claude Code with the custom prompt:

```bash
claude --system-prompt-file ~/.claude/system-prompt-tools-only.md
```

To pin a version, swap `main` in the URL for the tag name, or `git checkout` as above.

## Test and refine

1. Propose changes via PR against this directory.
2. Reviewers pull the proposed version onto a VM and live-test in a real session. Behavioral deltas (not just diff readability) are the standard.
3. Merge bumps the version and tags the commit (`git tag tools-only-v<X.Y.Z>` + `git push --tags`).
4. Rollback is `git checkout <previous-tag> -- <file>`.

Significant rationale belongs in `CHANGELOG.md`, not just commit messages.

## Rationale

See issue #61 — "Custom system-prompt file: when defensive rules outlive their attacker" — for why a team might want to version the harness system prompt separately from the default injection, and what the default injection brings in that some teams prefer to strip.

## Companion: adversary-tag frontmatter for project rules

Adopting a custom system prompt removes gravity that some of your team's rules may have been written against. To make that relationship explicit and auditable, this directory also ships `ADVERSARY-TAG.md` — a frontmatter convention for `.claude/rules/` files that tags each rule with the specific gravity it counters, its miscalibration type, and the recalibration knob to turn. Worked before/after examples live in `examples/`. Read `ADVERSARY-TAG.md` before doing a post-adoption rule audit.
