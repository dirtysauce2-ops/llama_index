# Claude Code to Codex transition record

## Baseline (2026-11-13)

Target: `dirtysauce2-ops/llama_index`, default branch `main`. An inspection of the GitHub repository tree at commit `444fdf0684707de62ebb447d658dba137dd9cb99` found **no tracked** `AGENTS.md`, `CLAUDE.md`, `.agents/`, `.claude/`, or `.codex/` at that commit. This is an inventory of the remote repository only. Local, ignored, untracked, and user-global artifacts have **not** been inspected.

This branch bootstraps shared agent guidance with `AGENTS.md`, a Claude import via `CLAUDE.md`, and a Codex transition skill in `.agents/skills/`. It does **not** convert any missing Claude skills, migrate session history, or alter Codex/Claude user profiles.

## Why this is incremental

The existing monorepo uses `uv` and `pytest`, with pre-commit checks described in `CONTRIBUTING.md`. A safe transition should not replace the project build workflow or introduce external hooks and session-state mutations without review.

Third-party tools reviewed:

1. [ccode-to-codex](https://github.com/zuharz/ccode-to-codex): experimental skill and agent conversion, with preview/dry-run and validators; requires Python 3.11+ and PyYAML. Its documented output locations include `.codex/skills/` and `.codex/agents/`.
2. [sessionbridge](https://github.com/tongtongtju/sessionbridge): Claude/Codex session import and text-summary handoff; requires Node.js 18+. The default Claude-to-Codex import writes JSONL and updates the Codex SQLite session index. Prefer `cl2cx <session-id> --inject` after reviewing the tool.
3. [Harness Hub](https://github.com/JasonxzWen/harness-hub): selective read-only Guided mode and separately authorized Managed mode. Its Codex skills use `.agents/skills/`. Avoid auto-merging its rules or hooks.

These are upstream project claims and inspected source behaviors, not proof of compatibility with any locally installed version.

## Review gates

1. **Inventory locally:** list source `.claude/skills/`, `.claude/agents/`, `.claude/commands/`, project settings and hooks; also inspect relevant `~/.claude/` entries only with appropriate authorization.
2. **Snapshot:** capture `git status --short` and back up locally maintained session/config files before any writing mode. Keep local secrets and private sessions out of Git.
3. **Convert in preview:** choose a real source item, run an upstream preview/dry-run, and inspect the exact output. Do not call nonexistent CLI scripts; install tooling only when deliberately selected.
4. **Reconcile paths:** compare `.agents/skills/` with any generator-created `.codex/skills/`; never blindly overwrite or duplicate a skill name.
5. **Review hooks/MCP:** compare tool namespace mapping, matching conditions and permissions. Never automatically enable converted hooks.
6. **Session handoff:** prefer text summary/import rather than direct SQLite modification until version compatibility has been independently tested.
7. **Validate:** check skill frontmatter, referenced paths, tool availability and final Git diff; run relevant project tests if implementation code changes.

## Current status

| Area | Status | Evidence / next action |
| --- | --- | --- |
| Shared agent guidance | Bootstrapped on transition branch | Review `AGENTS.md` and `CLAUDE.md` |
| Codex migration skill | Bootstrapped on transition branch | Review `.agents/skills/claude-to-codex-transition/SKILL.md` |
| Claude skill conversion | Not started | No tracked source skills; local inventory required |
| Claude agent conversion | Not started | No tracked source agents; local inventory required |
| Hooks and MCP configuration | Not changed | Local configuration must be assessed |
| Session history | Not changed | No session records accessible via GitHub |
| End-to-end Codex verification | Pending | Requires a compatible local Codex CLI and real source items |

## Project validation references

Follow `CONTRIBUTING.md` for `uv sync` and `uv run make lint`. For affected packages, execute `uv run -- pytest` from the corresponding package directory. Changes in this bootstrap are instruction and documentation files; no Python code or dependencies are modified.
