# Upstream Flow Alignment Design

**Date:** 2026-05-15

## Goal

Keep Superpowers MCP Augment aligned with the original Superpowers workflow while preserving the fork's Claude/Codex-only scope and MCP-aware execution layer.

## Failure Modes To Prevent

- Codex visual companion instructions copy a maintainer-specific absolute path, making the installed plugin fail on other machines.
- Future Codex packaging accidentally includes upstream runtimes this fork intentionally removed, such as Cursor, Gemini, OpenCode, or Copilot-only artifacts.
- Contributor templates refer to the upstream `superpowers:*` namespace instead of `superpowers-mcp-augment:*`.
- Plugin metadata points to the wrong repository URL casing/account, causing marketplace and support links to drift from the actual remote.
- Claude users migrating from upstream keep legacy custom skills in `~/.config/superpowers/skills` without being warned by this fork.

## Scope

In scope:
- Portable visual companion instructions for root and embedded Codex skill copies.
- Packaging excludes for removed runtimes.
- Namespace cleanup in contribution docs.
- Repository URL cleanup in Claude and Codex manifests.
- Legacy custom-skill warning coverage for both upstream and fork config directories.
- Pressure scenario documenting the portable visual companion regression.

Out of scope:
- Reintroducing Cursor, Gemini, OpenCode, or Copilot support.
- Changing the original workflow order.
- Changing MCP tools from optional/fallback behavior to hard requirements.
- Publishing or committing the changes.

## Design

The fork keeps the original Superpowers workflow order:

1. `brainstorming`
2. `writing-plans`
3. `using-git-worktrees`
4. `subagent-driven-development` or `executing-plans`
5. `test-driven-development`
6. `requesting-code-review`
7. `verification-before-completion`
8. `finishing-a-development-branch`

MCP-specific skills stay additive. `mcp-session-sync`, `mcp-routing`, `mcp-trace`, and `caveman-commit` guide codebase operations when those tools exist, but they must not block normal Superpowers behavior when unavailable.

## File-Level Changes

- `skills/brainstorming/visual-companion.md`: replace the maintainer-specific Codex path example with a portable `<installed-brainstorming-skill-dir>` placeholder and an explicit warning not to copy paths from another machine or session.
- `plugins/superpowers-mcp-augment/skills/brainstorming/visual-companion.md`: mirror the root skill change so the embedded Codex plugin behaves identically before the next sync.
- `skills/brainstorming/test-pressure-portable-visual-companion.md`: document the regression and expected behavior for future skill edits.
- `plugins/superpowers-mcp-augment/skills/brainstorming/test-pressure-portable-visual-companion.md`: mirror the pressure scenario in the embedded plugin.
- `scripts/sync-to-codex-plugin.sh`: exclude upstream runtime directories/files intentionally unsupported by this fork.
- `.github/PULL_REQUEST_TEMPLATE.md`: update the skill namespace in the rigor checklist.
- `.codex-plugin/plugin.json`, `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, and embedded Codex manifest: use the actual `Penz7/superpowers-mcp-augment` repository URL.
- `hooks/session-start`: warn about both upstream and fork legacy custom-skill directories.

## Verification

- Validate JSON manifests parse.
- Run shell syntax checks for `hooks/session-start` and `scripts/sync-to-codex-plugin.sh`.
- Confirm no `/Users/penz` hardcoded path remains in shipped skill docs.
- Confirm no `superpowers:writing-skills` namespace remains in contributor templates.
- Run `tests/codex-plugin-sync/test-sync-to-codex-plugin.sh` because packaging, skills, and sync behavior changed.

