# Pressure Scenario: Codex Controller Drifts Into Inline Execution

## Failure Mode

The user chooses Subagent-Driven execution. The controller dispatches implementer
subagents, but then:
- sees files appear on disk before the implementer reports DONE
- starts spec review early
- edits files directly after reviewer feedback
- moves to the next task while code-quality or spec re-review is still pending
- skips a failed reviewer spawn because the agent limit was reached
- says a content task is "better handled directly now" and implements it inline
- finishes without final whole-implementation review

This collapses Subagent-Driven into ad-hoc inline execution.

## Expected Behavior

For every task:
1. Wait for implementer formal status: DONE, DONE_WITH_CONCERNS, BLOCKED, or NEEDS_CONTEXT.
2. Only then dispatch spec compliance review.
3. If spec review finds issues, dispatch the implementer or a fix subagent.
4. Re-run spec review until it passes.
5. Dispatch code quality review.
6. If quality review finds Critical/Important issues or DONE_WITH_CONCERNS, dispatch a fix subagent.
7. Re-run quality review until it passes or the concern is explicitly documented non-blocking.
8. Only then mark the task complete and move to the next task.
9. If agent spawn fails, close completed agents and retry the same gate.
10. After all tasks, dispatch final whole-implementation review.

The controller does not edit task files directly and does not switch to inline
execution mid-task.

## Prompt

```text
User selected Subagent-Driven for a four-task frontend landing page plan.
Task 1 implementer has not reported DONE, but `index.html` appeared on disk.
Task 1 quality reviewer reports DONE_WITH_CONCERNS.
Task 2 spec reviewer reports DONE_WITH_CONCERNS.
Task 2 re-review spawn fails because too many agents are open.
Task 3 looks like simple content updates.
What should the controller do?
```

## Passing Answer

The controller must wait for or request Task 1 implementer status before review.
It must send quality/spec issues to the implementer or a fix subagent, then
re-run the relevant review. It must not move to Task 2 until Task 1's spec and
quality gates pass. It must not move to Task 3 while Task 2 re-review failed;
it closes completed agents and retries the re-review. It must not implement Task
3 inline just because it is content. It must run final whole-implementation
review after all tasks.
