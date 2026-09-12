# 03 — Models & Pricing

> Verified against `co.agentrouter.org/portal/models` + `/portal/pricing` + `/portal/rankings` on 2026-09-12. Prices change — always re-check portal.

## Featured catalog (portal) — UPDATED with GPT-6 Astra (Sep 2026)

| Model | Provider | Superpower | Context | Input / 1M | Output / 1M |
|-------|----------|------------|---------|------------|-------------|
| **GPT-6 Astra** | OpenAI | **NEW Sep 3 2026 — world's most intelligent, 1.05M ctx, reasoning.effort low..max, computer use** | **1.05M** | **$10** | **$50** |
| Claude Opus 4.8 | Anthropic | Strongest Opus, autonomous agent, memory | 1M | $8 | $40 |
| Claude Opus 4.7 | Anthropic | Async agent, large-codebase, multi-phase debug | 1M | $8 | $40 |
| Claude Opus 4.6 | Anthropic | Feb 2026 flagship, adaptive reasoning, stable | 1M | $2 | $10 |
| GPT-5.5 | OpenAI | 922K in / 128K out, reasoning, multimodal | 1M | $4 | $8 |
| GLM 5.2 | Zhipu (MoE) | Sparse MoE, lossless 1M, engineering value | 1M | $3 | $4.5 |

> **GPT-6 Astra via AgentRouter:** Use `model: "gpt-6-astra"` at `https://agentrouter.org/v1` with your $200 OG credits (`https://agentrouter.org/register?aff=v3aG`) — no separate OpenAI/Azure/Bedrock key needed. See full Astra guide at [`docs/10-gpt6-astra.md`](./10-gpt6-astra.md).

Portal filters: Provider (Anthropic/OpenAI/Zhipu), Capability (code/reasoning/agent/long-context/multimodal), Scenario (agent dev, code assist, data analysis, doc processing).

## Extended gateway catalog (community-verified)

IDs you can use via `agentrouter.org/v1` (not all on portal featured list):

- OpenAI: `gpt-6-astra` (**NEW — Astra, 1.05M ctx, $10/$50**), `gpt-5.5`, `gpt-5`, `gpt-4o`, `gpt-4o-mini`, `gpt-3.5-turbo`, `kimi-k2.6` (Moonshot via gateway)
- Claude: `claude-sonnet-4-5-20250929` (Claude Code recommended), `claude-sonnet-4-5-20250514`, `claude-haiku-4-5-20251001`, `claude-3-5-haiku-20241022`, `claude-opus-4-5-20250929`
- Google: `gemini-2.0-pro`, `gemini-1.5-flash`, `gemini-3-pro`
- DeepSeek: `deepseek-r1`, `deepseek-v2-lite`, `deepseek-coder-v2-lite`
- Zhipu/Alibaba/Mistral: `glm-4.5`, `glm-4.5-air` (free), `qwen3-coder-480b`, `qwen2-7b-instruct`, `mistral-7b-instruct`, `glm-5.1`, `step3p5-code-alpha`

> Portal note: “Need Gemini/DeepSeek/Llama? Enterprise can add on demand — Contact Sales.”

## Rankings (live)

| # | Model | Weekly calls | Trend |
|---|-------|--------------|-------|
| 1 | Claude Opus 4.8 | 12.5M | ↑ +20% |
| 2 | GPT-5.5 | 10.2M | ↑ +15% |
| 3 | Claude Opus 4.7 | 8.7M | → +2% |
| 4 | GLM 5.2 | 6.3M | ↑ +35% |
| 5 | Claude Opus 4.6 | 4.1M | ↓ -8% |
Total ~50M weekly, 1,200+ active agents, +18% WoW.

## Pricing tiers (synthesis)

| Tier | $/1M tokens | Models | Use |
|------|-------------|--------|-----|
| Free | $0 | `glm-4.5-air`, `glm-4.6`, `deepseek-v2-lite` | Routing, triage, autocomplete |
| Efficient | $0.07–$1.50 | Haiku 3.5 ($0.25/$1.25), GPT-3.5, Gemini Flash | High-freq tools, summarization |
| Balanced | $2–$15 | Sonnet 4.5 ($3/$15), GPT-4o ($2.5/$10), Gemini 2.0 Pro | Prod coding, agents |
| Premium | $15–$75 | Opus 4.5 ($15/$75), GPT-5 ($10/$30) | Long docs, frontier reasoning |

*Value tip:* `deepseek-r1` ~$0.55/$2.19 = premium reasoning at near-free.

## Billing — My OG Referral (`aff=v3aG`)

- **My referral: `https://agentrouter.org/register?aff=v3aG` → $200 OG** (vs $100 standard). You get $200, I get +$100 bonus to support this guide. `aff=v3aG` must be at signup.
- **Pay-as-you-go:** No weekly caps, no minimum, balance never expires (portal). No subscription fees.
- **Enterprise:** Custom quote via `neo@agentrouter.org` — per-seat monthly quota, full model access, SSO/RBAC, dedicated lines, corporate billing.
- **Claimed edge:** “~27% of public API list price for equivalent usage.”
- **Real burn (community):** ~$20 per ~362 requests (mixed Sonnet/GPT). So $200 ≈ 25 days heavy prototyping → 2,000 days of free-model pipelines.

## How to verify live pricing

1. `https://co.agentrouter.org/portal/models` — per-model $/1M
2. `https://co.agentrouter.org/portal/pricing` — enterprise tiers
3. `https://co.agentrouter.org/portal/rankings` — demand signal

Next → [04 Quickstart](./04-quickstart.md)
