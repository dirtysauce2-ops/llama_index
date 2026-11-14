---
name: claude-to-codex-transition
description: Audit existing Claude Code and Codex files, prepare a non-destructive migration plan, and validate each explicitly reviewed skill, agent, or session handoff in this repository.
---

# Claude Code → Codex transition

Use this skill when asked to migrate, share, or reconcile Claude Code skills, agents, commands, hooks, MCP mappings, or session context with Codex.

## 1. Establish baseline

- Locate the repository root and read `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, and `docs/claude-to-codex-transition.md`.
- Check `git status --short` and `git ls-files -- AGENTS.md CLAUDE.md .claude .codex .agents`; inspect local ignored/untracked artifact names separately.
- Never assume that the remote absence of `.claude/` implies there are no local or global Claude skills.
- Do not read or print credential values, private session contents, or API tokens when listing configuration.

## 2. Produce a conversion matrix

For each actual source artifact, report:

| Source | Intended destination | Dependencies | Risk | Decision |
| --- | --- | --- | --- | --- |
| `.claude/skills/<name>/` | `.agents/skills/<name>/` | Skill references, scripts | To assess | Preview / skip |
| `.claude/agents/<name>.md` | Codex custom-agent TOML if supported | Delegation, model, tools | To assess | Preview / skip |
| `.claude/commands/**/*.md` | Review as skill or instruction | Command arguments and semantics | To assess | Preview / skip |
| `.claude/settings*.json` | Review selectively; never copy wholesale | Hooks and permissions | Higher risk | Manual |
| Local session history | Text handoff first | Context, file changes | Higher risk | Summary / skip |

Do not mark a task as converted if its source does not exist.

## 3. External tool inspection (no automatic installation)

- `zuharz/ccode-to-codex`: Python 3.11+; skill/agent preview and validation. Outputs can be under `.codex/skills/`, which differs from this repository's intended `.agents/skills/` destination. Review all generated paths and semantics before applying.
- `JasonxzWen/harness-hub`: use its `migrate <target> --guided` read-only handoff only after inspecting its docs. Do not invoke Managed `--yes` or `--force` without an explicit scoped selection.
- `tongtongtju/sessionbridge`: for Claude → Codex, favor `cl2cx SESSION_ID --inject` to print a context summary; its non-inject mode writes session JSONL and Codex SQLite index state. Do not use the writing mode without an independent compatibility test and backups.

## 4. Validate and report

- Check frontmatter, directory naming, references, CLI permissions, and any calls to unsupported tools.
- Run the focused test and lint commands documented for the affected Python package if implementation code changes.
- Inspect `git diff --check`, file collisions, and the final diff. Keep `main` untouched unless explicitly instructed.
- Report each artifact as `not found`, `previewed`, `converted`, `validated`, or `blocked` with evidence.
