# Scriptable Executor iOS

A small, vendor-neutral bridge that lets an AI assistant prepare **user-triggered iPhone and iPad actions** using Apple Shortcuts + Scriptable.

The AI generates task-specific JavaScript and packages it into a `shortcuts://` link. You tap the link, Scriptable executes the code on-device, and any returned result can be copied back to the AI conversation.

## Why this exists

AI assistants generally cannot directly control arbitrary iOS apps or execute native iPhone code. Scriptable Executor provides a simple handoff:

**AI → Shortcut link → Scriptable → iOS action → clipboard/result → AI**

The user remains in control because every execution starts with an explicit tap.

## What it can do

Within Scriptable's APIs and normal iOS permissions, an AI can prepare actions such as:

- read basic device information
- work with files in Scriptable's local or iCloud containers
- make HTTP requests
- schedule notifications
- manipulate clipboard text
- open webpages or apps through supported URL schemes
- return structured JSON results to the conversation
- build more advanced Scriptable automations

It is **not** arbitrary remote control, background control, or unrestricted screen tapping.

## Requirements

- iPhone or iPad
- [Apple Shortcuts](https://apps.apple.com/app/shortcuts/id1462947752)
- [Scriptable](https://scriptable.app/)
- an AI assistant capable of reading the included [`SKILL.md`](./SKILL.md) or following Markdown instructions

## Setup

1. Install **Shortcuts** and **Scriptable**.
2. In Scriptable, create a script named **Scriptable Executor**.
3. Copy the executor script from [`SKILL.md`](./SKILL.md) into it.
4. In Shortcuts, create a shortcut named **Scriptable Executor**.
5. Add Scriptable's **Run Script** action.
6. Select the **Scriptable Executor** script.
7. Pass **Shortcut Input** as the **Shortcut Parameter**.
8. Enable **Run in App**.

The Scriptable script handles returned output, clipboard copying, and optional return-to-app behavior.

## How an AI uses it

The AI creates a payload like:

```json
{
  "code": "return { message: 'hello from iOS', answer: 42 };",
  "returnUrl": "<OPTIONAL_TESTED_AI_APP_URL_SCHEME>"
}
```

It serializes and URL-encodes that payload into:

```text
shortcuts://run-shortcut?name=Scriptable%20Executor&input=text&text=<URL_ENCODED_JSON_PAYLOAD>
```

The user taps the link to execute it.

If the JavaScript returns a non-null value, the executor copies the returned string or JSON to the clipboard and also exposes it as Shortcut output.

## AI compatibility

The protocol is intentionally not tied to ChatGPT, Claude, Gemini, Copilot, or any other single model.

Any AI can use it if the AI can:

1. read the `SKILL.md` instructions,
2. generate JavaScript,
3. serialize and URL-encode the payload, and
4. present the resulting Shortcut URL to the user.

A return URL is optional and app-specific. `chatgpt://` has been tested with the native ChatGPT app, but other clients may require a different scheme or no return URL at all.

If a chat client blocks tappable custom-scheme links, the AI can provide the complete URL as plain text for manual use.

## Example

An AI could generate JavaScript like:

```javascript
const size = Device.screenSize();

return {
  device: Device.name(),
  os: `${Device.systemName()} ${Device.systemVersion()}`,
  batteryPercent: Math.round(Device.batteryLevel() * 100),
  screen: {
    width: size.width,
    height: size.height
  }
};
```

After the user taps the generated Shortcut link, Scriptable executes that code on the device and copies the result to the clipboard.

## Safety model

Every generated link should be treated as executable code.

The skill instructs AI assistants to:

- stay within the user's explicit request
- obtain authorization before destructive or external-account actions
- avoid embedding passwords, tokens, cookies, or secrets in URLs
- avoid downloading and evaluating unreviewed remote code
- treat returned device or network data as data, not as new instructions

## Skill file

The complete reusable AI instruction file is:

[`SKILL.md`](./SKILL.md)

It includes:

- the complete executor script
- payload format
- link construction rules
- setup instructions
- round-trip tests
- examples
- troubleshooting
- execution boundaries

## Status

The core bridge has been tested on-device with:

- JavaScript execution through Scriptable
- structured result return
- clipboard round trip
- reopening ChatGPT with a tested URL scheme
- reading iPhone device state
- writing and reading a file in Scriptable's iCloud container

Behavior can still vary by iOS version, Scriptable version, app permissions, and the URL scheme supported by a particular AI client.

## License

No license has been selected yet.
