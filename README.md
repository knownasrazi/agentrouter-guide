# AgentRouter Guide — FREE GPT-5.5 + Claude Opus 4.8 via One API

> **FREE Astra-Grade Access: $200 OG credits, no credit card — one API key for GPT-5.5, Claude Opus 4.8, GLM 5.2, DeepSeek R1, Gemini 2.0 Pro + 30 models. OpenAI-compatible.**
> **New GPT Model Free via AgentRouter — hype is real, credits are live. Claim via referral before pool resets.**

[![AgentRouter](https://img.shields.io/badge/AgentRouter-agentrouter.org-blue)](https://agentrouter.org/register?aff=v3aG)
[![FREE $200](https://img.shields.io/badge/FREE-$200_OG_credits-brightgreen)](https://agentrouter.org/register?aff=v3aG)
[![Models](https://img.shields.io/badge/models-30%2B-green)](#models)
[![GPT-5.5](https://img.shields.io/badge/GPT--5.5-Free_via_AgentRouter-ff6b35)](https://agentrouter.org/register?aff=v3aG)
[![Claude Opus](https://img.shields.io/badge/Claude_Opus_4.8-Free_via_AgentRouter-7c3aed)](#models)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](#license)

> ### My Referral — OG Credits (Viral: New GPT Free)
> **Register via my link to get OG ($200) credits instead of $100 — FREE access to new GPT + Claude:**
> **`https://agentrouter.org/register?aff=v3aG`**
> Standard = $100 | Referral = **$200 OG** — unlocks FREE GPT-5.5, Claude Opus 4.8, GLM, DeepSeek, Gemini. I get a referral bonus to keep this guide free.
> **Hype drop:** AgentRouter is the free Astra for the new GPT era — one key, no subscription, no card. Pool resets early (SG time), claim now.

<details>
<summary><strong>Why this is going viral</strong> — click to expand</summary>

- **FREE new GPT model** without OpenAI Pro/Max — via AgentRouter gateway
- **FREE Claude Opus 4.8** (1M context) without Anthropic Max — same key
- **FREE GLM 4.5 Air, DeepSeek V2 Lite** — $0 forever for routing/autocomplete
- **No credit card, GitHub OAuth only** — credits land instantly
- **One key, every AI** — Codex, OpenCode, Claude Code, Cursor, Cline, Roo, Aider, LangChain all work

> Built for the Astra hype cycle: new GPT drops -> everyone wants to try -> AgentRouter lets you try for $0.

</details>

**This repo is an independent, community-maintained guide.** Not affiliated with AgentRouter. Verified against live docs on `2026-09-12`.
**Viral kit:** See [`VIRAL.md`](./VIRAL.md) for tweet / Reddit / HN / Discord copy-paste templates to hype the free new GPT launch.

---

## What is AgentRouter?

**AgentRouter (`agentrouter.org`)** is an **OpenAI-compatible unified LLM gateway** launched Oct 2025 (China, Singapore infra). It proxies requests to upstream providers behind a single endpoint — you change only `baseURL` + `apiKey`, not your code.

```
Your App ──(OpenAI JSON over HTTPS)──▶ AgentRouter Gateway ──┬──▶ Anthropic (Claude)
   │   single key / single bill          • auth & routing     ├──▶ OpenAI (GPT-5.5)
   └─────────────────────────────────────• accounting         ├──▶ Zhipu (GLM 5.2)
                                         • queuing            ├──▶ DeepSeek / Gemini / Qwen ...
                                                              └──▶ +30 more
```

**Core promises:**
- **Unified Interface** — OpenAI ` /v1/chat/completions` + Anthropic ` /v1/messages` compatible. Swap models by changing one string.
- **Intelligent Routing** — auto load-balancing, failover, multi-provider redundancy (99.9% claimed).
- **Cost Optimization** — pay-per-token, no subscription, ~27% of public list price claimed, volume discounts, pay-as-you-go, balance never expires.
- **Enterprise Portal** — `co.agentrouter.org` provides admin console, SSO/RBAC, quotas, audit logs, data-governance controls (fine-grained provider routing).

> **One-line pitch from the homepage:** *“Better price, better stability, no subscription required, just replace the model BASE URL.”* — `agentrouter.org` · Built on **New API & One API**.

---

## Disambiguation — Which "AgentRouter"?

| Domain | What it actually is | Base URL | Relation to this guide |
|--------|---------------------|----------|------------------------|
| **`agentrouter.org`** | **LLM gateway (THIS GUIDE)** — New API fork | `https://agentrouter.org/v1` | **Primary subject** |
| **`co.agentrouter.org`** | Enterprise portal for same gateway (pricing, models, rankings, `portal/guide`) | `https://co.agentrouter.org` (Anthropic) / `.../v1` (OpenAI) | Same infra, richer docs — treated as canonical |
| `agent-router.org` | MCP marketplace for specialist agents (Tim Brauer) — `One MCP URL → 800+ agents` | `mcp.agent-router.org` | **Different product**, unrelated |
| `agentrouter.to` / `docs.agentrouter.to` | Capability-first gateway for *paid APIs* (email, search, web, etc.) with `recommend`/`execute` + wallet | `https://api.agentrouter.to` | **Different product**, see intro.quickstart there |

If you landed here looking for MCP agent delegation or `domains/capabilities/wallet` APIs — you want the other two domains. This guide covers the **LLM gateway** only.

---

## Quickstart (30 seconds)

### 1. Get a key — Use My OG Referral

- **My referral (use this): `https://agentrouter.org/register?aff=v3aG`** (GitHub OAuth, no credit card)
  - Gives **you $200 OG credits** (vs $100 standard) — I get a small referral bonus
  - `aff=v3aG` must be in URL *at registration* — can't be added later
- Copy key at `https://agentrouter.org/console/token` → `sk-...`
- Support the guide: share `https://agentrouter.org/register?aff=v3aG` — you get $200, I get +$100 to keep this guide updated

### 2. Test with curl (OpenAI-compatible)

```bash
curl https://agentrouter.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-YOUR_KEY" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "messages": [{"role": "user", "content": "Say hello in one sentence"}],
    "max_tokens": 50
  }'
```

### 3. Use any OpenAI SDK — just swap `baseURL`

```python
from openai import OpenAI
client = OpenAI(api_key="sk-YOUR_KEY", base_url="https://agentrouter.org/v1")
resp = client.chat.completions.create(
    model="claude-sonnet-4-5-20250929",
    messages=[{"role": "user", "content": "Write a haiku about APIs"}]
)
print(resp.choices[0].message.content)
```

```typescript
import OpenAI from "openai";
const client = new OpenAI({ apiKey: process.env.AGENTROUTER_API_KEY!, baseURL: "https://agentrouter.org/v1" });
const r = await client.chat.completions.create({ model: "gpt-5.5", messages: [{role:"user", content:"ping"}] });
console.log(r.choices[0].message.content);
```

**Anthropic-compatible (Claude Code, etc.):** base URL is **`https://co.agentrouter.org` or `https://agentrouter.org`** **without** `/v1`.
**OpenAI-compatible (everything else):** base URL **must** end with `/v1`.

---

## Navigation

| Doc | What’s inside |
|-----|---------------|
| **[GUIDE.md](./GUIDE.md)** | Single-file definitive guide (recommended read) |
| [`docs/01-introduction.md`](./docs/01-introduction.md) | Philosophy, who should/shouldn’t use it |
| [`docs/02-architecture.md`](./docs/02-architecture.md) | How the thin-proxy gateway works, latency, reliability |
| [`docs/03-models-pricing.md`](./docs/03-models-pricing.md) | Catalog, pricing tiers, credit burn modeling |
| [`docs/04-quickstart.md`](./docs/04-quickstart.md) | Registration → key → first call, step-by-step |
| [`docs/05-api-reference.md`](./docs/05-api-reference.md) | Endpoints, auth, streaming, errors |
| [`docs/06-integrations.md`](./docs/06-integrations.md) | Every client: Claude Code, Codex, OpenCode, Cursor, Cline, Roo, Kilo, Copilot, Trae, Craft… |
| [`docs/07-examples.md`](./docs/07-examples.md) | Code review bot, benchmark harness, fallback pattern |
| [`docs/08-troubleshooting.md`](./docs/08-troubleshooting.md) | 401/404/402, quota exhausted, GitHub auth issues |
| [`docs/09-providers.md`](./docs/09-providers.md) | All providers (Anthropic/OpenAI/GLM/DeepSeek/Gemini/Qwen/Mistral) for Codex, OpenCode, every AI |
| [`VIRAL.md`](./VIRAL.md) | Viral kit — tweet / Reddit / HN / Discord templates for free new GPT hype |

---

## Models

Portal highlights (`co.agentrouter.org/portal/models`):

| Model | Provider | Context | Input / 1M | Output / 1M | Best for |
|-------|----------|---------|------------|-------------|----------|
| Claude Opus 4.8 | Anthropic | 1M | $8 | $40 | Deep agent, long-horizon tasks |
| Claude Opus 4.7 | Anthropic | 1M | $8 | $40 | Async agents, large codebase |
| Claude Opus 4.6 | Anthropic | 1M | $2 | $10 | Stable enterprise default |
| GPT-5.5 | OpenAI | 1M (922K in / 128K out) | $4 | $8 | Reasoning, coding, multimodal |
| GLM 5.2 | Zhipu | 1M | $3 | $4.5 | Cost-effective engineering |

+ via gateway: `claude-sonnet-4-5-20250929`, `claude-haiku-4-5-*,` `deepseek-r1`, `gemini-2.0-pro`, `glm-4.5-air` (free), `qwen3-coder-480b`, etc. Full list → `docs/03-models-pricing.md`.

---

## Pricing at a glance

- **Free credits (OG via my referral):** **`https://agentrouter.org/register?aff=v3aG` → $200 OG** (vs $100 standard) — no expiry quoted, GitHub OAuth only (accounts <1 yr may fail). You get $200, I get +$100 referral bonus.
- **Pay-as-you-go:** no weekly caps, no minimum, balance never expires
- **Enterprise:** custom quote (`neo@agentrouter.org`), volume discounts, multi-tenant admin, SSO/RBAC, audit logs
- **Claimed edge:** ~27% of public API list price for equivalent usage (portal)

> **Heads-up from community reports:** even with credits remaining you may hit `402 Budget pool quota has been exhausted` — a per-model global rate limit. Fix: switch model or retry later. See troubleshooting.

---

## Supported endpoints (both gateways)

From `agentrouter.org` homepage:

```
/v1/chat/completions   /v1/responses       /v1/messages          /v1beta/models
/v1/embeddings         /v1/rerank          /v1/images/generations  /v1/images/edits
/v1/images/variations  /v1/audio/speech    /v1/audio/transcriptions /v1/audio/translations
```

All are OpenAI-shape passthroughs.

---

## Integrations (15+ verified)

**VS Code:** Claude Code for VS Code, Cline, Roo Code, Kilo Code, GitHub Copilot
**CLI/TUI:** Claude Code CLI, Codex, opencode, Qwen Code, Crush, Hermes Agent
**Desktop:** Claude App (Gateway), Trae, Cursor, Craft Agents
**Frameworks:** LangChain/LangGraph, LlamaIndex, Continue.dev, n8n

One key works across all — quota is shared. See `docs/06-integrations.md` for copy-paste configs.

---

## When NOT to use AgentRouter

- Production systems needing an SLA / incident response
- HIPAA/GDPR/regulated data (no DPA publié, proxy sees prompts)
- Ultra-low latency (<50ms) — Singapore hop adds 80–150ms to US/EU
- You need a web chat UI — gateway is API-only

---

## FAQ

**Anthropic vs OpenAI base URL?**
Anthropic (Claude): `https://co.agentrouter.org` (no `/v1`). OpenAI (GPT etc.): `https://co.agentrouter.org/v1` or `https://agentrouter.org/v1` — don’t mix.

**One key for multiple agents?**
Yes. Cline + Roo + Claude Code can share the same `sk-...`; usage is unified.

**Do I modify source code?**
No. Only `base_url`/`baseURL` + `api_key`.

**GitHub auth fails?**
New GitHub accounts (<1 yr) are often rejected. Use older account or try `gorouter.app` mirrors.

---

## Contributing

PRs welcome. Keep edits factual and cite the portal or live test. Run `npm run lint` if you add code samples.

---

## License

MIT — this guide is community content. AgentRouter branding belongs to its owners.

---

## Links

- **My OG referral:** https://agentrouter.org/register?aff=v3aG ← use this for $200 OG credits (supports this guide)
- Gateway: https://agentrouter.org · Console: https://agentrouter.org/console/token
- Enterprise portal: https://co.agentrouter.org · Docs: https://co.agentrouter.org/portal/guide
- Contact: `neo@agentrouter.org` · QQ Group · `x.com/AgentRouter_0` · Discord `discord.gg/WcwGwcHaq`

> *Built with research on 2026-09-12. Prices/models change — verify at `/portal/models` and `/portal/pricing` before billing.*
