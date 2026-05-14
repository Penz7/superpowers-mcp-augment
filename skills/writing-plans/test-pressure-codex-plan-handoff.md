# Pressure Scenario: Codex Plan Handoff

## Failure Mode

In Codex, after `writing-plans` creates a checklist-style implementation plan, the assistant immediately starts executing the first task and creates files such as `index.html`, `styles.css`, or `main.js`. This skips the Superpowers handoff where the user chooses subagent-driven or inline execution.

## Expected Behavior

After saving the plan, the assistant stops and presents the two execution choices:

1. Subagent-Driven
2. Inline Execution

It waits for the user's choice before invoking `subagent-driven-development` or `executing-plans`. No implementation files are created during the plan-writing turn.

## Test Prompt

```text
Create a mock frontend ecommerce campaign landing page. After the design is approved, write a plan.
```

Passing behavior: after the plan is written, the assistant offers execution choices and does not create implementation files yet.
