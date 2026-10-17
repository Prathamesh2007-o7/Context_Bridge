# ContextBridge Privacy Policy

_Last updated: October 17, 2026_

ContextBridge captures text from an AI conversation only when the user activates the extension and chooses **Capture conversation**. The captured text may contain personal communications, project details, or other information present in that chat.

## How information is used

The captured transcript is shown in the extension so the user can review and edit it. When the user chooses to generate a handoff, ContextBridge sends that text to the Ollama service configured at `localhost:11434` or `127.0.0.1:11434` on the user's own computer. The local model processes the text to create a structured handoff. ContextBridge does not operate a cloud backend and does not send captured transcripts to a ContextBridge server. The user's local Ollama installation and computer are managed by the user.

## Storage and sharing

ContextBridge does not intentionally store captured transcripts or generated handoffs on a ContextBridge server. The current session is held in the extension popup and is cleared when the user chooses **Clear session**; closing the popup also ends the current popup session. If the user copies the handoff or exports a Markdown file, the resulting content is then under the user's control.

ContextBridge does not include advertising, analytics, or sale of conversation content. It does not share conversation content with the developer.

## User choice and safety

The extension reads the active tab only when the user invokes the capture action. Users should inspect the extracted transcript and remove passwords, API keys, authentication details, or other sensitive information before processing or copying it. Users can cancel by not generating a handoff and can clear the current session in the popup.

## Local AI requirement

ContextBridge requires a local Ollama service and a compatible model, such as Gemma 3 (4B). Conversation text submitted for handoff generation is sent to that local service, not to a hosted AI API operated by ContextBridge.

## Changes and contact

This policy may be updated if the extension's data handling changes. For questions, contact the maintainer through the project's public repository.
