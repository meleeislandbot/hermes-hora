# Hora port: Claude Code

Claude Code does not need request middleware to get Hora-style time awareness: its
[`UserPromptSubmit` hook](https://docs.anthropic.com/en/docs/claude-code/hooks) already
injects hook stdout as model-visible context for the current turn, without writing it
into the user's message or the persisted transcript.

That gives the same contract as the Hermes plugin:

- the model sees `[time: ISO-8601]` for every user turn;
- the user's message text is untouched;
- the system prompt is untouched (prompt-cache friendly);
- temporal reasoning is delegated to the model, no custom heuristics.

## Install

Add to `~/.claude/settings.json` (user scope) or `.claude/settings.json` (project scope):

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "date '+[time: %Y-%m-%dT%H:%M:%S%z]'",
            "timeout": 5
          }
        ]
      }
    ]
  }
}
```

Merge with any existing `hooks` object rather than replacing it. Hooks are snapshotted
at session start: restart Claude Code (or start a new session) for the change to apply.

If you run several Claude Code profiles via `CLAUDE_CONFIG_DIR`, remember each config
dir has its own `settings.json` — install the hook in every profile that should have
time awareness.

### Time zone

`date` uses the local time zone. To pin one, wrap the command:

```json
"command": "TZ=America/Bogota date '+[time: %Y-%m-%dT%H:%M:%S%z]'"
```

## Verify

Start a new session and ask:

```text
Technical test: if you see a [time: ...] context stamp, respond only by copying it; otherwise respond NO_TIME.
```

Expected response shape:

```text
[time: 2026-07-16T18:26:01-0500]
```

## Differences from the Hermes plugin

| | Hermes Hora | Claude Code port |
| --- | --- | --- |
| Mechanism | `llm_request` middleware rewrite | `UserPromptSubmit` hook context |
| Historical turns | Re-stamped from stored metadata | Each turn stamped live at submit time; earlier stamps persist in context |
| Message text | Prefixed API-only | Never touched (stamp travels as hook context) |
| Config | `~/.hermes/config.yaml` | `settings.json` hooks block |
| Dependencies | Python plugin | one `date` invocation |

The optional SOUL.md guidance from the main README works unchanged as a
`CLAUDE.md` snippet if you want the model to treat the stamp as ambient sense of
time rather than quotable metadata.
