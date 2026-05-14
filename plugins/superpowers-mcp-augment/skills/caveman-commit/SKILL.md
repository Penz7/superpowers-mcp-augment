---
name: caveman-commit
description: Use when preparing commit messages or code review summaries and a Caveman commit, review, or compression capability is available
---

# Caveman Commit

Delegate commit and review wording to Caveman when the capability is available.

## Commit Messages

Use the Caveman commit helper to generate the message from the verified diff.

Required format:

```text
<type>(<scope>): <subject>
```

Rules:
- Subject max 50 characters.
- Use lowercase conventional commit type.
- Do not mention tools unless the change is tooling-specific.
- Do not generate a commit message before verification.

If Caveman is unavailable, write the message manually using the same format.

## Review Summaries

Use the Caveman review helper to compress long diffs or reviewer output when available. Keep blocking findings intact; compression must not hide Critical or Important issues.

## Red Flags

- Committing before fresh verification.
- Letting compression remove evidence, commands, or failure details.
- Treating Caveman output as authoritative without checking it against the diff.
