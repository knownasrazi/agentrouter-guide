# 09 — All Providers Guide: Codex, OpenCode & Every AI

> Verified against `co.agentrouter.org/portal/models` and gateway catalog. Use my OG referral `https://agentrouter.org/register?aff=v3aG` for $200 credits. Prices change — verify at `/portal/models` before use.

This guide lists **every provider type available via AgentRouter** and shows **exactly how to call each provider from Codex, OpenCode, and any OpenAI-compatible AI**. One AgentRouter key (`sk-...` from `https://agentrouter.org/console/token`) works for all.

## Provider Matrix

| Provider | Example Models | Context | Pricing (Input/Output per 1M) | Gateway Base URL | Best For |
|----------|---------------|---------|-------------------------------|------------------|----------|
| **Anthropic — Claude** | `claude-opus-4-8`, `claude-opus-4-7`, `claude-opus-4-6`, `claude-sonnet-4-5-20250929`, `claude-haiku-4-5-20251001` | 200K – 1M | $2–$8 / $10–$40 | `https://co.agentrouter.org` (Anthropic, no `/v1`) or `https://agentrouter.org/v1` (OpenAI shape) | Agentic coding, long-horizon, reasoning |
| **OpenAI — GPT** | `gpt-5.5`, `gpt-5`, `gpt-4o`, `gpt-4o-mini`, `gpt-3.5-turbo` | 128K – 1M | $0.15–$10 / $0.6–$30 | `https://co.agentrouter.org/v1` | General, vision, structured output |
| **Zhipu AI — GLM** | `glm-5.2` (MoE flagship), `glm-5.1`, `glm-4.5`, `glm-4.5-air` (free) | 128K – 1M | $0–$3 / $0–$4.5 | `https://co.agentrouter.org/v1` | Cost-effective engineering, CN models |
| **DeepSeek** | `deepseek-r1`, `deepseek-v2-lite`, `deepseek-coder-v2-lite` | 32K – 64K | $0.14–$0.55 / $0.28–$2.19 | `https://co.agentrouter.org/v1` | Math, STEM, cheap reasoning |
| **Google — Gemini** | `gemini-2.0-pro`, `gemini-1.5-flash`, `gemini-3-pro` | 1M – 2M | $0.075–$7 / $0.3–$21 | `https://co.agentrouter.org/v1` | Long docs, multimodal, OCR |
| **Alibaba — Qwen** | `qwen3-coder-480b`, `qwen2-7b-instruct`, `kimi-k2.6` (Moonshot) | 32K – 64K | $2–$3 / $6–$15 | `https://co.agentrouter.org/v1` | Code gen, instruction following |
| **Mistral** | `mistral-7b-instruct` | 32K | $0.25 / $0.5 | `https://co.agentrouter.org/v1` | Fast, lightweight |
| **Other via gateway** | `step3p5-code-alpha` | 200K | Contact portal | `https://co.agentrouter.org/v1` | Specialized code |

Portal filters: Provider, Capability (code/reasoning/agent/long-context/multimodal), Scenario (agent dev, code assist, data analysis, doc processing). Enterprise can request Gemini/DeepSeek/Llama custom additions via `neo@agentrouter.org`.

---

## Codex — All Providers

Codex uses `~/.codex/config.toml`. One provider entry (`agentrouter`) routes to all models — just change `model = "..."` or `--model` flag.

### Base Config (covers every provider)

```toml
# ~/.codex/config.toml — macOS/Linux: ~/.codex/config.toml | Windows: $HOME\.codex\config.toml
model = "gpt-5.5"
model_provider = "agentrouter"

[model_providers.agentrouter]
name     = "AgentRouter"
base_url = "https://co.agentrouter.org/v1"
env_key  = "AGENTROUTER_API_KEY"
wire_api = "chat"
```

```bash
export AGENTROUTER_API_KEY="sk-YOUR_KEY"
# or PowerShell: $env:AGENTROUTER_API_KEY="sk-YOUR_KEY"
```

### Per-Provider Usage

```bash
# Anthropic Claude
codex --model claude-opus-4-8 "Review this PR for security issues"
codex --model claude-sonnet-4-5-20250929 "Refactor to async/await"
codex --model claude-haiku-4-5-20251001 "Summarize these logs"

# OpenAI GPT
codex --model gpt-5.5 "Explain CAP theorem with examples"
codex --model gpt-4o "Generate unit tests for this file"
codex --model gpt-4o-mini "Draft release notes"

# Zhipu GLM
codex --model glm-5.2 "Design a microservice architecture"
codex --model glm-4.5-air "Classify this ticket: billing or tech?"

# DeepSeek
codex --model deepseek-r1 "Solve this math proof step by step"
codex --model deepseek-v2-lite "Code completion for this function"

# Google Gemini
codex --model gemini-2.0-pro "Summarize this 500-page PDF"
codex --model gemini-1.5-flash "OCR post-processing for these images"

# Alibaba Qwen / Moonshot
codex --model qwen3-coder-480b "Implement a BST in Python"
codex --model kimi-k2.6 "Write a CLI tool in Go"
```

Verify any provider:
```bash
codex "Please reply OK"  # uses default model in config.toml
```

---

## OpenCode — All Providers

OpenCode uses `opencode.json` in project root. Configure once, switch models with `/model`.

### Base Config (all providers in one file)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "agentrouter": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "AgentRouter",
      "options": { "baseURL": "https://co.agentrouter.org/v1" },
      "models": {
        "claude-opus-4-8": { "name": "Claude Opus 4.8" },
        "claude-opus-4-7": { "name": "Claude Opus 4.7" },
        "claude-sonnet-4-5-20250929": { "name": "Claude Sonnet 4.5" },
        "claude-haiku-4-5-20251001": { "name": "Claude Haiku 4.5" },
        "gpt-5.5": { "name": "GPT-5.5" },
        "gpt-4o": { "name": "GPT-4o" },
        "gpt-4o-mini": { "name": "GPT-4o Mini" },
        "glm-5.2": { "name": "GLM 5.2" },
        "glm-4.5-air": { "name": "GLM 4.5 Air (free)" },
        "deepseek-r1": { "name": "DeepSeek R1" },
        "deepseek-v2-lite": { "name": "DeepSeek V2 Lite" },
        "gemini-2.0-pro": { "name": "Gemini 2.0 Pro" },
        "gemini-1.5-flash": { "name": "Gemini 1.5 Flash" },
        "qwen3-coder-480b": { "name": "Qwen3 Coder 480B" },
        "mistral-7b-instruct": { "name": "Mistral 7B" },
        "step3p5-code-alpha": { "name": "Step 3.5 Code Alpha" }
      }
    }
  },
  "model": "agentrouter/gpt-5.5"
}
```

Login and run:
```bash
opencode providers login --provider agentrouter  # paste sk-... when prompted
opencode
# Inside TUI:
#   /model agentrouter/claude-opus-4-8
#   /model agentrouter/deepseek-r1
#   /model agentrouter/gemini-2.0-pro
```

### Provider-Specific Tips for OpenCode

| Provider | Recommended Model for OpenCode | When to Use |
|----------|-------------------------------|-------------|
| Anthropic | `claude-sonnet-4-5-20250929` | Balanced agentic coding (default) |
| Anthropic | `claude-opus-4-8` | Deep refactor, architecture |
| OpenAI | `gpt-5.5` | Reasoning + tool use |
| OpenAI | `gpt-4o-mini` | Cheap, fast edits |
| Zhipu | `glm-4.5-air` | Free — triage, classification, autocomplete |
| DeepSeek | `deepseek-r1` | Free-ish reasoning — math, proofs |
| Google | `gemini-1.5-flash` | Free-ish — OCR, doc parsing |
| Qwen | `qwen3-coder-480b` | Specialized code gen |

Switch without editing file:
```bash
# Launch with different default model
opencode --model agentrouter/claude-opus-4-8
```

---

## Every AI — Provider Quick Reference

For **any AI** that supports OpenAI-compatible `baseURL` / `OPENAI_API_BASE` (Windsurf, Zed, Aider, LiteLLM, AnythingLLM, Cursor, etc.):

| Provider | model string to use | baseURL | Pricing hint |
|----------|---------------------|---------|--------------|
| Anthropic Claude | `claude-opus-4-8` | `https://co.agentrouter.org/v1` (or Anthropic `https://co.agentrouter.org`) | $8/$40 per 1M |
| Anthropic Claude | `claude-sonnet-4-5-20250929` | same | $3/$15 |
| OpenAI GPT | `gpt-5.5` | `https://co.agentrouter.org/v1` | $4/$8 |
| Zhipu GLM | `glm-5.2` | same | $3/$4.5 |
| Zhipu GLM | `glm-4.5-air` | same | $0 free |
| DeepSeek | `deepseek-r1` | same | $0.55/$2.19 |
| Google Gemini | `gemini-2.0-pro` | same | $3.5/$10.5 |
| Qwen | `qwen3-coder-480b` | same | $2/$6 |
| Mistral | `mistral-7b-instruct` | same | $0.25/$0.5 |

### Python Example — all providers

```python
from openai import OpenAI
import os
client = OpenAI(api_key=os.environ["AGENTROUTER_API_KEY"], base_url="https://agentrouter.org/v1")

def call(model, prompt):
    return client.chat.completions.create(model=model, messages=[{"role":"user","content":prompt}]).choices[0].message.content

print(call("claude-opus-4-8", "Review this code"))
print(call("gpt-5.5", "Write a haiku"))
print(call("glm-4.5-air", "Classify: billing or tech?"))
print(call("deepseek-r1", "Prove sqrt(2) is irrational"))
print(call("gemini-2.0-pro", "Summarize this doc"))
```

### Node Example — all providers

```typescript
import OpenAI from "openai";
const client = new OpenAI({ apiKey: process.env.AGENTROUTER_API_KEY!, baseURL: "https://agentrouter.org/v1" });
const models = ["claude-opus-4-8","gpt-5.5","glm-5.2","deepseek-r1","gemini-2.0-pro"] as const;
for (const m of models) {
  const r = await client.chat.completions.create({ model: m, messages: [{role:"user", content:"Say hi in 5 words"}] });
  console.log(m, "->", r.choices[0].message.content);
}
```

### cURL — all providers

```bash
for m in claude-opus-4-8 gpt-5.5 glm-5.2 deepseek-r1 gemini-2.0-pro; do
  echo "=== $m ==="
  curl -s https://agentrouter.org/v1/chat/completions \
    -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
    -H "Content-Type: application/json" \
    -d "{\"model\":\"$m\",\"messages\":[{\"role\":\"user\",\"content\":\"Say hi\"}],\"max_tokens\":20}" | head -c 200
done
```

---

## Choosing a Provider

- **Start free:** `glm-4.5-air` or `deepseek-v2-lite` ($0) to verify key, then move up.
- **Agentic coding:** `claude-sonnet-4-5-20250929` (balanced) or `claude-opus-4-8` (max).
- **Reasoning / math:** `deepseek-r1` (cheap) or `gpt-5.5` (premium).
- **Long docs (1M ctx):** `claude-opus-4-8`, `glm-5.2`, `gemini-2.0-pro`.
- **Fast / cheap edits:** `claude-haiku-4-5-20251001`, `gpt-4o-mini`, `gemini-1.5-flash`.

See `03-models-pricing.md` for full tier table and `06-integrations.md` for client setup.

Next → [08 Troubleshooting](./08-troubleshooting.md)
