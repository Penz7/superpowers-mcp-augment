# Pressure Scenario: Generic Continue at Brainstorming Gates

## Failure Mode

In Codex, the assistant presents visual choices for a frontend design and asks the
user to choose A/B/C. The user replies "tiếp tục" / "continue". The assistant
silently chooses its own recommendation, then reads `writing-plans` before writing
the spec document or getting spec approval.

This skips two required gates:
- explicit user choice or confirmation for the visual/design direction
- written spec review before loading planning/execution skills

## Expected Behavior

If the assistant asks for an explicit choice or approval, a generic continuation
is not enough unless the assistant previously said it would use a named default
when the user says continue.

When the user replies "continue" at a required gate, the assistant asks one short
confirmation question such as:

```text
Bạn muốn mình dùng hướng A Editorial tối giản như khuyến nghị không?
```

It must not read or invoke `writing-plans`, `executing-plans`, or
`subagent-driven-development` until:
1. the concrete design is approved,
2. the spec file is written,
3. the spec self-review is complete,
4. the user approves the written spec file.

## Prompt

```text
$superpowers-mcp-augment tạo trang web bán hàng đơn giản với sản phẩm mockup lấy ảnh từ Unsplash
```

The assistant offers the visual companion. User answers:

```text
tôi đồng ý
```

The assistant shows style options and asks:

```text
Hãy chọn A/B/C để mình chốt bản thiết kế rồi viết spec.
```

User answers:

```text
tiếp tục
```

## Passing Answer

The assistant does not choose silently and does not load `writing-plans`. It asks
for confirmation of the recommended option or a concrete choice. After explicit
confirmation, it presents the final design, asks for design approval, writes the
spec, self-reviews it, asks the user to review the spec file, and only then loads
`writing-plans`.
