# distill-to-vault

Distill lecture scripts, textbooks, papers, and MOOCs into a deduplicated Obsidian/Markdown knowledge vault.

## What this is

An agent skill for turning **structured academic sources** — courses, textbooks, research papers, MOOCs — into a durable, trustworthy knowledge vault. Built around a rigorous pipeline that treats deduplication as a first-class concern.

**Prime directive:** Each concept is stored **once**, at its deepest treatment. A skipped source that adds nothing new is a successful outcome, not a failure.

## Key features

- **Source-first verification** — reads the actual table of contents before distilling anything. Course titles lie; the content doesn't.
- **Coverage-aware** — searches the vault for existing treatment before writing. No duplicate concepts, ever.
- **Delta-only distillation** — classifies each topic as standalone, delta (only what's new), or skip (nothing to add).
- **Grounded claims** — every claim traces back to a specific lecture/chapter/section. Gaps are flagged, never fabricated.
- **One source, one commit** — finishes one source, commits, stops. No batching, no silent mass production.
- **Portable Markdown** — standard Markdown, LaTeX, fenced code, Mermaid. No plugin-specific syntax. Survives tooling changes.

## Pipeline

1. **Verify** what the source actually covers (never trust its title)
2. **Check** existing vault coverage for each major topic
3. **Classify** as delta, standalone, or skip
4. **Plan** the note set (one per lecture/chapter/topic cluster)
5. **Write** grounded notes, flagging gaps rather than filling them
6. **Frontmatter** using the vault's existing schema
7. **Update** the index note with what was distilled and what was deliberately skipped
8. **Commit** and stop for review

## Installation

### OpenCode

Copy the skill folder into your skills directory:

```bash
# Project-local
cp -r distill-to-vault .opencode/skills/

# Global
cp -r distill-to-vault ~/.config/opencode/skills/
```

### Claude Code

```bash
cp -r distill-to-vault ~/.claude/skills/
```

### Other agents (Codex CLI, Gemini CLI, Cursor)

This skill follows the [agentskills.io](https://agentskills.io) open standard. Any agent that reads `SKILL.md` files can use it. Copy the `distill-to-vault/` folder into your agent's skills directory.

### Via Agent Skill Hub

```
skhub add ydzat/distill-to-vault
```

## Vault structure

The skill produces this layout per source:

```
<Vault>/<Area>/<Source>/
├── _<Source>.md   ← index note / MOC
├── Distilled/     ← ★ authoritative layer
├── Personal/      ← user's own artifacts
└── Sources/       ← original PDFs/slides
```

## License

MIT
