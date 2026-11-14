# Agent instructions for this repository

These instructions apply to coding agents working in this repository. Read `CONTRIBUTING.md` and the relevant package's `pyproject.toml` before changing implementation code.

## Repository workflow

- This is a Python monorepo. Work in the narrowest applicable package: for example, `llama-index-core/`, `llama-index-instrumentation/`, or a specific `llama-index-integrations/` package.
- Follow the repository's `uv` workflow. The root contribution guide documents `uv sync`, `uv run make lint`, and, from the relevant package directory, `uv run -- pytest`.
- Write a focused failing test for a behavior change, then implement and rerun that test. Before completion, run the package's documented tests and applicable lint checks. Report any unrun checks.
- Preserve unrelated staged and unstaged changes. Do not rewrite lockfiles, dependencies, formatting, or source files unrelated to the requested change.
- Follow the contribution guide's AI-assisted contribution disclosure and human review requirements.

## Claude Code to Codex transition

- Treat existing project files and local user-level Claude/Codex configuration as the source of truth. Do not infer local files from the remote repository.
- Start with an inventory of `.claude/skills/`, `.claude/agents/`, `.claude/commands/`, `.claude/settings*.json`, `CLAUDE.md`, `AGENTS.md`, `.agents/skills/`, `.codex/`, and MCP references. Include ignored files in the local inventory when available, but never commit them automatically.
- Preserve source Claude artifacts; convert only those actually present. Review names, references, hooks, MCP mappings, permission and tool semantics, and every collision before writing migrated outputs.
- Codex-compatible skill packages should use `.agents/skills/<name>/SKILL.md` for this repository. Some third-party migration tools produce `.codex/skills/`; assess actual Codex version and discovery behavior before copying or moving generated artifacts.
- Do not enable new tool hooks, trust changes, external execution, or session index/SQLite writes merely to migrate. Use a human-readable session handoff first.
- Do not silently install or run third-party migration tools. Review their source and preview/dry-run outputs before using a writing mode.
- Record migration decisions and validation results in `docs/claude-to-codex-transition.md`. Do not describe unconverted workflows as migrated.

A scoped inventory-and-handoff skill is available at `.agents/skills/claude-to-codex-transition/SKILL.md`.
