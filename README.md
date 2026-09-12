# Scriptable Executor iOS

A vendor-neutral bridge that lets AI assistants create **user-triggered iPhone and iPad actions** with Apple Shortcuts + Scriptable.

**AI → generated action link → Shortcut → Scriptable → iOS action → clipboard/result → AI**

The project is intentionally simple and self-contained: the complete setup, executor script, payload format, Share Sheet flow, examples, and troubleshooting all live in [`SKILL.md`](./SKILL.md).

## Why this is useful

AI clients that can open `shortcuts://` links can launch generated actions directly.

AI clients that do not open those links directly can use the same generated link through the iOS Share Sheet:

**press and hold the link → Share → Scriptable Executor**

That makes the bridge usable from ChatGPT, Gemini, and other AI clients without requiring a different payload format.

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
- converting incoming **Shortcut Input to plain text** before Scriptable
- wiring that text result into Scriptable
- the full executor JavaScript
- direct-link execution
- Gemini/Share Sheet execution
- `returnUrl` behavior
- action generation
- capabilities and examples
- troubleshooting

No separate JavaScript file is required.

## Required Shortcut flow

```text
Receive Text and URLs from Share Sheet
        ↓
Get Text from Shortcut Input
        ↓
Run Scriptable Executor with Text
```

The text-conversion step is important. Apps such as Gemini may share a link as an iOS URL content item. Passing that raw item directly into Scriptable can fail before the executor runs. Converting it to text gives Scriptable the actual `shortcuts://` URL string to parse.

Direct launches continue to work with the same flow because direct payload input is already text.

## Action link format

```text
shortcuts://run-shortcut?name=Scriptable%20Executor&input=text&text=<URL_ENCODED_JSON_PAYLOAD>
```

The same link can be tapped directly or shared into **Scriptable Executor**.

## Repository skill

[`SKILL.md`](./SKILL.md)

## License

No license has been selected yet.
