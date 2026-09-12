# Scriptable Executor iOS

A vendor-neutral bridge that lets AI assistants create **user-triggered iPhone and iPad actions** with Apple Shortcuts + Scriptable.

**AI → generated action link → Shortcut → Scriptable → iOS action → clipboard/result → AI**

The same generated link works in two ways: tap it directly in clients that support `shortcuts://`, or **press and hold → Share → Scriptable Executor** in clients such as Gemini.

## What it can do

Within Scriptable's APIs and normal iOS permissions, an AI can generate actions that read device state, work with local/iCloud files, call APIs, schedule notifications and timer-like alerts, manipulate clipboard content, open deep links and compose flows, show custom `WebView` interfaces, and create/update Scriptable widgets.

## Requirements

- iPhone or iPad
- [Apple Shortcuts](https://apps.apple.com/app/shortcuts/id1462947752)
- [Scriptable](https://scriptable.app/)
- an AI assistant that can read [`SKILL.md`](./SKILL.md)

## Setup

1. Install **Shortcuts** and **Scriptable**.
2. In Scriptable, create a script named **Scriptable Executor**.
3. Paste [`scriptable-executor.js`](./scriptable-executor.js) into that script.
4. In Shortcuts, create a shortcut named **Scriptable Executor**.
5. Open the shortcut details and enable **Show in Share Sheet**.
6. Configure Share Sheet input to receive **Text and URLs**. Keep **Continue** when there is no input.
7. Add Scriptable's **Run Script** action and select **Scriptable Executor**.
8. Pass **Shortcut Input** as the **Shortcut Parameter**.
9. Enable **Run in App**.

That single shortcut handles both direct action links and links shared from another AI app.

## How an AI generates an action

Create a payload:

```json
{
  "code": "return { message: 'hello from iOS', answer: 42 };",
  "returnUrl": "chatgpt://"
}
```

Serialize it and URL-encode it into:

```text
shortcuts://run-shortcut?name=Scriptable%20Executor&input=text&text=<URL_ENCODED_JSON_PAYLOAD>
```

For ChatGPT round trips, use `chatgpt://`. For another AI app, use its supported return URL when appropriate. Omit `returnUrl` when the action intentionally leaves another destination on screen.

## Running from ChatGPT

Tap the generated action link. Shortcuts launches Scriptable, executes the JavaScript, copies any returned value to the clipboard, and optionally returns to ChatGPT.

## Running from Gemini or another client with Share Sheet

Use the **same generated action link**:

1. Press and hold the link.
2. Tap **Share**.
3. Select **Scriptable Executor**.
4. The shortcut passes the shared URL/text to Scriptable.
5. Scriptable extracts the embedded payload and executes it.

No separate Gemini-specific JavaScript format is required.

## Executor input formats

[`scriptable-executor.js`](./scriptable-executor.js) accepts:

- an already-parsed payload object
- a raw JSON payload string
- a full generated `shortcuts://run-shortcut?...&text=...` URL
- shared text containing that generated Shortcut URL

## Skill file

Install or give your AI [`SKILL.md`](./SKILL.md). It contains the full protocol, link-generation rules, setup instructions, capabilities, examples, Share Sheet instructions, troubleshooting, and execution boundaries.

## Safety model

Each generated action link is executable code. The user explicitly launches it by tapping the link or choosing **Scriptable Executor** from the Share Sheet. Normal iOS permissions continue to apply.

## License

No license has been selected yet.
