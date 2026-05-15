# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent.

**Purpose:** Verify implementation is well-built (clean, tested, maintainable)

**Only dispatch after spec compliance review passes.**

Use this template for both per-task quality reviews and the final whole-implementation review.
For the final review, evaluate the complete diff against the full spec and plan, including
integration between tasks, responsive/UI behavior where applicable, and whether earlier
scaffold reviews failed to cover later feature work.

```
Task tool (general-purpose):
  Use template at requesting-code-review/code-reviewer.md

  DESCRIPTION: [task summary, from implementer's report]
  PLAN_OR_REQUIREMENTS: Task N from [plan-file]
  BASE_SHA: [commit before task]
  HEAD_SHA: [current commit]
```

**In addition to standard code quality concerns, the reviewer should check:**
- Does each file have one clear responsibility with a well-defined interface?
- Are units decomposed so they can be understood and tested independently?
- Is the implementation following the file structure from the plan?
- Did this implementation create new files that are already large, or significantly grow existing files? (Don't flag pre-existing file sizes — focus on what this change contributed.)
- If MCP tools are available, did implementation use structured graph/symbol/diagnostic tools instead of broad manual code navigation?
- If Caveman compression or review helpers were used, did they preserve blocking findings and verification evidence?
- For frontend/UI work, is there browser, screenshot, or explicit visual QA evidence? If not, flag that visual correctness is unverified even if the build passes.

**Code reviewer returns:** Strengths, Issues (Critical/Important/Minor), Assessment
