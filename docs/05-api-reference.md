# 05 — API Reference (LLM Gateway)

> Base URLs verified at `co.agentrouter.org/portal/guide` + `agentrouter.org` homepage. Test with `curl` before SDK integration.

## Base URLs

| Protocol | Base URL | When |
|----------|----------|------|
| **OpenAI-compatible** | `https://agentrouter.org/v1` or `https://co.agentrouter.org/v1` | `POST /v1/chat/completions`, embeddings, images, audio… |
| **Anthropic-compatible** | `https://co.agentrouter.org` or `https://agentrouter.org` (**no** `/v1`) | Claude Code, `/v1/messages`, gateway mode |

**Do not mix.** Anthropic with `/v1` → 404. OpenAI without `/v1` → 404/incompatible.

## Auth

```http
Authorization: Bearer sk-YOUR_AGENTROUTER_KEY
```

Env-var aliases (same key):
- `ANTHROPIC_AUTH_TOKEN` / `ANTHROPIC_API_KEY` (Anthropic surface)
- `OPENAI_API_KEY` / `AGENTROUTER_API_KEY` (OpenAI surface)

One key covers both; quota is shared.

## Endpoints

```
POST /v1/chat/completions
POST /v1/responses
POST /v1/messages
GET  /v1beta/models
POST /v1/embeddings
POST /v1/rerank
POST /v1/images/generations
POST /v1/images/edits
POST /v1/images/variations
POST /v1/audio/speech
POST /v1/audio/transcriptions
POST /v1/audio/translations
```

All passthrough, OpenAI shape. Support `stream: true` (SSE).

## Chat completions — request

```json
{
  "model": "claude-sonnet-4-5-20250929",
  "messages": [
    {"role": "system", "content": "You are a senior engineer."},
    {"role": "user", "content": "Explain CAP theorem succinctly"}
  ],
  "temperature": 0.5,
  "max_tokens": 800,
  "stream": false
}
```

## Chat completions — response

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "model": "claude-sonnet-4-5-20250929",
  "choices": [{
    "index": 0,
    "message": {"role": "assistant", "content": "..."},
    "finish_reason": "stop"
  }],
  "usage": {"prompt_tokens": 14, "completion_tokens": 17, "total_tokens": 31}
}
```

## Examples

```bash
# Non-streaming (OpenAI surface)
curl https://agentrouter.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
  -d '{"model":"claude-sonnet-4-5-20250929","messages":[{"role":"user","content":"Say hello"}],"max_tokens":100}'

# Streaming
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-4o","stream":true,"messages":[{"role":"user","content":"Write a haiku about APIs."}]}'

# Anthropic surface (Claude Code style)
curl https://co.agentrouter.org/v1/messages \
  -H "x-api-key: $AGENTROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"claude-opus-4-8","max_tokens":100,"messages":[{"role":"user","content":"ping"}]}'
```

## Errors

| Code | Body hint | Fix |
|------|-----------|-----|
| 401 | `Unauthorized` | Check key, `Bearer ` prefix, not expired |
| 404 | `Not Found` / `model not found` | Fix base URL `/v1` presence; verify model ID at `/portal/models` |
| 402 | `Budget pool quota has been exhausted` | Global per-model cap hit — switch model or retry later (not your balance) |
| 429 / timeout | Rate / peak | Backoff, retry, chunk request, try off-peak (EU night) |

Next → [06 Integrations](./06-integrations.md)
