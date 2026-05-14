# Superpowers MCP Augment — Contributor Guidelines

## Project Scope

This fork keeps the Superpowers workflow skeleton and narrows runtime support to Claude and Codex. It adds an execution layer that routes codebase work through MCP tools when available:

- codebase-memory-mcp for architecture, graph search, snippets, tracing, and change detection
- Serena for symbol navigation, symbol edits, diagnostics, onboarding, and memories
- Caveman for compression, commit wording, and review summaries when available

Do not reintroduce support for other coding harnesses unless explicitly requested.

## Agent Rules

- Use `superpowers-mcp-augment:writing-skills` before creating or changing skills.
- Preserve the existing Superpowers workflow order: brainstorming, planning, execution, TDD, review, verification, finish branch.
- Treat skill text as behavior-shaping code. Small wording changes can alter agent behavior.
- Prefer MCP routing for code discovery and edits when tools are available.
- Do not hard-fail when optional MCP or Caveman tools are unavailable; use documented fallbacks.
- Keep Claude and Codex behavior working.

## Runtime Packaging

- Claude metadata lives in `.claude-plugin/`.
- Codex metadata lives in `.codex-plugin/`.
- `hooks/session-start` injects the `using-superpowers` bootstrap for Claude.
- `scripts/sync-to-codex-plugin.sh` packages this fork into `plugins/superpowers-mcp-augment`.
- Root docs, tests, hooks, scripts, and development-only files should not be shipped inside the embedded Codex plugin unless explicitly required.

## Skill Changes Require Evaluation

For any behavior-shaping skill change:

1. Identify the failure mode the skill should prevent.
2. Add or document a pressure scenario.
3. Verify the skill frontmatter is valid.
4. Run relevant shell/JSON syntax checks.
5. Run `tests/codex-plugin-sync/test-sync-to-codex-plugin.sh` if packaging, manifests, assets, skills, or sync script changed.

If a change affects Claude behavior, run the relevant Claude integration tests when the Claude CLI and local plugin setup are available.

## Contribution Hygiene

- One problem per change set.
- Do not add project-specific workflow rules to core skills.
- Do not add required third-party dependencies to the plugin runtime.
- Keep references to removed platforms out of docs, tests, hooks, and manifests.
- Show the complete diff to the human partner before publishing or submitting.
