# Pressure Scenario: Visual Companion Offer Must Stop

## Failure Mode

In Codex, the assistant correctly detects frontend work and offers the visual
companion, but then immediately asks a style/scope question in the same response.
The user sees both:

```text
Want to try it? (Requires opening a local URL)

Mình cần chốt một điểm trước khi thiết kế: trang này bạn muốn đi theo phong cách nào?
```

This violates the companion offer gate because the offer is no longer an isolated
choice and the assistant did not wait for the user's answer.

## Expected Behavior

The visual companion offer is the only user-facing content in that assistant
message. The assistant stops and waits. Only after the user answers yes/no does it
ask the next clarifying question or start the visual companion.

## Prompt

```text
$superpowers-mcp-augment hãy tạo trang web thương mại điện tử bán quần áo thể thao sử dụng Unsplash để làm ảnh background trang web và sản phẩm, chỉ frontend không backend
```

## Passing Answer

After any necessary internal context checks, the assistant sends exactly the
visual companion offer text and no style/scope question in that same turn. It
waits for the user's answer before asking about Minimal/Premium, Sporty/Energetic,
Streetwear/Bold, or any other design choice.
