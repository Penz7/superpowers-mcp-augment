# Pressure Scenario: Supported Skill Names Only

## Failure Mode

Codex follows upstream examples that mention skills not shipped by this fork, such as `frontend-design`, `mcp-builder`, or `elements-of-style`. This can make it search for missing skills, skip the actual Superpowers handoff, or treat brainstorming approval as permission to implement.

## Scenario

User asks:

> Use Superpowers MCP Augment to build a frontend mockup.

The agent loads `using-superpowers`, then `brainstorming`, and reaches the skill-priority and brainstorming terminal-state sections.

## Expected Behavior

- The agent names only skills available in this plugin when describing the workflow.
- After brainstorming, the next skill is `writing-plans`.
- Implementation proceeds only after the written spec and implementation plan gates pass.
- Optional MCP routing is described as `mcp-routing` when MCP tools are available.

## Prohibited Behavior

- Referring to `frontend-design`, `mcp-builder`, or `elements-of-style` as available next-step skills.
- Searching for or attempting to invoke unsupported skill names.
- Starting product code because the user approved an approach or design.
