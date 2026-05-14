---
name: mcp-trace
description: Use when a bug, feature, refactor, or review requires tracing dependencies, call chains, data flow, impact zones, or cross-module behavior in a codebase
---

# MCP Trace

Map the code path before changing it.

## Trace Flow

1. Define the entry point, symptom, or target symbol.
2. Use codebase-memory `search_graph` to find the exact function/class/route.
3. Use codebase-memory `trace_path`:
   - `mode: calls` for caller/callee chains.
   - `mode: data_flow` for value propagation.
   - `mode: cross_service` for HTTP/async boundaries.
4. Use Serena `find_referencing_symbols` for exact impact zones in editable source.
5. Use codebase-memory `get_code_snippet` or Serena symbol reads for only the symbols on the path.
6. Summarize the trace before implementation.

## Output

```text
Trace:
- Entry: <symbol/route/file>
- Path: A -> B -> C
- Data crossing: <important args/values>
- Impact zones: <symbols/files>
- Unknowns: <gaps or stale graph concerns>
```

## Debugging Integration

For bugs:
- Start from the observed failure or runtime log.
- Trace backward to the origin of the bad value or wrong decision.
- If runtime traces are available, ingest them into codebase-memory before forming a fix hypothesis.
- Do not patch the first suspicious function without completing the trace.

## Fallbacks

Use Serena references when graph data is missing. Use shell search only for literal error strings, logs, config keys, or when MCP tools cannot answer.
