# Superpowers MCP Augment

Superpowers MCP Augment is a Claude/Codex workflow framework built on the Superpowers methodology and augmented with MCP routing for codebase-memory, Serena, and Caveman.

## Quickstart

Give your agent Superpowers MCP Augment: [Claude Code](#claude-code), [Codex CLI](#codex-cli), or [Codex App](#codex-app).

## How it works

It starts from the moment you fire up your coding agent. As soon as it sees that you're building something, it *doesn't* just jump into trying to write code. Instead, it steps back and asks you what you're really trying to do. 

Once it's teased a spec out of the conversation, it shows it to you in chunks short enough to actually read and digest. 

After you've signed off on the design, your agent puts together an implementation plan that's clear enough for an enthusiastic junior engineer with poor taste, no judgement, no project context, and an aversion to testing to follow. It emphasizes true red/green TDD, YAGNI (You Aren't Gonna Need It), and DRY. 

Next up, once you say "go", it launches a *subagent-driven-development* process, having agents work through each engineering task, inspecting and reviewing their work, and continuing forward. It's not uncommon for Claude to be able to work autonomously for a couple hours at a time without deviating from the plan you put together.

There's a bunch more to it, but that's the core of the system. And because the skills trigger automatically, you don't need to do anything special. Your coding agent just has the augmented workflow.

## Installation

Installation differs by harness. If you use more than one, install Superpowers MCP Augment separately for each one.

### Claude Code

Install the plugin from your fork or development marketplace as `superpowers-mcp-augment`.

#### Official Marketplace

- Install from a marketplace entry:

  ```bash
  /plugin install superpowers-mcp-augment
  ```

#### Development Marketplace

For local development, register this checkout as a plugin marketplace.

- Register the marketplace:

  ```bash
  /plugin marketplace add <your-fork-or-local-marketplace>
  ```

- Install the plugin from this marketplace:

  ```bash
  /plugin install superpowers-mcp-augment@superpowers-mcp-augment-dev
  ```

### Codex CLI

Install Superpowers MCP Augment from the Codex plugin marketplace or your forked plugin source.

- Open the plugin search interface:

  ```bash
  /plugins
  ```

- Search for Superpowers MCP Augment:

  ```bash
  superpowers-mcp-augment
  ```

- Select `Install Plugin`.

### Codex App

Install Superpowers MCP Augment from the Codex plugin marketplace or your forked plugin source.

- In the Codex app, click on Plugins in the sidebar.
- You should see `Superpowers MCP Augment` in the Coding section.
- Click the `+` next to Superpowers MCP Augment and follow the prompts.

## The Basic Workflow

1. **brainstorming** - Activates before writing code. Refines rough ideas through questions, explores alternatives, presents design in sections for validation. Saves design document.

2. **using-git-worktrees** - Activates after design approval. Creates isolated workspace on new branch, runs project setup, verifies clean test baseline.

3. **writing-plans** - Activates with approved design. Breaks work into bite-sized tasks (2-5 minutes each). Every task has exact file paths, complete code, verification steps.

4. **subagent-driven-development** or **executing-plans** - Activates with plan. Dispatches fresh subagent per task with two-stage review (spec compliance, then code quality), or executes in batches with human checkpoints.

5. **test-driven-development** - Activates during implementation. Enforces RED-GREEN-REFACTOR: write failing test, watch it fail, write minimal code, watch it pass, commit. Deletes code written before tests.

6. **requesting-code-review** - Activates between tasks. Reviews against plan, reports issues by severity. Critical issues block progress.

7. **finishing-a-development-branch** - Activates when tasks complete. Verifies tests, presents options (merge/PR/keep/discard), cleans up worktree.

**The agent checks for relevant skills before any task.** Mandatory workflows, not suggestions. When codebase-memory, Serena, or Caveman are available, the augmented skills route code discovery, editing, diagnostics, summaries, and review through those tools.

## What's Inside

### Skills Library

**Testing**
- **test-driven-development** - RED-GREEN-REFACTOR cycle (includes testing anti-patterns reference)

**Debugging**
- **systematic-debugging** - 4-phase root cause process (includes root-cause-tracing, defense-in-depth, condition-based-waiting techniques)
- **verification-before-completion** - Ensure it's actually fixed
- **mcp-trace** - Trace call chains, data flow, and impact zones with MCP tools

**Collaboration** 
- **brainstorming** - Socratic design refinement
- **writing-plans** - Detailed implementation plans
- **executing-plans** - Batch execution with checkpoints
- **dispatching-parallel-agents** - Concurrent subagent workflows
- **requesting-code-review** - Pre-review checklist
- **receiving-code-review** - Responding to feedback
- **using-git-worktrees** - Parallel development branches
- **finishing-a-development-branch** - Merge/PR decision workflow
- **subagent-driven-development** - Fast iteration with two-stage review (spec compliance, then code quality)
- **mcp-session-sync** - Initialize graph, Serena state, onboarding, and memories at session start
- **mcp-routing** - Route code navigation, edits, diagnostics, summaries, and review to the best available tool
- **caveman-commit** - Delegate commit and review wording to Caveman when available

**Meta**
- **writing-skills** - Create new skills following best practices (includes testing methodology)
- **using-superpowers** - Introduction to the skills system

## Philosophy

- **Test-Driven Development** - Write tests first, always
- **Systematic over ad-hoc** - Process over guessing
- **Complexity reduction** - Simplicity as primary goal
- **Evidence over claims** - Verify before declaring success

This fork builds on the original Superpowers workflow philosophy and narrows support to Claude and Codex.

## Contributing

The general contribution process for Superpowers MCP Augment is below. Keep in mind that updates to skills should be tested across Claude and Codex.

1. Fork the repository
2. Switch to the 'dev' branch
3. Create a branch for your work
4. Follow the `writing-skills` skill for creating and testing new and modified skills
5. Submit a PR, being sure to fill in the pull request template.

See `skills/writing-skills/SKILL.md` for the complete guide.

## Updating

Updates depend on whether you installed the Claude or Codex plugin, but are often automatic.

## License

MIT License - see LICENSE file for details

## Community

Superpowers MCP Augment is a fork of Superpowers, adapted for Claude/Codex MCP-augmented workflows.

- **Issues**: https://github.com/penz/superpowers-mcp-augment/issues
