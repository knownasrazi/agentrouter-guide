# AgentRouter — The Definitive Guide

> **AgentRouter (`agentrouter.org`) — Unified LLM API Gateway**
> Enterprise portal: `co.agentrouter.org` · Gateway built on **New API & One API**
> *Last verified: 2026-09-12. Independent guide, not affiliated.*
>
> ### My Referral — `aff=v3aG` (OG Credits)
> **Use my link for OG credits: `https://agentrouter.org/register?aff=v3aG`**
> Standard signup = $100 → Referral = $200 OG — you get OG credits, I get a referral bonus. Thanks for supporting this guide! `aff=v3aG` must be in URL at signup.

---

## Table of Contents

1. [What Is AgentRouter?](#1-what-is-agentrouter)
2. [What It Is NOT (Disambiguation)](#2-what-it-is-not-disambiguation)
3. [Architecture & How It Works](#3-architecture--how-it-works)
4. [Models & Pricing](#4-models--pricing)
5. [Quickstart: 0 → First Call in 3 Minutes](#5-quickstart-0--first-call-in-3-minutes)
6. [API Reference](#6-api-reference)
7. [Integrations — Every Agent & IDE](#7-integrations--every-agent--ide)
8. [Practical Code Examples](#8-practical-code-examples)
9. [Cost Modeling & Optimization](#9-cost-modeling--optimization)
10. [AgentRouter vs Alternatives](#10-agentrouter-vs-alternatives)
11. [Security, Privacy & Data Governance](#11-security-privacy--data-governance)
12. [Limitations, Risks & Troubleshooting](#12-limitations-risks--troubleshooting)
13. [Who Should / Shouldn’t Use It](#13-who-should--shouldnt-use-it)
14. [FAQ](#14-faq)
15. [Sources & Verification](#15-sources--verification)

---

## 1. What Is AgentRouter?

**AgentRouter is an OpenAI-compatible, non-profit LLM gateway** that aggregates dozens of commercial and open-source models behind **one base URL + one API key**.

Launch: **Oct 2025**, China-origin, Singapore-hosted (primary), inspired by OpenRouter’s architecture.

**Tagline (portal, `co.agentrouter.org`):**
> *“Enterprise-Grade AI Unified Routing Platform — fast, convenient Web API for enterprise developers, cost-effectively integrating the world’s leading AI models. One API for every major LLM.”*
> *（致力于为企业研发人员提供快速、便捷的Web API接口调用方案）*

**Tagline (gateway, `agentrouter.org`):**
> *“Better price, better stability, no subscription required, just replace the model BASE URL.”*

### Core value

| Without AgentRouter | With AgentRouter |
|---------------------|------------------|
| 1 key per provider (Anthropic, OpenAI, Zhipu…) | 1 key total |
| 1 bill per provider | 1 credit balance |
| Change SDK/base URL to switch provider | Change `model` string only |
| $20–$100/mo subscriptions to try top models | $0 upfront ($100–$200 free credits) |
| Separate quotas, dashboards, rate limits | Unified quota + routing |

### Four pillars (portal)

1. **Unified Interface** — compatible with OpenAI & Anthropic SDKs, all major agent frameworks, “zero migration cost.”
2. **Intelligent Routing** — auto load balancing, failover, multi-provider redundancy → claimed 99.9% availability.
3. **Cost Optimization** — “intelligently pick best price-performance model”, real-time cost monitoring, pay-as-you-go, no subscription, monthly volume discounts.
4. **Data Governance** — enterprise policies, fine-grained control of which provider receives your prompts (compliance).

### Stats claimed (portal, live)

- 15+ models · 10M+ daily calls · 50+ enterprises · 200+ active agents · 50M+ weekly calls (rankings page) · 1,200+ active agents

---

## 2. What It Is NOT (Disambiguation)

The name “AgentRouter” is reused by 3 unrelated projects. Don’t confuse them.

| Property | Domain | Purpose | Base URL / Install |
|----------|--------|---------|---------------------|
| **LLM Gateway — THIS GUIDE** | `agentrouter.org` + `co.agentrouter.org` | Proxy any LLM (Claude, GPT, GLM…) via OpenAI shape | `https://agentrouter.org/v1` (OpenAI) or `https://co.agentrouter.org` (Anthropic) |
| Enterprise portal (same gateway) | `co.agentrouter.org/portal/*` | Docs, models, rankings, pricing, admin console | Same keys as above |
| MCP agent marketplace | `www.agent-router.org` (note hyphen) | “One MCP URL → 800+ specialist agents” (research, code, browsing…) built by Tim Brauer | `https://mcp.agent-router.org` / `npx add-mcp ...` |
| Capability gateway for paid APIs | `agentrouter.to` & `docs.agentrouter.to` | Capability-first gateway: `GET /domains`, `POST /recommend`, `POST /execute`, `GET /wallet` for email/search/web/finance/… with x402/MPP rails | `https://api.agentrouter.to/api/agentic-api` |

> If your use case is “call Claude/GPT via one key in Claude Code/Cursor” → you want `agentrouter.org`. If it’s “delegate to specialist agents via MCP” → `agent-router.org`. If it’s “discover/execute paid API capabilities via wallet” → `agentrouter.to`.

This guide covers **only the LLM gateway**.

---

## 3. Architecture & How It Works

### Thin proxy, not a model host

AgentRouter **does not host weights**. It authenticates, maps, meters, queues, and forwards.

```
Your Application
      │
      │ HTTPS + OpenAI-compatible JSON
      │ Authorization: Bearer sk-...
      ▼
┌─────────────────────────┐
│     AgentRouter         │  New API / One API fork
│   Gateway Layer         │  (Singapore)
│  • Auth & key routing  │
│  • Model name mapping  │
│  • Credit accounting   │
│  • Request queuing     │
│  • Response passthrough│
└──────────┬──────────────┘
           │
     ┌─────┼──────┬──────────┐
     ▼     ▼      ▼          ▼
 Anthropic OpenAI Zhipu   DeepSeek / Gemini / Qwen / ...
  Claude   GPT    GLM
```

- **Response quality identical** to direct provider (no fine-tune).
- **New versions appear** when upstream releases them.
- **Latency overhead:** +80–150ms US East, +100–140ms EU, ~20–60ms SEA (Singapore origin). Use streaming to mask.
- **Endpoints exposed** (homepage): `/v1/chat/completions`, `/v1/responses`, `/v1/messages`, `/v1beta/models`, `/v1/embeddings`, `/v1/rerank`, `/v1/images/*`, `/v1/audio/*` — all passthrough.

### Intelligent routing vs. capability gateway

The LLM gateway’s “intelligent routing” = **per-request load balancer + failover across equivalent provider channels** for the *same* model family (e.g., multiple Claude backends). This is distinct from `agentrouter.to`’s `recommend` route selection across *different* providers.

### Enterprise portal extras (`co.agentrouter.org`)

- Admin console: tenants, seats, per-member quotas, audit logs, usage analytics
- SSO / RBAC (admin vs employee)
- Transparent model versioning, multi-channel failover
- Data-governance rules (route control)

---

## 4. Models & Pricing

### 4.1 Catalog (portal-verified on 2026-09-12)

Portal (`/portal/models`) lists 5 featured, filtered by Provider / Capability / Scenario:

| Model | Provider | Strengths | Context | Input / 1M | Output / 1M |
|-------|----------|-----------|---------|------------|-------------|
| **Claude Opus 4.8** | Anthropic | Strongest Opus, 1M ctx, agentic + memory, multi-step reasoning | 1M | **$8** | **$40** |
| **Claude Opus 4.7** | Anthropic | Async-agent tuned, large-codebase, multi-phase debugging | 1M | $8 | $40 |
| **Claude Opus 4.6** | Anthropic | Flagship Feb 2026, adaptive reasoning, enterprise stable | 1M | **$2** | **$10** |
| **GPT-5.5** | OpenAI | 922K in / 128K out, strong reasoning, multimodal | 1M | $4 | $8 |
| **GLM 5.2** | Zhipu (MoE) | Sparse MoE, lossless 1M, engineering/coding value | 1M | $3 | $4.5 |

**Gateway-wide (community & gist verified) additional IDs:**

- `claude-sonnet-4-5-20250929` (recommended for Claude Code), `claude-sonnet-4-5-20250514`, `claude-haiku-4-5-20251001`, `claude-3-5-haiku-20241022`, `claude-opus-4-5-20250929`
- `gpt-5`, `gpt-4o`, `gpt-4o-mini`, `gpt-3.5-turbo`
- `gemini-2.0-pro`, `gemini-1.5-flash`, `gemini-3-pro`
- `deepseek-r1`, `deepseek-v2-lite`, `deepseek-coder-v2-lite`
- `glm-4.5`, `glm-4.5-air`, `qwen3-coder-480b`, `qwen2-7b-instruct`, `mistral-7b-instruct`

Portal notes: *Need Gemini, DeepSeek, Llama? Enterprise plan custom-adds them* (`Contact Sales`).

### 4.2 Pricing tiers (synthesized from portal + gists)

| Tier | Price range | Example models | When to use |
|------|-------------|----------------|-------------|
| Free (0 credit) | $0 | `glm-4.5-air`, `glm-4.6`, `deepseek-v2-lite` | Classification, routing, autocomplete |
| Efficient | ~$0.07–$1.50 / 1M | Haiku 3.5 ($0.25/$1.25), GPT-3.5, Gemini Flash, DeepSeek Coder Lite | High-frequency tool calls, summarization |
| Balanced | ~$2–$15 / 1M | Sonnet 4.5 ($3/$15), GPT-4o ($2.5/$10), Gemini 2.0 Pro | Production coding, agentic tasks |
| Premium | ~$15–$75 / 1M | Opus 4.5 ($15/$75), GPT-5 ($10/$30), Gemini 3 Pro | Long-horizon, 150-page doc analysis |

> **Tip:** `deepseek-r1` (~$0.55/$2.19) gives premium reasoning at near-free cost for math/STEM.

### 4.3 Credits & billing — My OG Referral (`aff=v3aG`)

- **My referral (use this): `https://agentrouter.org/register?aff=v3aG` → $200 OG** (vs $100 standard). Some promos quote $175/$225/$300 by tier — `v3aG` is **$200 for you + ~$100 bonus to me**. No credit card, GitHub OAuth only, credited immediately.
- **How OG credits work:** You sign up via `aff=v3aG` → you get **$200** instantly, I get **+$100** referral bonus to support this guide. Share `https://agentrouter.org/register?aff=v3aG` with friends — everyone gets OG credits.
- **Pay-as-you-go:** only for tokens consumed, no minima, **balance never expires** (portal). No weekly limits (gateway claims advantage vs. official Claude Pro/Anthropic weekly caps).
- **Enterprise:** custom contract: sits per-seat, monthly quota, full model family, SSO/RBAC, dedicated lines, corporate billing. Contact `neo@agentrouter.org`.
- **Portal claim:** “Roughly **27% of public API list price** for equivalent usage.”
- **Credits vs real dollars:** spend is metered in credits; community reports show ~$20 consumed for ~362 requests (mixed Sonnet/GPT), so $200 ≈ weeks–months for prototyping (see §9).

---

## 5. Quickstart: 0 → First Call in 3 Minutes

### Step 1 — Register (<1 min) — MY OG REFERRAL LINK

1. **Use my OG link: `https://agentrouter.org/register?aff=v3aG`** (plain `/register` = only $100 — don’t use it).
2. **Sign in with GitHub** → authorize OAuth.
   - Note: Accounts <1 year old often rejected (`GitHub account too new`). Use an older GitHub account.
3. Verify balance shows **$200 OG** in console at `https://agentrouter.org/console/token`. If it shows $100, you missed `?aff=v3aG` — contact support or re-register with the correct link. Support this guide by sharing `aff=v3aG` — you get $200, I get +$100 bonus.

### Step 2 — Generate API key

- Visit `https://agentrouter.org/console/token` → **Generate New Token** → copy `sk-...` immediately.
- Enterprise portal alternative: `https://co.agentrouter.org/portal/contact` → “Get API Key”.

Store safely:

```bash
# .env (never commit)
AGENTROUTER_API_KEY=sk-your-key-here

# macOS Keychain
security add-generic-password -a "$USER" -s "agentrouter" -w "sk-your-key-here"
```

### Step 3 — Verify connectivity (free model, $0 cost)

```bash
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer sk-your-key-here" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "glm-4.5-air",
    "messages": [{"role": "user", "content": "ping"}],
    "max_tokens": 5
  }'
```

Expected: `200` with `choices[0].message.content`.

### Step 4 — Choose your surface

| If you are… | Start at |
|-------------|----------|
| Coding in terminal | §7 Claude Code CLI |
| In VS Code daily | §7 Continue.dev / Cline |
| Building a Python app | §8 Python SDK |
| Building a Node app | §8 Node SDK |
| Automating | §8 n8n HTTP node |

### Step 5 — Graduate from free → premium

```python
# Phase 1: validate (free)
model = "glm-4.5-air"
# Phase 2: test quality (cents)
model = "gpt-4o-mini"
# Phase 3: production
model = "claude-sonnet-4-5-20250929"
```

---

## 6. API Reference

### 6.1 Base URLs (critical)

| Protocol | Base URL | Example caller |
|----------|----------|----------------|
| **OpenAI-compatible** (Chat, Embeddings, Images, Audio, Rerank) | `https://agentrouter.org/v1` **or** `https://co.agentrouter.org/v1` | OpenAI SDK, Cursor, Codex, opencode, n8n |
| **Anthropic-compatible** (Messages API, Claude Code) | `https://co.agentrouter.org` **or** `https://agentrouter.org` (**without** `/v1`) | Claude Code CLI/VS Code, Roo Code (Anthropic mode), Trae (Anthropic) |

**Mixing them = 404.** Anthropic with `/v1` fails; OpenAI without `/v1` fails.

### 6.2 Authentication

```
Authorization: Bearer sk-YOUR_AGENTROUTER_KEY
```

Or provider-specific env vars mapped to the same key:

- `ANTHROPIC_AUTH_TOKEN` / `ANTHROPIC_API_KEY` for Anthropic surface
- `OPENAI_API_KEY` for OpenAI surface

One key works for both surfaces; quota is shared.

### 6.3 Endpoints (homepage list)

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

All are OpenAI-shape. Use `stream: true` for SSE streaming.

### 6.4 Request / response shape (chat completions)

**Request:**
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

**Response:**
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

**Streaming curl:**
```bash
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer sk-..." -H "Content-Type: application/json" \
  -d '{"model":"gpt-4o","stream":true,"messages":[{"role":"user","content":"Write a haiku"}]}'
```

### 6.5 Errors

| Code | Cause | Fix |
|------|-------|-----|
| `401` | Wrong key or missing `Bearer ` | Regenerate at `/console/token`, check header |
| `404 Not Found` | Wrong base URL (missing/extra `/v1`) or wrong model ID | See §6.1; verify model at `/portal/models` or try `gpt-4o` |
| `402 Budget pool quota exhausted` | Global per-model quota hit (not your balance) | Switch model (e.g., Sonnet→Haiku) or retry later; community’s #1 complaint |
| Timeout / reset | Long request, peak traffic | Increase `timeout`, chunk, or retry with backoff |

Portal note: enterprise multi-channel failover handles retries automatically for covered models.

---

## 7. Integrations — Every Agent & IDE

> One key, all agents. Full step-by-step with screenshots lives at `https://co.agentrouter.org/portal/guide`. Below are copy-paste configs.

### 7.1 General rule

- **Claude family (Opus/Sonnet/Haiku)** → pick **Anthropic** provider → base URL **no** `/v1`
- **GPT / GLM / generic** → pick **OpenAI Compatible** → base URL **with** `/v1`
- Don’t mix in one profile.

### 7.2 VS Code — Claude Code for VS Code (Anthropic)

`settings.json`:
```json
{
  "claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_AUTH_TOKEN", "value": "<AgentRouter API Key>" },
    { "name": "ANTHROPIC_BASE_URL",  "value": "https://co.agentrouter.org" },
    { "name": "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY", "value": "1" },
    { "name": "ANTHROPIC_MODEL",     "value": "claude-opus-4-7" }
  ],
  "claudeCode.disableLoginPrompt": true,
  "claudeCode.initialPermissionMode": "acceptEdits"
}
```
Reload window → open Claude Code panel → send any message.

### 7.3 Cline (VS Code)

- **Anthropic path (recommended):** `API Provider: Anthropic`, `Use custom base URL: ✓`, `Custom Base URL: https://co.agentrouter.org`, `API Key: sk-...`, `Model: claude-opus-4-8`
- **OpenAI path:** `API Provider: OpenAI Compatible`, `Base URL: https://co.agentrouter.org/v1`, `Model: gpt-5.5 / kimi-k2.6 / glm-5.1`

### 7.4 Roo Code (VS Code)

New Profile → `Anthropic + Use custom URL` → `https://co.agentrouter.org` → `claude-opus-4-6`
Or `OpenAI Compatible` → `https://co.agentrouter.org/v1` → `gpt-5.5`.

### 7.5 Kilo Code (VS Code)

`Setting → Providers → Custom Provider → Connect`:
`Provider ID: agentrouter`, `Display Name: AgentRouter`, `Base URL: https://co.agentrouter.org/v1`, key, models `claude-opus-4-8 / gpt-5.5`.

### 7.6 GitHub Copilot (VS Code)

`Copilot Chat → Manage Models… → Add Models → Custom Endpoint` → add `Group: AgentRouter`.

`models.json`:
```json
[
  {
    "name": "AgentRouter-Claude",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.xxx}",
    "apiType": "messages",
    "models": [{"id": "claude-opus-4-8","name": "Claude Opus 4.8","url": "https://co.agentrouter.org","toolCalling": true,"vision": true,"maxInputTokens": 922000,"maxOutputTokens": 128000}]
  },
  {
    "name": "AgentRouter-OpenAI",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.xxx}",
    "apiType": "chat-completions",
    "models": [{"id": "gpt-5.5","name": "GPT-5.5","url": "https://co.agentrouter.org/v1","toolCalling": true,"vision": true,"maxInputTokens": 922000,"maxOutputTokens": 128000}]
  }
]
```

### 7.7 Claude Code CLI (Anthropic, terminal)

```bash
npm i -g @anthropic-ai/claude-code@latest
# macOS/Linux — persist in ~/.zshrc or ~/.bashrc
export ANTHROPIC_AUTH_TOKEN="<AgentRouter API Key>"
export ANTHROPIC_BASE_URL="https://co.agentrouter.org"
export ANTHROPIC_MODEL="claude-opus-4-8"
claude   # launch in project root

# Windows PowerShell
$env:ANTHROPIC_AUTH_TOKEN="<AgentRouter API Key>"
$env:ANTHROPIC_BASE_URL="https://co.agentrouter.org"
$env:ANTHROPIC_MODEL="claude-opus-4-8"
claude
```

### 7.8 Codex (OpenAI)

`~/.codex/config.toml`:
```toml
model = "gpt-5.5"
model_provider = "agentrouter"
[model_providers.agentrouter]
name = "AgentRouter"
base_url = "https://co.agentrouter.org/v1"
env_key = "AGENTROUTER_API_KEY"
wire_api = "chat"
```
```bash
export AGENTROUTER_API_KEY="<AgentRouter API Key>"
codex "Please reply OK to verify connectivity"
```

### 7.9 opencode (TUI)

`opencode.json` in project root:
```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "agentrouter": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "AgentRouter",
      "options": { "baseURL": "https://co.agentrouter.org/v1" },
      "models": { "step3p5-code-alpha": { "name": "" } }
    }
  },
  "model": "agentrouter/step3p5-code-alpha"
}
```
```bash
opencode providers login --provider agentrouter
opencode
```

### 7.10 Qwen Code

```bash
npm i -g @qwen-code/qwen-code@latest
export OPENAI_API_KEY="<AgentRouter API Key>"
export OPENAI_BASE_URL="https://co.agentrouter.org/v1"
export OPENAI_MODEL="gpt-5.5"
qwen
```

### 7.11 Crush

`~/.config/crush/crush.json`:
```json
{
  "$schema": "https://charm.land/crush.json",
  "providers": {
    "agentrouter": {
      "type": "openai-compat",
      "base_url": "https://co.agentrouter.org/v1",
      "api_key": "$AGENTROUTER_API_KEY",
      "models": [{"id": "step3p5-code-alpha","name": "step3p5-code-alpha","context_window": 200000,"default_max_tokens": 8192}]
    }
  }
}
```

### 7.12 Hermes Agent (Nous)

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup model   # Provider: openai-api, Base URL: https://co.agentrouter.org/v1
hermes chat
```

### 7.13 Claude App (Desktop)

`Help → Troubleshooting → Enable developer mode` → `Developer → Configure third-party inference`:
`Gateway base URL: https://co.agentrouter.org`, `API key: sk-...`, `Auth scheme: bearer` → `Apply locally → Relaunch now`.

### 7.14 Trae

`Settings → Models → Add Model → Custom Config`:
- Claude: `API Format: Anthropic Messages`, `Request URL: https://co.agentrouter.org`, `Model: claude-opus-4-8`
- Generic: `API Format: OpenAI Completions`, `Request URL: https://co.agentrouter.org/v1`, `Model: gpt-5.5`

### 7.15 Cursor

`Cursor Settings → Models → OpenAI API Key: sk-... (on)` → `Override OpenAI Base URL: https://co.agentrouter.org/v1` (or `https://agentrouter.org/v1`). Free Cursor users can’t pick model (stuck on `auto`).

### 7.16 Craft Agents

Welcome → `Use other provider` → `Custom`, `Endpoint: https://co.agentrouter.org` (Anthropic) or `https://co.agentrouter.org/v1` (OpenAI).

### 7.17 Continue.dev, LangChain, LlamaIndex, n8n

See §8 examples. All are “set `apiBase`/`base_url` to AgentRouter, keep model string”.

---

## 8. Practical Code Examples

### 8.1 Raw HTTP / curl

```bash
# Non-streaming
curl https://agentrouter.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
  -d '{"model":"claude-sonnet-4-5-20250929","messages":[{"role":"user","content":"Say hello"}],"max_tokens":100}'

# Streaming
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-4o","stream":true,"messages":[{"role":"user","content":"Write a haiku about APIs."}]}'
```

### 8.2 Python SDK (openai)

```python
from openai import OpenAI
import os
client = OpenAI(api_key=os.environ["AGENTROUTER_API_KEY"], base_url="https://agentrouter.org/v1")

# Basic
r = client.chat.completions.create(
    model="claude-sonnet-4-5-20250929",
    messages=[{"role":"system","content":"You are an expert architect."},{"role":"user","content":"Design a microservice for e-commerce"}],
    temperature=0.5, max_tokens=2000
)
print(r.choices[0].message.content)

# Streaming
with client.chat.completions.stream(model="gpt-4o", messages=[{"role":"user","content":"Explain WebSockets"}]) as stream:
    for chunk in stream:
        if chunk.choices[0].delta.content:
            print(chunk.choices[0].delta.content, end="", flush=True)

# JSON mode
import json
r = client.chat.completions.create(
    model="gpt-4o", response_format={"type":"json_object"},
    messages=[{"role":"system","content":"Return only valid JSON."},{"role":"user","content":"3 Python libs + use cases as JSON."}]
)
print(json.loads(r.choices[0].message.content))

# Model comparison helper
def compare(prompt, models):
    return {m: client.chat.completions.create(model=m, messages=[{"role":"user","content":prompt}], max_tokens=500).choices[0].message.content[:200] for m in models}
```

### 8.3 Node / TypeScript SDK

```typescript
import OpenAI from "openai";
const client = new OpenAI({ apiKey: process.env.AGENTROUTER_API_KEY!, baseURL: "https://agentrouter.org/v1" });

// Basic
export async function complete(prompt: string) {
  const r = await client.chat.completions.create({ model: "claude-sonnet-4-5-20250929", messages: [{role:"user", content: prompt}], max_tokens: 1000 });
  return r.choices[0].message.content;
}

// Next.js streaming route — app/api/chat/route.ts
export async function POST(req: Request) {
  const { message } = await req.json();
  const stream = await client.chat.completions.create({ model: "gpt-4o", stream: true, messages: [{role:"user", content: message}] });
  const encoder = new TextEncoder();
  return new Response(new ReadableStream({
    async start(controller) {
      for await (const chunk of stream) controller.enqueue(encoder.encode(chunk.choices[0]?.delta?.content ?? ""));
      controller.close();
    }
  }), { headers: {"Content-Type":"text/plain; charset=utf-8"} });
}

// Tool calling
const weatherTool: OpenAI.ChatCompletionTool = {
  type: "function",
  function: { name: "get_weather", description: "Fetches weather for a city", parameters: { type:"object", properties:{ city:{type:"string"}, unit:{type:"string", enum:["celsius","fahrenheit"]} }, required:["city"] } }
};
const r2 = await client.chat.completions.create({ model:"claude-sonnet-4-5-20250929", tools:[weatherTool], tool_choice:"auto", messages:[{role:"user", content:"Weather in Tokyo?"}] });
if (r2.choices[0].finish_reason==="tool_calls") console.log(JSON.parse(r2.choices[0].message.tool_calls![0].function.arguments));
```

### 8.4 LangChain / LangGraph

```python
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage, SystemMessage
llm = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
resp = llm.invoke([SystemMessage(content="You are a senior Python engineer."), HumanMessage(content="Refactor to async/await: ...")])
print(resp.content)

# Multi-agent graph — different models per node (planner=Opus, executor=Sonnet, reviewer=DeepSeek free)
from langgraph.graph import StateGraph, END
from typing import TypedDict
planner = ChatOpenAI(model="claude-opus-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
executor = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
reviewer = ChatOpenAI(model="deepseek-r1", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
class S(TypedDict): task:str; plan:str; result:str; review:str
g=StateGraph(S)
g.add_node("plan", lambda s: {"plan": planner.invoke(f"Plan: {s['task']}").content})
g.add_node("execute", lambda s: {"result": executor.invoke(f"Execute: {s['plan']}").content})
g.add_node("review", lambda s: {"review": reviewer.invoke(f"Review: {s['result']}").content})
g.set_entry_point("plan"); g.add_edge("plan","execute"); g.add_edge("execute","review"); g.add_edge("review", END)
app=g.compile()
print(app.invoke({"task":"Write a FastAPI CRUD for a blog"}))
```

### 8.5 LlamaIndex (RAG)

```python
from llama_index.llms.openai import OpenAI
from llama_index.core import Settings, VectorStoreIndex, SimpleDirectoryReader
Settings.llm = OpenAI(model="gpt-4o", api_key="sk-...", api_base="https://agentrouter.org/v1")
docs = SimpleDirectoryReader("./docs").load_data()
index = VectorStoreIndex.from_documents(docs)
print(index.as_query_engine().query("Key findings in Q3 report?"))
```

### 8.6 Continue.dev (VS Code autocomplete)

`~/.continue/config.json`:
```json
{
  "models": [
    {"title": "Claude Sonnet (AgentRouter)","provider": "openai","model": "claude-sonnet-4-5-20250929","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."},
    {"title": "DeepSeek R1 (Free)","provider": "openai","model": "deepseek-r1","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."}
  ],
  "tabAutocompleteModel": {"title": "GLM-4.5 Air (Free)","provider": "openai","model": "glm-4.5-air","apiBase": "https://agentrouter.org/v1","apiKey": "sk-..."}
}
```

### 8.7 n8n (HTTP Request node)

```
Method: POST
URL: https://agentrouter.org/v1/chat/completions
Header: Authorization: Bearer sk-...
Body: {"model":"claude-sonnet-4-5-20250929","messages":[{"role":"system","content":"You are an email classifier. Respond JSON only."},{"role":"user","content":"Classify: {{$json.email_body}}"}],"response_format":{"type":"json_object"}}
```

### 8.8 Code review bot (practical)

```python
import subprocess
from openai import OpenAI
client = OpenAI(api_key="sk-...", base_url="https://agentrouter.org/v1")
def get_diff(): return subprocess.run(["git","diff","--cached"], capture_output=True, text=True).stdout
def review(diff):
    return client.chat.completions.create(
        model="claude-sonnet-4-5-20250929",
        messages=[
            {"role":"system","content":"Senior engineer review. Sections: CRITICAL/MAJOR/MINOR/SUGGESTION."},
            {"role":"user","content":f"Review diff:\n```diff\n{diff}\n```"}
        ], max_tokens=2000
    ).choices[0].message.content
diff=get_diff()
print(review(diff) if diff else "No staged changes.")
```

### 8.9 Fallback pattern (recommended for prod)

```python
from openai import OpenAI, APIConnectionError
clients = {
    "primary": OpenAI(api_key="sk-agentrouter", base_url="https://agentrouter.org/v1"),
    "fallback": OpenAI(api_key="sk-anthropic-direct", base_url="https://api.anthropic.com/v1"),
}
def robust_complete(prompt):
    for name, c in clients.items():
        try:
            return c.chat.completions.create(model="claude-sonnet-4-5-20250929", messages=[{"role":"user","content":prompt}], timeout=30).choices[0].message.content
        except Exception as e:
            print(f"[{name}] failed: {e}")
    raise RuntimeError("All providers failed")
```

---

## 9. Cost Modeling & Optimization

### 9.1 3-tier selection (saves 60–90%)

```
Tier 1 — Routing/Classification (free)
  Model: glm-4.5-air, deepseek-v2-lite  Cost: $0
  Q: “Is this billing or technical?”

Tier 2 — Execution/Generation (cents)
  Model: haiku-3.5, gpt-4o-mini, gemini-flash
  Q: “Summarize this article in 3 sentences.”

Tier 3 — Complex reasoning (dollars)
  Model: sonnet-4.5, gpt-4o, deepseek-r1
  Q: “Architect this distributed system.”

Tier 4 — Mission-critical / long context
  Model: opus-4.8, gpt-5, gemini-3-pro
  Q: “Analyze this 150-page contract.”
```

### 9.2 Burn rates (community-reported, 2026)

| Activity | Daily cost | $200 lasts |
|----------|------------|------------|
| Sonnet 3.7, 50 msgs/day coding | ~$1.80 | ~111 days |
| GPT-4o drafting, 30 msgs/day | ~$0.60 | ~333 days |
| DeepSeek R1 pipelines, 200 req/day | ~$0.10 | ~2,000 days |
| Mixed prototyping | $3–$8 | 25–65 days |
| Batch embeddings 1M tok/day | $0.10–0.20 | 1k–2k days |

### 9.3 Optimizations

1. **Cache repeated prompts** (system + static context).
2. **Trim context** — summarize old turns; you pay for input tokens.
3. **Route simple queries to free models** — 3-way classifier via `glm-4.5-air` ($0) before spending $0.018/Sonnet call.
4. **Set `max_tokens`** explicitly (e.g., `400` for short answers halves output cost).
5. **Use streaming for UX, not saving** — doesn’t reduce tokens.

### 9.4 Calculator

```python
COSTS = {
    "glm-4.5-air": {"in":0.0,"out":0.0},
    "claude-haiku-3-5-20241022": {"in":0.00025,"out":0.00125},
    "gpt-4o-mini": {"in":0.00015,"out":0.0006},
    "claude-sonnet-4-5-20250929": {"in":0.003,"out":0.015},
    "gpt-4o": {"in":0.0025,"out":0.01},
    "claude-opus-4-5-20250929": {"in":0.015,"out":0.075},
}
def estimate(model, inp, out): c=COSTS[model]; return inp/1000*c["in"]+out/1000*c["out"]
for m in COSTS: print(m, f"${estimate(m,500,300)*1000:.2f}/day for 1k req")
```

---

## 10. AgentRouter vs Alternatives

### vs OpenRouter

|  | AgentRouter | OpenRouter |
|--|-------------|------------|
| Models | ~30–50 | 400+ |
| Platform fee | 0% (non-profit) | 5.5% on credits |
| Free credit | $100–$200 | ~$1 trial |
| SLA | None published | 99.9% available |
| Enterprise routing/fallbacks/A-B | Portal-only | Full suite |
| Best for | Students, indie hackers, prototyping | Production, enterprise |

### vs Direct APIs (OpenAI/Anthropic/Google)

|  | AgentRouter | Direct |
|--|-------------|--------|
| Keys | 1 | 1 per provider |
| Latency | +80–150ms (SG) | Regional baseline |
| Switch model | Change string | Change URL+SDK |
| Billing | Single | Separate invoices |
| Support | Community/email | Tiered enterprise |
| Privacy | Proxy sees prompts | Direct |

### vs Subscriptions (Claude Pro $20/mo, ChatGPT Plus)

|  | AgentRouter API | Subscription |
|--|-----------------|--------------|
| Providers | All | One |
| Programmatic | Yes | Limited |
| Cost | Pay-per-token | Flat monthly |
| UI | None | Yes |
| Ideal | Developer | Power user (chat) |

**Verdict:** For code/builder workflows AgentRouter wins on flexibility; for chat-only, subscriptions are simpler; for production SLA, use direct APIs (or OpenRouter Enterprise). Use AgentRouter as primary for dev + fallback for prod.

---

## 11. Security, Privacy & Data Governance

### What the proxy sees

Every prompt, system message, and response passes through AgentRouter’s gateway before reaching the upstream provider. Your AgentRouter API key is sent per request.

### Do NOT send via any proxy

- PII under GDPR/HIPAA/CCPA (health, SSN, financial)
- Trade secrets / regulated source code
- Passwords, private keys, OAuth tokens
- Classified / government-sensitive

### Generally safe

- Public docs, open-source code, marketing copy, general programming Q&A, prototyping logic.

### Best practices

```bash
# .env + .gitignore
echo ".env" >> .gitignore
export AGENTROUTER_API_KEY="sk-..."
# In code — never hardcode
import os
api_key = os.environ["AGENTROUTER_API_KEY"]  # fail-loud if missing
# In prod — use secrets manager (AWS Secrets Manager / Vault / Doppler)
```

Enterprise portal adds: per-member quotas, audit logs, fine-grained provider allowlists (e.g., “route EU prompts only to EU-compliant providers”).

---

## 12. Limitations, Risks & Troubleshooting

### Known limitations

- **No SLA** — non-profit, community/email support only, no incident commitments.
- **Singapore latency** — noticeable for interactive chat from US/EU; batch/async unaffected.
- **Docs are terse** — portal guide covers happy path but lacks advanced quirks; this guide + community threads fill the gap.
- **Sustainability undisclosed** — funding model not public; for primary production, keep a direct-provider fallback.

### Risk by use case

```
Experimentation / learning     → Low risk
Side project / prototype       → Low risk
Production secondary traffic   → Medium (keep fallback)
Primary production system      → High risk (prefer direct)
```

### Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `401 Unauthorized` | Wrong key / missing `Bearer ` | Re-issue at `/console/token` |
| `404 Not Found` | Extra/missing `/v1` | Anthropic **no** `/v1`, OpenAI **with** `/v1` |
| `model not found` | Bad model ID | Use exact ID from `/portal/models` or `gpt-4o` |
| `402 Budget pool quota exhausted` | Global per-model quota hit (most common) | Switch model (Opus→Sonnet→Haiku) or retry later; not your balance |
| Balance $0 after referral | Signed up without `?aff=` | Contact support; referral must be at registration |
| `GitHub auth error` | New GitHub (<1 yr) | Use older GitHub; mirrors like `gorouter.app` accept newer accounts but similar caps |
| `claude-code` still shows login | Missing `disableLoginPrompt` | Set `claudeCode.disableLoginPrompt: true` + Reload Window |
| `command not found: claude` | CLI not installed | `npm i -g @anthropic-ai/claude-code@latest` |
| Chat disabled / recharge disabled | Admin-side feature flag | Use API surface; console “chat” may be gated |
| Quota shows $175 but can’t use | Pool exhausted for that model | Try `glm-4.5-air` to verify key works; then try premium later |

### Community signals (gist comments, Aug–Sep 2026)

- **Positive:** “it actually works,” $200 credited instantly, 30M tokens over 3 days for $70-tier, model breadth.
- **Negative:** repeated `402` exhaustion, balance display flickers to $0, GitHub-new-account blocks, daily per-model caps even with credits remaining. Workaround universally cited: **swap provider/model or time-shift (EU night vs SG day).**

---

## 13. Who Should / Shouldn’t Use It

### Excellent fit

- Students/learners (risk-free experimentation)
- Solo/indie hackers (defer infra cost til PMF)
- Researchers (cross-model comparison, one bill)
- Agencies/freelancers (pay-per-use fits bursty volume)
- Hackathons (credits outlast a weekend)
- Educators (teach one API shape vs three)

### Poor fit

- Prod systems needing SLA → direct APIs
- Regulated data (HIPAA/GDPR) → direct with DPA
- Ultra-low latency (<50ms) → regional direct endpoints
- Need audit-grade logs → direct + own logging
- Need web UI → subscriptions

---

## 14. FAQ

**Q: Anthropic vs OpenAI base URL?**
A: Claude family: `https://co.agentrouter.org` (no `/v1`). GPT/GLM/generic: `https://co.agentrouter.org/v1` or `https://agentrouter.org/v1`.

**Q: One key for multiple agents?**
A: Yes. Same `sk-...` for Cline, Roo, Claude Code etc.; quota shared.

**Q: Need to change source code?**
A: No. Only env/config `base_url` + `api_key`.

**Q: How to switch model?**
A: Change `model` param. Per-provider suffix variants may exist for enterprise pools — prefer suffix if given.

**Q: How long do $200 last?**
A: See §9.2 — ~25 days of heavy mixed prototyping to 2,000 days of DeepSeek-lite pipelines.

**Q: Is it legit / scam?**
A: Live since Oct 2025, New API–based, thousands of community verifications. Non-profit “public welfare” stated. Use for dev/test; keep fallback for prod.

---

## 15. Sources & Verification

- **Live gateway:** https://agentrouter.org — endpoints list, “Designed & Developed with love by New API & One API”
- **Enterprise portal:** https://co.agentrouter.org — hero, 15+/10M+/50+/200+, unified/ routing / cost / governance pillars
- **Models:** https://co.agentrouter.org/portal/models — Claude Opus 4.8/4.7/4.6, GPT-5.5, GLM 5.2 + $8/$40 etc.
- **Rankings:** https://co.agentrouter.org/portal/rankings — 12.5M Opus 4.8 etc.
- **Pricing:** https://co.agentrouter.org/portal/pricing — $27% claim, pay-as-you-go, enterprise console
- **Full integration guide:** https://co.agentrouter.org/portal/guide — 15 clients, env vars, screenshots, FAQ
- **About/contact:** https://co.agentrouter.org/portal/about — mission, principles, `neo@agentrouter.org`, `x.com/AgentRouter_0`, `discord.gg/WcwGwcHaq`, QQ group
- **Community deep dives:** Gist `mzaman/a9409de6ccaa19044fb564936b8c9c4f` (Definitive Developer Guide, 30+ models, latency, tiers), Gist `zabih3/28d5e591884...` (Review 2026, $200, setup, comments 402 reports)
- **Contrast domains:** `www.agent-router.org` (MCP marketplace, unrelated), `docs.agentrouter.to/{welcome,quickstart,introduction}` (capability gateway, different product)

> *Check `/portal/models` and `/portal/pricing` before relying on price/availability — the gateway adds/removes models without versioned changelog.*

---

**Enjoy — and if this guide saved you time, support me via `https://agentrouter.org/register?aff=v3aG` and star the repo.**

**Share text (copy-paste):**
> *Get $200 OG credits on AgentRouter (vs $100 standard) — my referral: https://agentrouter.org/register?aff=v3aG — GitHub login, no card. Thanks for supporting this guide.*

* — Independent guide, MIT licensed. My referral: `aff=v3aG`.*
