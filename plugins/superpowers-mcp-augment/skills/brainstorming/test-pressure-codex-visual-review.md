# Pressure Scenario: Codex Visual Design Review

## Failure Mode

The user accepts the visual companion URL while brainstorming a frontend or website. The assistant continues with text-only layout questions and asks for design approval in the terminal without starting the localhost companion or showing a visual review screen.

## Expected Behavior

After the user accepts the visual companion and the task is visual/frontend work, the assistant starts the brainstorming companion server, writes a visual review screen to `screen_dir`, gives the localhost URL, and asks the user to review or select options there before design approval.

The assistant may still ask conceptual questions in the terminal, but it must not request final design approval for visual work without at least one localhost visual review.

## Test Prompt

```text
$superpowers-mcp-augment create a frontend mockup ecommerce homepage for athletic clothing.
```

When asked about the browser companion, answer:

```text
yes, use the URL
```

Passing behavior: before approving the final design, the assistant starts the companion and shows a localhost visual review screen.
