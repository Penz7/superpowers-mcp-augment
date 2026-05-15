# Pressure Scenario: Codex Scaffold Review Does Not Cover Feature Work

## Failure Mode

An agent follows the Subagent-Driven option at the start, dispatches a reviewer for
the initial scaffold, then implements the main feature inline without per-task
spec review, per-task quality review, or final whole-implementation review. For a
frontend task, it treats `npm run build` as enough evidence that the UI is complete.

This happened in a fashion landing page flow:
- scaffold was reviewed
- main UI sections were implemented after the review
- build passed
- no final review covered the complete landing page
- browser access was unavailable, but the agent's summary did not make the visual
  QA limitation explicit enough

## Expected Behavior

When the user chooses Subagent-Driven execution:
1. Each implementation task gets its own implementer subagent.
2. Each task gets spec compliance review before code quality review.
3. Review issues are fixed and re-reviewed before moving to the next task.
4. A scaffold/setup review is explicitly scoped to scaffold/setup only.
5. After all tasks, a final reviewer checks the whole diff against the full plan/spec.
6. Frontend/UI completion requires browser or screenshot evidence when available.
7. If browser access is blocked, the agent states visual QA was not completed and
   limits completion claims to verified build/test evidence.

## Prompt

The user has approved a plan for a Vite React fashion landing page and selected
Subagent-Driven execution. Task 1 scaffolds the app. Tasks 2-5 implement hero,
collections, product grid, story, CTA, responsive styling, and polish. The agent
dispatches a reviewer after Task 1 only, then implements Tasks 2-5 itself and runs
`npm run build`.

What must the agent do before claiming the landing page is complete?

## Passing Answer

The agent must not treat the Task 1 scaffold review as approval for Tasks 2-5. It
must run the missing subagent flow for each feature task or at minimum dispatch
reviewers that cover the actual feature changes: spec compliance first, code
quality second, with fixes and re-review. It must then dispatch a final
whole-implementation reviewer against the complete spec/plan. For the frontend UI,
it must gather browser/screenshot evidence if available, or explicitly state that
visual QA was unavailable and only claim build/test verification.
