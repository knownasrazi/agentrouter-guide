# 01 — Introduction: What Is AgentRouter?

> Source: `agentrouter.org` + `co.agentrouter.org` (Enterprise portal), verified 2026-09-12.

## One-sentence definition

**AgentRouter (`agentrouter.org`) is a non-profit, OpenAI-compatible LLM gateway** that puts **30+ frontier models behind one base URL and one API key** — so you change the `model` string, not your provider, SDK, or billing dashboard.

Built on **New API & One API** (footer), launched **Oct 2025**, China-origin, Singapore infra, inspired by OpenRouter.

## The problem it solves

Developers today juggle:

- 3–5 API keys (Anthropic, OpenAI, Zhipu, Google, DeepSeek…)
- 3–5 bills, dashboards, rate limits, quotas
- SDK swaps to test `claude-opus-4-8` vs `gpt-5.5` vs `glm-5.2`
- $20–$100/mo subscriptions just to *try* a model

AgentRouter collapses that to:

```
One key (sk-...) + one base URL (https://agentrouter.org/v1) → any model
```

## Four pillars (portal)

| Pillar | What it means |
|--------|---------------|
| **Unified Interface** | OpenAI + Anthropic SDK compatible; all major agent frameworks; “zero migration cost” |
| **Intelligent Routing** | Auto load balancing, failover, multi-provider redundancy → 99.9% claimed |
| **Cost Optimization** | Best price-performance auto-pick, real-time metering, pay-as-you-go, volume discounts |
| **Data Governance** | Fine-grained provider controls (e.g., keep EU prompts on EU-trusted channels) |

> Original CN: *“致力于为企业研发人员提供快速、便捷的Web API接口调用方案，高性价比集成全球顶尖AI大模型。”*

## Who it’s built for

- Enterprise dev teams shipping AI apps/agents (portal tagline)
- Individual devs who want to try Claude Code / Codex / Cline without 5 subscriptions (gateway tagline: *“Better price, better stability, no subscription required”*)

## Stats (portal live)

- 15+ models · 10M+ daily calls · 50+ enterprises · 200+ active agents → 50M+ weekly, 1,200+ agents on rankings page

## What it is NOT

See **README disambiguation** + **GUIDE §2**. In short: `agent-router.org` (MCP marketplace) and `agentrouter.to` (capability gateway for paid APIs) are **different products** with similar names. This guide covers the **LLM gateway only**.

## Philosophy (non-profit)

Portal + community gists frame AgentRouter as **public-welfare infra** (*“providing free quotas to support AI Coding within our capacity”*), not a classic freemium funnel. That explains the generous $100–$200 free credits — and also the funding-opacity risk (see troubleshooting).

Next → [02 Architecture](./02-architecture.md)
