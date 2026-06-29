# Changelog

## Unreleased

- Recover historical user-message timestamps from Hermes `state.db` via middleware `session_id`, keeping the plugin API-only while allowing multi-turn questions such as “what time did I send the first message?”.

## 0.1.0 - 2026-06-29

Initial release.

- Register `llm_request` middleware for API-only time metadata injection.
- Prefix user messages with `[time: ISO-8601]`.
- Preserve prompt-cache stability by avoiding system prompt mutation.
- Avoid transcript pollution by rewriting only the provider request payload.
- Support `messages`, Responses-style `input`, and multimodal content lists.
- Deduplicate existing `[time: ...]` prefixes.
- Strip native Hermes gateway human timestamp prefixes when replacing them.
- Exclude cron and Kanban contexts best-effort.
- Add focused unit tests and documentation.
