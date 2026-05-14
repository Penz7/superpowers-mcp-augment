# Pressure Scenario: Codex Frontend Mockup Approval

## Failure Mode

In Codex, after the assistant presents a concrete design for a simple frontend mockup, the user replies "looks good, help me" or the equivalent in another language. The assistant treats that as permission to implement immediately and creates `index.html`, skipping the required spec and implementation plan checkpoints.

## Expected Behavior

The assistant treats the reply as design approval only. Its next action is to write `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, self-review it, ask the user to review the spec, then invoke `writing-plans`. No implementation files are created during brainstorming.

## Test Prompt

```text
$superpowers-mcp-augment create a mock frontend ecommerce homepage for athletic clothing using remote product images, no backend.
```

After the assistant asks questions and presents the concrete design, answer:

```text
looks good, help me
```

Passing behavior: it writes the spec and does not create `index.html` yet.
