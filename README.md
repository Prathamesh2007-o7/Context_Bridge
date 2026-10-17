# ContextBridge

**Carry your working context between AI conversations.**

ContextBridge is a Chrome Manifest V3 extension that captures a visible AI conversation, uses a local Ollama model such as **Gemma 3 (4B)** to create a structured continuation handoff, and lets you edit, copy, or export the result as Markdown.

## Features

- Capture visible conversation messages from supported ChatGPT, Claude, and Gemini page layouts (best-effort DOM extraction; sites can change their markup).
- Review and edit extracted text before it is processed.
- Generate a structured handoff covering project state, decisions, constraints, failed attempts, blockers, and next steps.
- Run inference against Ollama on your own computer; no ContextBridge cloud backend or remote AI API is used.
- Edit the final handoff, copy it to the clipboard, or export a `.md` file.
- Keeps the current session in the popup only; Clear session removes the current text.

## Requirements

- Google Chrome or another Chromium browser that supports Manifest V3.
- [Ollama](https://ollama.com/) installed and running.
- Gemma 3 (4B) downloaded locally.

## Setup

### 1. Install the model

Open PowerShell or Command Prompt and run:

```powershell
ollama pull gemma3:4b
```

Make sure Ollama is running. Verify the model is available with:

```powershell
ollama list
```

### 2. Allow the browser extension to access Ollama

Ollama restricts browser origins. Its official guidance documents `OLLAMA_ORIGINS` for allowing browser-extension origins. For a local development setup, allow Chrome extensions:

**Windows (PowerShell):**

```powershell
[Environment]::SetEnvironmentVariable('OLLAMA_ORIGINS', 'chrome-extension://*', 'User')
```

Then completely quit Ollama from the system tray and start it again so it picks up the variable. If this doesn't take effect, sign out/in or restart Windows and retry. If you start Ollama manually from a terminal instead of the desktop app, set it in that terminal before launching Ollama:

```powershell
$env:OLLAMA_ORIGINS='chrome-extension://*'
ollama serve
```

**macOS/Linux (launch Ollama from a terminal):**

```bash
OLLAMA_ORIGINS='chrome-extension://*' ollama serve
```

For a stricter setup, replace the wildcard with the specific extension origin shown in Chrome at `chrome://extensions` (the `chrome-extension://<extension-id>` origin). Avoid allowing origins you do not trust.

### 3. Load ContextBridge into Chrome

1. Download or clone this repository.
2. Open `chrome://extensions`.
3. Enable **Developer mode**.
4. Click **Load unpacked**.
5. Select the folder containing `manifest.json`.
6. Pin ContextBridge from Chrome's Extensions menu.

### 4. Use it

1. Open a conversation in ChatGPT, Claude, or Gemini and ensure the messages you need are loaded on the page.
2. Click the ContextBridge toolbar icon.
3. Click **Capture conversation**.
4. Open **Review / edit extracted text** and check the transcript. Remove sensitive details before proceeding.
5. Click **Check Ollama**. If the model is missing, run `ollama pull gemma3:4b`.
6. Click **Generate handoff** and keep the popup open while the local model works.
7. Review/edit the handoff, then **Copy handoff** or **Export .md** and paste it into your destination AI chat.

## Troubleshooting

### `Couldn't connect to Ollama` or CORS/origin error

- Confirm Ollama is running and `http://localhost:11434/api/tags` responds locally.
- Confirm `OLLAMA_ORIGINS` includes `chrome-extension://*` (or this extension's exact origin).
- Completely quit and restart Ollama after changing the environment variable.

### Model not found

Run `ollama pull gemma3:4b`, then click **Check Ollama** again. The model name in the popup must match the model shown by `ollama list`.

### Conversation wasn't detected

Update to the latest extension folder and click Reload on `chrome://extensions`. Then refresh the actual chat tab before capturing. The extractor checks multiple known DOM patterns, but website markup can change. If it still fails, copy the full error message (it now includes the detected site and selector counts) when reporting the issue. The extension only reads content rendered in the active tab; it cannot recover messages that are not loaded into the page.

### The result is incomplete

Review the captured transcript for missing messages. Very long pages are limited to recent messages so the prompt stays manageable. Shorten the transcript manually if Gemma runs out of context or takes too long.

## Privacy and security

- The captured transcript is submitted to the Ollama service at `localhost:11434` on your computer. ContextBridge does not upload the transcript to a ContextBridge server or call a hosted AI API.
- The conversation already exists on the source website; this extension only reads text rendered in the active tab after you click Capture.
- Review the transcript before generation. Do not include passwords, API keys, access tokens, or sensitive information in a handoff.
- This MVP does not persist the transcript or generated handoff after the popup session is closed. Copy/exported content is then under your control.
- The extension requests access to the active tab only when invoked, rather than broad access to all websites.

## Tech stack

- Chrome Extension Manifest V3
- HTML, CSS, JavaScript
- Ollama local API (`/api/tags`, `/api/chat`)
- Gemma 3 (4B)

## Current limitations

- Conversation extraction relies on the websites' visible DOM and can break when those sites change their markup.
- Some historical messages may not be loaded in the webpage; the extension cannot recover content that isn't rendered.
- This is a local developer-focused MVP. Users must install Ollama and download the model first.
- Generated handoffs should be reviewed for accuracy before being pasted elsewhere.

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE).


## Troubleshooting capture

If capture finds no messages, refresh the open chat tab, wait until the messages are visible, and capture again. Chat websites periodically change their page structure; ContextBridge checks both legacy role markers and newer ChatGPT user-bubble / assistant-message markers. Long chats may render only part of their history at a time, so review the captured transcript before generating a handoff.

## Distribution

### Use locally

Load the unpacked extension from `chrome://extensions` with **Developer mode** enabled. Select the folder containing `manifest.json`. This is suitable for your own machine and development/testing.

### Publish on the Chrome Web Store

1. Register in the [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/devdashboard/) and complete the one-time developer registration fee.
2. Create a ZIP whose root contains `manifest.json` and the extension files (this release ZIP is structured that way).
3. Upload the ZIP, complete the store listing, add screenshots, and accurately complete the Privacy practices and permissions disclosures.
4. Submit it for review. Do not describe the extension as published until Chrome Web Store review is complete.

ContextBridge depends on users having Ollama installed locally with the selected model pulled. Make that requirement clear in the listing so users know it is not a cloud AI service.
