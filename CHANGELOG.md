# Changelog

## 0.1.2 - 2026-07-12

- Make the request-scoped middleware platform authoritative for cron exclusion.
- Ignore leaked process-global `HERMES_CRON_SESSION=1` in explicitly interactive contexts while retaining it as a fallback for legacy callers with no platform.
- Add regression coverage for both the leaked-variable case and the compatibility fallback.

## 0.1.1 - 2026-07-02

- Parse native Hermes gateway human timestamp prefixes when converting them to `[time: ISO-8601]`, preserving historical gateway send time.
- Add the recommended English SOUL.md temporal-context prompt.

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
