---
name: mcp-session-sync
description: Use at the start of a coding session in a repository when codebase-memory-mcp or Serena tools are available, especially after context loss, branch changes, or returning to ongoing work
---

# MCP Session Sync

Initialize the execution layer before normal Superpowers workflow work.

## When to Run

Run once near session start when:
- You are inside a repository.
- codebase-memory or Serena tools are available.
- The task will involve codebase understanding, implementation, debugging, or review.

Skip for casual questions, pure writing tasks, or repositories with no configured MCP tools.

## Sequence

1. Identify the active repository path.
2. codebase-memory `list_projects()` and select the project matching the current path.
3. codebase-memory `detect_changes(project)` to understand drift since the graph was built.
4. Serena `get_current_config()` to verify active project and tool state.
5. Serena `check_onboarding_performed()`.
6. If onboarding is missing, run Serena `onboarding()` before code navigation.
7. Serena `list_memories()` and read only memories relevant to the task.
8. If long architecture docs are relevant and Caveman compression is available, compress them before loading details.

## Report Format

Keep the report short:

```text
Session sync:
- Project: <name/path>
- Graph: <available/stale/missing>
- Serena: <active/onboarding needed/unavailable>
- Relevant memories: <names or none>
- Fallbacks: <only if needed>
```

## Common Mistakes

- Running onboarding repeatedly when it already exists.
- Reading every memory instead of relevant memory keys.
- Treating stale graph output as authoritative without noting drift.
- Blocking the user if one MCP is unavailable; continue with available tools.
