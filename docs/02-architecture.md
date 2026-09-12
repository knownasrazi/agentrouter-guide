# 02 — Architecture: How the Gateway Works

## Thin proxy, not a model host

```
Your App ── HTTPS + OpenAI JSON ──▶ AgentRouter Gateway ──┬──▶ Anthropic (Claude)
   │   Authorization: Bearer sk-...     Auth & routing     ├──▶ OpenAI (GPT)
   └────────────────────────────────── Accounting          ├──▶ Zhipu (GLM)
                                     Queuing              ├──▶ DeepSeek / Gemini
                                     Passthrough          └──▶ Qwen / Mistral / ...
                                     (New API / One API, Singapore)
```

- **No weight hosting** — routes to upstream providers; response quality = direct.
- **No fine-tune layer** — prompt/response passed as-is (privacy implication).
- **Model additions are upstream-driven** — new releases appear when provider does.

## Why this matters for latency & reliability

| Factor | Detail |
|--------|--------|
| **Location** | Primary infra Singapore (`region: sgp` seen in WAF token) |
| **Latency add** | ~80–150ms US East, ~100–140ms EU, ~20–60ms SEA |
| **Mitigation** | Use `stream: true` (SSE) to mask TTFB; batch/async unaffected |
| **Reliability** | Claimed 99.9% via multi-channel failover — but **no public SLA**; community reports occasional `402 pool exhausted` even with balance |
| **Failover scope** | Per-model channel redundancy (e.g., multiple Claude backends), not cross-provider model substitution |

## Endpoints (gateway homepage)

All are OpenAI-shape passthroughs:

```
/v1/chat/completions   /v1/responses       /v1/messages
/v1beta/models         /v1/embeddings      /v1/rerank
/v1/images/generations /v1/images/edits    /v1/images/variations
/v1/audio/speech       /v1/audio/transcriptions /v1/audio/translations
```

## Two base URLs (critical)

| Surface | Base URL | Callers |
|---------|----------|---------|
| OpenAI-compatible | `https://agentrouter.org/v1` or `https://co.agentrouter.org/v1` | OpenAI SDK, Cursor, Codex, opencode, n8n |
| Anthropic-compatible | `https://co.agentrouter.org` or `https://agentrouter.org` (no `/v1`) | Claude Code CLI/VS Code, Roo (Anthropic), Trae (Anthropic), Copilot messages |

Mixing = 404. This is the #1 integration bug.

## Enterprise portal overlay

`co.agentrouter.org` adds: tenant management, per-member quotas, SSO/RBAC, audit logs, usage analytics, transparent versioning, data-governance routing rules.

Next → [03 Models & Pricing](./03-models-pricing.md)
