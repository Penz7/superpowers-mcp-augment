---
name: mcp-routing
description: Use when working in a codebase with codebase-memory-mcp, Serena, or Caveman tools available and the task requires code discovery, editing, debugging, review, summarization, or implementation
---

# MCP Routing

Use Superpowers as the workflow brain, then route codebase operations to the best available tool instead of reading and editing files manually.

## Core Rule

Prefer structured tools over raw shell file reading.

| Need | First choice | Fallback |
|------|--------------|----------|
| Architecture overview | codebase-memory `get_architecture` | README/docs |
| Find functions/classes/routes | codebase-memory `search_graph` | Serena `find_symbol` |
| Trace calls/data/impact | codebase-memory `trace_path` | Serena references |
| Read symbol source | codebase-memory `get_code_snippet` | Serena symbol tools |
| Navigate exact code | Serena `find_symbol`, `find_referencing_symbols` | `rg` only if symbolic search fails |
| Edit symbol bodies | Serena `replace_symbol_body`, `insert_*_symbol` | `apply_patch` for non-symbol or broad edits |
| Diagnostics | Serena `get_diagnostics_for_file` | project analyzer/test output |
| Compress long artifacts | Caveman compression if available | concise manual summary |
| Commit messages | Caveman commit helper if available | write manually after verification |
| Code review summaries | Caveman review helper if available | reviewer subagent/template |

## Hard Constraints

- Do not start with `grep`, `cat`, `sed`, or broad file reads for code discovery when graph or Serena tools are available.
- Use shell search only for string literals, config files, generated artifacts, missing MCP coverage, or when graph/Serena results are insufficient.
- Do not force MCP usage for Markdown docs, JSON manifests, shell scripts, CI config, or assets.
- Do not invent tool results. If a tool is unavailable or fails, say so and use the fallback.

## Execution Layer Sequence

Before implementation:
1. codebase-memory `detect_changes(project)` if a project graph exists.
2. codebase-memory `get_architecture(project)` for unfamiliar codebases.
3. Serena `get_current_config()` and `check_onboarding_performed()` if Serena is active.
4. Serena `get_symbols_overview()` for target files or modules before edits.

During implementation:
1. Locate definitions with graph/Serena.
2. Trace impact with graph `trace_path` or Serena references.
3. Edit with Serena symbol tools when the change maps cleanly to a symbol.
4. Use `apply_patch` for Markdown, shell, JSON, or multi-symbol edits.

After each logical unit:
1. Run Serena diagnostics on edited source files when available.
2. Run the verification command from the active Superpowers workflow.
3. Write Serena memory only for durable project context, not routine progress noise.

## Subagent Instructions

When dispatching implementer or reviewer subagents, include:

```text
Use structured code tools first. Prefer codebase-memory for architecture,
call graph, and source snippets. Prefer Serena for symbol navigation,
references, diagnostics, and symbol edits. Use shell reads/search only when
MCP tools are unavailable or insufficient, and explain the fallback.
```

## Red Flags

- Starting code discovery with broad `find`, `grep`, or `cat`.
- Reading entire files when a symbol query would answer the question.
- Editing by line number when a symbol edit is available.
- Writing memories for every step instead of only reusable context.
- Treating Caveman as mandatory when no Caveman tool is installed.
