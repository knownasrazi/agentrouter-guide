# 08 — Troubleshooting, Security & FAQ

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `401 Unauthorized` | Wrong key / missing `Bearer ` | Regenerate at `agentrouter.org/console/token`; header must be `Authorization: Bearer sk-...` |
| `404 Not Found` | Wrong base URL | Anthropic surface **no** `/v1` (`https://co.agentrouter.org`); OpenAI surface **with** `/v1` (`https://agentrouter.org/v1`) |
| `model not found` | Bad ID | Use exact ID from `/portal/models`; try `gpt-4o` or `claude-sonnet-4-5-20250929` |
| `402 Budget pool quota has been exhausted` | Global per-model cap hit (not your balance) — #1 community complaint | Switch model (Opus → Sonnet → Haiku) or retry later / off-peak; free `glm-4.5-air` to verify key |
| Balance $0 right after signup | Referral missing at signup | Contact support; referral `?aff=` must be in URL at registration time |
| `GitHub auth error` | New GitHub (<1 yr) | Use older GitHub; mirrors like `gorouter.app` accept newer but similar caps |
| `claude-code` still shows login | `disableLoginPrompt` false | Set `claudeCode.disableLoginPrompt: true` + `Developer: Reload Window` |
| `command not found: claude` | CLI not installed | `npm i -g @anthropic-ai/claude-code@latest` |
| Chat disabled / recharge disabled | Admin feature flag | Use API; console chat may be gated |
| Quota shows $175 but can’t call | Pool exhausted for that model | Try another model; see 402 row |
| Latency >500ms | Peak / US→SG hop | Retry with backoff; stream; use free model temporarily |
| Streaming no output | Missing `stream: true` | Add `"stream": true` to JSON |

**Community signals (Aug–Sep 2026 gists):** `402` appears even with $175–$275 remaining, balance flickers to $0, nightly pool resets early (SG time). Universal workaround: **swap model/provider or time-shift.**

## Security & Privacy

### What the proxy sees
Every prompt, system message, and response transits AgentRouter infra before upstream.

### Do NOT send via any proxy
- PII under GDPR/HIPAA/CCPA (health, SSN, financial)
- Trade secrets / regulated source code
- Passwords, private keys, OAuth tokens
- Classified / government-sensitive

### Generally safe
Public docs, open-source code, marketing copy, general coding Q&A, prototyping logic.

### Best practices

```bash
echo ".env" >> .gitignore
export AGENTROUTER_API_KEY="sk-..."
# In code — never hardcode
import os
api_key = os.environ["AGENTROUTER_API_KEY"]  # fail-loud if missing
# Prod — secrets manager: AWS Secrets Manager / Vault / Doppler
```

Enterprise portal adds: per-member quotas, audit logs, fine-grained provider allowlists (e.g., EU-only routes).

### No SLA — risk guidance

```
Experimentation / learning     → Low risk
Side project / prototype       → Low risk
Production secondary traffic   → Medium (keep direct fallback)
Primary production system      → High risk (use direct APIs)
```

Use the fallback pattern in `07-examples.md`.

## FAQ

**Anthropic vs OpenAI base URL?**
Anthropic (Claude): `https://co.agentrouter.org` (no `/v1`). OpenAI (GPT etc.): `https://co.agentrouter.org/v1` or `https://agentrouter.org/v1`. Don’t mix.

**One key for multiple agents?**
Yes. Cline + Roo + Claude Code can share `sk-...`; usage unified.

**Need to modify source code?**
No. Only `base_url`/`api_key` env/config.

**How to switch model?**
Change `model` param. Enterprise suffix variants (company-specific) take precedence if issued.

**How long does $200 last?**
~25 days heavy mixed prototyping to ~2,000 days of free-model pipelines. See GUIDE §9.

**Is it legit or scam?**
Live since Oct 2025, New API fork, thousands of verifications, non-profit “public welfare” stated. Works for dev/test; keep fallback for prod. Skepticism is healthy.

**Why Singapore latency?**
Primary infra is SG (`region: sgp` seen in WAF token). Batch/async unaffected; interactive chat benefits from streaming.

## Sources

- Gateway: `https://agentrouter.org` (endpoints, One API footer)
- Portal: `https://co.agentrouter.org` + `/portal/models` + `/portal/pricing` + `/portal/rankings` + `/portal/guide` + `/portal/about` (`neo@agentrouter.org`)
- Community: Gist `mzaman/a9409de6…` (Definitive Guide), Gist `zabih3/28d5e…` (Review 2026, $200, 402 reports)
- Contrast: `www.agent-router.org` (MCP, unrelated), `docs.agentrouter.to` (capability gateway, unrelated)

---
*Prices/models change — verify at `/portal/models` before billing.*
