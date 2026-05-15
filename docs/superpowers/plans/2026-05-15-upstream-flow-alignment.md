# Upstream Flow Alignment Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers-mcp-augment:subagent-driven-development (recommended) or superpowers-mcp-augment:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Align Superpowers MCP Augment with upstream Superpowers workflow expectations while preserving Claude/Codex-only MCP augmentation.

**Architecture:** This is a documentation, packaging, and hook correction. The workflow sequence stays unchanged; MCP additions remain optional execution-layer guidance.

**Tech Stack:** Markdown skills, JSON plugin manifests, Bash hook/sync scripts.

## Task 1: Add Pressure Scenario For Portable Visual Companion Paths

**Files:**
- Create: `skills/brainstorming/test-pressure-portable-visual-companion.md`
- Create: `plugins/superpowers-mcp-augment/skills/brainstorming/test-pressure-portable-visual-companion.md`

- [ ] **Step 1: Write the pressure scenario**

```markdown
# Pressure Scenario: Portable Visual Companion Script Path

## Failure Mode

The visual companion instructions show a maintainer-specific absolute path such as `/Users/penz/...`, and an agent copies that path when running on another machine or plugin version.

## Scenario

User asks for visual brainstorming in Codex. The installed skill path differs from the maintainer's local cache path.

## Expected Behavior

The agent resolves the script path from the currently installed brainstorming skill directory surfaced by the active skill list or plugin runtime, then runs that session-local path. The instructions must not contain a real maintainer home directory.

## Regression Check

Search shipped skill docs for `/Users/penz`. The result must be empty.
```

- [ ] **Step 2: Mirror the scenario in the embedded plugin**

Copy the same content to the embedded Codex plugin path so local marketplace testing uses the same pressure scenario.

## Task 2: Make Visual Companion Instructions Portable

**Files:**
- Modify: `skills/brainstorming/visual-companion.md`
- Modify: `plugins/superpowers-mcp-augment/skills/brainstorming/visual-companion.md`

- [ ] **Step 1: Replace the hardcoded path example**

Replace the maintainer-specific Codex command with:

```bash
<installed-brainstorming-skill-dir>/scripts/start-server.sh --project-dir /path/to/project
```

- [ ] **Step 2: Add a portability guard**

State that agents must use the active installed skill path for the current session and must not copy paths from another machine, user account, or plugin version.

## Task 3: Keep Packaging Excludes Aligned With Fork Scope

**Files:**
- Modify: `scripts/sync-to-codex-plugin.sh`

- [ ] **Step 1: Add removed runtime excludes**

Add these top-level excludes:

```bash
"/.cursor-plugin/"
"/.opencode/"
"/GEMINI.md"
"/gemini-extension.json"
```

- [ ] **Step 2: Preserve current Codex plugin destination**

Keep:

```bash
DEST_REL="plugins/superpowers-mcp-augment"
```

## Task 4: Fix Namespace And Repository Metadata

**Files:**
- Modify: `.github/PULL_REQUEST_TEMPLATE.md`
- Modify: `.codex-plugin/plugin.json`
- Modify: `.claude-plugin/plugin.json`
- Modify: `.claude-plugin/marketplace.json`
- Modify: `plugins/superpowers-mcp-augment/.codex-plugin/plugin.json`

- [ ] **Step 1: Update contribution checklist namespace**

Change:

```markdown
superpowers:writing-skills
```

to:

```markdown
superpowers-mcp-augment:writing-skills
```

- [ ] **Step 2: Update repository URLs**

Use:

```text
https://github.com/Penz7/superpowers-mcp-augment
```

for homepage, repository, and website fields that point to the fork.

## Task 5: Warn About Upstream And Fork Legacy Skill Directories

**Files:**
- Modify: `hooks/session-start`

- [ ] **Step 1: Check both legacy locations**

Detect:

```bash
${HOME}/.config/superpowers/skills
${HOME}/.config/superpowers-mcp-augment/skills
```

- [ ] **Step 2: Include all detected paths in the warning**

The warning should tell users these legacy custom skills are not loaded by Claude Code's skills system and should be moved to `~/.claude/skills`.

## Task 6: Verify

**Files:**
- No production edits.

- [ ] **Step 1: Validate JSON**

Run:

```bash
python3 -m json.tool .codex-plugin/plugin.json >/dev/null
python3 -m json.tool .claude-plugin/plugin.json >/dev/null
python3 -m json.tool .claude-plugin/marketplace.json >/dev/null
python3 -m json.tool plugins/superpowers-mcp-augment/.codex-plugin/plugin.json >/dev/null
```

Expected: all commands exit successfully.

- [ ] **Step 2: Validate shell syntax**

Run:

```bash
bash -n hooks/session-start
bash -n scripts/sync-to-codex-plugin.sh
```

Expected: both commands exit successfully.

- [ ] **Step 3: Check regressions**

Run:

```bash
rg '/Users/penz|superpowers:writing-skills' skills plugins/superpowers-mcp-augment .github scripts hooks .codex-plugin .claude-plugin
```

Expected: no matches for the removed hardcoded path or old checklist namespace.

- [ ] **Step 4: Run packaging sync test**

Run:

```bash
tests/codex-plugin-sync/test-sync-to-codex-plugin.sh
```

Expected: test passes.

