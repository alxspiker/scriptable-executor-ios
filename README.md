# Scriptable Executor iOS

A vendor-neutral bridge that lets AI assistants create **user-triggered iPhone and iPad actions** with Apple Shortcuts + Scriptable.

**AI → generated action link → Shortcut → Scriptable → iOS action → clipboard/result → AI**

The project is intentionally simple and self-contained: the complete setup, executor script, payload format, Share Sheet flow, examples, and troubleshooting all live in [`SKILL.md`](./SKILL.md).

## Why this is useful

AI clients that open `shortcuts://` links can launch generated actions directly.

AI clients that do not open those links directly can use the same generated link through the iOS Share Sheet:

**press and hold the link → Share → Scriptable Executor**

Some clients may wrap the custom Shortcut link inside an ordinary web URL before it reaches the Share Sheet. The executor unwraps encoded query-parameter values until it finds the embedded `shortcuts://run-shortcut?...` action, so the AI does not need to generate a second payload format.

## What it can do

Within Scriptable's APIs and normal device permissions, an AI can generate actions that:

- read device state
- work with local and iCloud files
- call HTTP APIs
- schedule notifications and timer-like alerts
- manipulate clipboard content
- open deep links and compose flows
- present custom `WebView` interfaces
- build mini control panels
- create and update Scriptable widgets
- return structured results to the AI conversation

## Requirements

- iPhone or iPad
- [Apple Shortcuts](https://apps.apple.com/app/shortcuts/id1462947752)
- [Scriptable](https://scriptable.app/)
- an AI assistant that can read [`SKILL.md`](./SKILL.md)

## Install

Open [`SKILL.md`](./SKILL.md) and give it to your AI assistant.

The skill contains the complete instructions for:

- creating the **Scriptable Executor** script
- creating the **Scriptable Executor** Shortcut
- enabling **Text and URLs** in the Share Sheet
- converting **Shortcut Input** to plain text before Scriptable runs
- the full executor JavaScript
- direct-link execution
- Share Sheet execution
- wrapped-link unwrapping
- `returnUrl` behavior
- action generation
- capabilities and examples
- troubleshooting

No separate JavaScript file is required.

## Shortcut flow

```text
Receive Text and URLs from Share Sheet
        ↓
Get Text from Shortcut Input
        ↓
Run Scriptable Executor with Text
```

## Action link format

```text
shortcuts://run-shortcut?name=Scriptable%20Executor&input=text&text=<URL_ENCODED_JSON_PAYLOAD>
```

The same action can be tapped directly or shared into **Scriptable Executor**.

## Repository skill

[`SKILL.md`](./SKILL.md)

## License

No license has been selected yet.
