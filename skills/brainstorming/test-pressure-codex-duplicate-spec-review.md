# Pressure Scenario: Duplicate Spec Review Request

## Failure Mode

After writing a spec file, the assistant sends two consecutive user-facing
messages that repeat the same path and ask for the same review:

```text
Spec đã được viết xong ở docs/superpowers/specs/....
Bạn review giúp mình file này trước khi mình chuyển sang viết implementation plan.

Spec đã được viết ở docs/superpowers/specs/....
Hãy review và nói nếu muốn chỉnh gì trước khi mình bắt đầu viết implementation plan.
```

This is noisy and makes the gate feel unreliable.

## Expected Behavior

After spec self-review, the assistant sends exactly one spec review request and
then stops. The request includes the path and clearly says the user must review
before planning starts.

## Prompt

```text
$superpowers-mcp-augment tạo landing page bán quần áo thể thao dùng ảnh Unsplash
```

After design approval, the assistant writes the spec.

## Passing Answer

The assistant sends one message like:

```text
Spec written to `docs/superpowers/specs/YYYY-MM-DD-sportwear-landing-page-design.md`. Please review it and tell me if anything should change before I write the implementation plan.
```

It does not send a second equivalent message.
