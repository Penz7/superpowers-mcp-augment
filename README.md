# Superpowers MCP Augment

Superpowers MCP Augment is a fork of the original [Superpowers](https://github.com/obra/superpowers) project. It keeps the Superpowers workflow skeleton for planning, TDD, debugging, review, and delivery, then adds an MCP-aware execution layer for code navigation and implementation.

The goal is simple: Superpowers controls the workflow, while specialized tools handle the codebase.

```text
Superpowers MCP Augment
  |- brainstorming / planning / TDD / review workflow
  |- codebase-memory-mcp for architecture and graph intelligence
  |- Serena MCP for symbol navigation, edits, diagnostics, and memories
  `- Caveman for compression, commit wording, and review summaries
```

## Credits

This fork exists because the original Superpowers project established a strong workflow model for coding agents. Credit and thanks go to Jesse Vincent, Prime Radiant, and the Superpowers contributors for the original methodology, skill structure, and agent behavior design.

This repository is a focused fork for Claude and Codex users who want MCP-augmented execution. It is not an upstream replacement and does not claim ownership of the original Superpowers work.

## Supported Agents

- Claude Code
- Codex CLI / Codex App

Support for other coding harnesses was intentionally removed from this fork to keep the plugin focused and easier to maintain.

## Required MCP Setup

The plugin still works without MCP tools, but the main benefit comes when these tools are installed and available in your agent session.

**Recommended MCP stack:**

- `codebase-memory-mcp`: architecture overview, graph search, source snippets, dependency tracing, runtime trace ingestion.
- `serena-mcp`: project onboarding, symbol search, references, symbol edits, diagnostics, and project memories.
- `caveman`: compression, commit-message generation, and code-review summarization when available.

**Expected tool capabilities:**

| Tool | Used for |
|---|---|
| `codebase-memory` `list_projects` | Detect indexed projects |
| `codebase-memory` `detect_changes` | Check graph drift |
| `codebase-memory` `get_architecture` | Understand architecture before edits |
| `codebase-memory` `search_graph` | Find functions/classes/routes |
| `codebase-memory` `trace_path` | Trace calls, data flow, and impact |
| `codebase-memory` `get_code_snippet` | Read exact symbol source |
| `serena` `get_current_config` | Verify active project |
| `serena` `check_onboarding_performed` / `onboarding` | Prepare project context |
| `serena` `find_symbol` / `find_referencing_symbols` | Navigate code semantically |
| `serena` `replace_symbol_body` / `insert_*_symbol` | Edit symbols safely |
| `serena` `get_diagnostics_for_file` | Validate edited files |
| `serena` `list_memories` / `read_memory` / `write_memory` | Persist useful project context |
| `caveman` commit/review/compress helpers | Reduce context and standardize summaries |

If a tool is unavailable, the skills fall back to normal Superpowers behavior instead of failing hard.

## Install: Claude Code

### From GitHub Marketplace Source

Register this repository as a plugin marketplace:

```bash
/plugin marketplace add Penz7/superpowers-mcp-augment
```

Install the plugin:

```bash
/plugin install superpowers-mcp-augment@superpowers-mcp-augment-dev
```

Restart Claude Code or start a new session after installing.

### Local Development Install

Clone the repo:

```bash
git clone https://github.com/Penz7/superpowers-mcp-augment.git
cd superpowers-mcp-augment
```

Register the local checkout according to your Claude Code plugin development workflow, then install `superpowers-mcp-augment`.

## Install: Codex

This repository includes a Codex plugin manifest at:

```text
.codex-plugin/plugin.json
```

Once published to the Codex plugin marketplace, install it from the Codex plugin UI by searching:

```text
superpowers-mcp-augment
```

For development or marketplace sync, use:

```bash
./scripts/sync-to-codex-plugin.sh --bootstrap
```

The sync script packages this plugin into:

```text
plugins/superpowers-mcp-augment
```

## Verify Installation

Start a clean Claude or Codex session inside a code repository and ask:

```text
Let's make a react todo list
```

Expected behavior:

- The agent loads `using-superpowers`.
- The agent uses `brainstorming` before writing code.
- If MCP tools are available, the agent also initializes MCP session context and routes code discovery through `mcp-routing`.

You can also ask directly:

```text
Use superpowers-mcp-augment:mcp-session-sync for this repo.
```

## How It Works

The workflow follows the original Superpowers shape:

1. `brainstorming`: clarify requirements before implementation.
2. `writing-plans`: write an explicit implementation plan.
3. `using-git-worktrees`: isolate work when appropriate.
4. `subagent-driven-development` or `executing-plans`: execute tasks.
5. `test-driven-development`: enforce red-green-refactor.
6. `requesting-code-review`: review early and often.
7. `verification-before-completion`: verify before claiming success.
8. `finishing-a-development-branch`: decide how to merge, PR, keep, or discard work.

The fork adds these MCP-oriented skills:

- `mcp-session-sync`: initialize codebase-memory, Serena, onboarding, and relevant memories.
- `mcp-routing`: route architecture, navigation, editing, diagnostics, review, and commit wording to the best available tool.
- `mcp-trace`: map call chains, data flow, and impact zones before changing code.
- `caveman-commit`: delegate commit and review wording to Caveman when available.

## Usage Pattern

For a normal feature request:

```text
I want to add user profile editing. Use Superpowers MCP Augment and the MCP tools if available.
```

For codebase sync at session start:

```text
Use superpowers-mcp-augment:mcp-session-sync, then summarize the active project state.
```

For a bug:

```text
Use systematic debugging and mcp-trace to find the root cause before changing code.
```

For a plan:

```text
Execute docs/superpowers/plans/my-feature.md using superpowers-mcp-augment:subagent-driven-development.
```

## Skills Library

**Workflow**

- `using-superpowers`
- `brainstorming`
- `writing-plans`
- `executing-plans`
- `subagent-driven-development`
- `dispatching-parallel-agents`
- `using-git-worktrees`
- `finishing-a-development-branch`

**Engineering discipline**

- `test-driven-development`
- `systematic-debugging`
- `requesting-code-review`
- `receiving-code-review`
- `verification-before-completion`

**MCP augmentation**

- `mcp-session-sync`
- `mcp-routing`
- `mcp-trace`
- `caveman-commit`

**Skill authoring**

- `writing-skills`

## Development

Run lightweight validation:

```bash
python3 -m json.tool .claude-plugin/plugin.json >/dev/null
python3 -m json.tool .codex-plugin/plugin.json >/dev/null
bash -n hooks/session-start scripts/sync-to-codex-plugin.sh
bash tests/codex-plugin-sync/test-sync-to-codex-plugin.sh
```

Validate all skill frontmatter:

```bash
python3 - <<'PY2'
from pathlib import Path
for p in sorted(Path('skills').glob('*/SKILL.md')):
    text = p.read_text()
    if not text.startswith('---\n') or '\n---\n' not in text[4:]:
        raise SystemExit(f'bad frontmatter: {p}')
    head = text.split('\n---\n', 1)[0]
    if 'name:' not in head or 'description:' not in head:
        raise SystemExit(f'missing required frontmatter: {p}')
print('all skill frontmatter ok')
PY2
```

## License

MIT License. This fork retains the original license and attribution.

## Acknowledgements

Thank you to the original Superpowers maintainers and contributors for the workflow foundation this fork builds on.
