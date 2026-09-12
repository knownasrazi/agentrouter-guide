# 06 — Integrations: Every Agent & IDE (All 15+ Clients)

> One key, all AIs. Full steps + screenshots at `https://co.agentrouter.org/portal/guide`. Below are **copy-paste configs for every supported AI client** using your OG referral. Get $200 at `https://agentrouter.org/register?aff=v3aG` → key at `/console/token`.

## 0. Rule of thumb — OpenAI vs Anthropic

| Model family | Provider to select | Base URL | Example models |
|--------------|-------------------|----------|----------------|
| **Claude** Opus/Sonnet/Haiku | **Anthropic** | `https://co.agentrouter.org` (**no `/v1`**) | `claude-opus-4-8`, `claude-opus-4-7`, `claude-sonnet-4-5-20250929` |
| **Everything else** GPT/GLM/DeepSeek/Gemini/Qwen | **OpenAI Compatible** | `https://co.agentrouter.org/v1` or `https://agentrouter.org/v1` (**with `/v1`**) | `gpt-5.5`, `glm-5.2`, `deepseek-r1`, `gemini-2.0-pro`, `qwen3-coder-480b` |

> Mixing = 404. Anthropic with `/v1` fails. OpenAI without `/v1` fails. One `sk-...` works for all — quota is shared.

---

## A. VS Code Extensions (5)

### 1. Claude Code for VS Code — Anthropic (official)

1. Install extension `Anthropic.Claude Code` in VS Code
2. `Ctrl+Shift+P` → `Preferences: Open User Settings (JSON)` → merge:

```json
{
  "claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_AUTH_TOKEN", "value": "sk-YOUR_KEY" },
    { "name": "ANTHROPIC_BASE_URL",  "value": "https://co.agentrouter.org" },
    { "name": "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY", "value": "1" },
    { "name": "ANTHROPIC_MODEL",     "value": "claude-opus-4-7" }
  ],
  "claudeCode.disableLoginPrompt": true,
  "claudeCode.initialPermissionMode": "acceptEdits"
}
```

3. `Developer: Reload Window` → open Claude Code sidebar → send any message → success

### 2. Cline — Anthropic OR OpenAI Compatible

**Recommended — Anthropic (Claude):**
`Cline panel → gear → API Provider: Anthropic` → `Use custom base URL: ✓` → `Custom Base URL: https://co.agentrouter.org` → `API Key: sk-...` → `Model: claude-opus-4-8` → Save → test.

**OpenAI Compatible (GPT etc.):**
`API Provider: OpenAI Compatible` → `Base URL: https://co.agentrouter.org/v1` → `API Key: sk-...` → `Model ID: gpt-5.5` or `glm-5.2` or `kimi-k2.6` → Save.
> Do NOT use `/v1` for Anthropic. Do use `/v1` for OpenAI Compatible.

### 3. Roo Code — profile-based

`Roo Code → Profiles → New Profile`:
- **Claude path:** `Provider: Anthropic + Use custom URL` → `Custom Base URL: https://co.agentrouter.org` → `API Key: sk-...` → `Model: claude-opus-4-6`
- **GPT path:** `Provider: OpenAI Compatible` → `Base URL: https://co.agentrouter.org/v1` → `API Key: sk-...` → `Model: gpt-5.5`

### 4. Kilo Code — Custom Provider

`Kilo Code → Setting → Providers → Custom Provider → Connect`:
```
Provider ID: agentrouter
Display Name: AgentRouter
Base URL: https://co.agentrouter.org/v1
API Key: sk-YOUR_KEY
Models: claude-opus-4-8 / gpt-5.5 / glm-5.1 / kimi-k2.6
```

### 5. GitHub Copilot — Custom Endpoint

1. Ensure Copilot extension installed + GitHub logged in
2. `Copilot Chat → model picker → Manage Models… → Add Models → Custom Endpoint` → Group `AgentRouter`
3. Choose `apiType` + add models. Example `models.json` (VS Code will auto-fill `apiKey` via secret storage):

```json
[
  {
    "name": "AgentRouter-Claude",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.xxx}",
    "apiType": "messages",
    "models": [
      {"id": "claude-opus-4-8","name": "Claude Opus 4.8","url": "https://co.agentrouter.org","toolCalling": true,"vision": true,"maxInputTokens": 922000,"maxOutputTokens": 128000},
      {"id": "claude-opus-4-7","name": "Claude Opus 4.7","url": "https://co.agentrouter.org","toolCalling": true,"vision": true}
    ]
  },
  {
    "name": "AgentRouter-OpenAI",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.xxx}",
    "apiType": "chat-completions",
    "models": [
      {"id": "gpt-5.5","name": "GPT-5.5","url": "https://co.agentrouter.org/v1","toolCalling": true,"vision": true,"maxInputTokens": 922000,"maxOutputTokens": 128000},
      {"id": "glm-5.2","name": "GLM 5.2","url": "https://co.agentrouter.org/v1","toolCalling": true}
    ]
  }
]
```

> Claude = `messages` + `https://co.agentrouter.org` | OpenAI = `chat-completions` + `https://co.agentrouter.org/v1`

---

## B. CLI / Terminal (6)

### 6. Claude Code CLI — Anthropic (official)

```bash
npm install -g @anthropic-ai/claude-code@latest
claude --version

# macOS / Linux — persist in ~/.zshrc or ~/.bashrc
export ANTHROPIC_AUTH_TOKEN="sk-YOUR_KEY"
export ANTHROPIC_BASE_URL="https://co.agentrouter.org"
export ANTHROPIC_MODEL="claude-opus-4-8"
claude  # launch in project root, send any message

# Windows PowerShell — persist with [Environment]::SetEnvironmentVariable
$env:ANTHROPIC_AUTH_TOKEN="sk-YOUR_KEY"
$env:ANTHROPIC_BASE_URL="https://co.agentrouter.org"
$env:ANTHROPIC_MODEL="claude-opus-4-8"
claude
```

> `ANTHROPIC_BASE_URL` must NOT end with `/v1`. If you still see login prompt, ensure `claudeCode.disableLoginPrompt: true` in VS Code variant.

### 7. Codex — OpenAI Compatible

**OpenAI's terminal Codex** via AgentRouter — use any model (GPT-5.5, Claude via OpenAI shape, GLM):

```toml
# ~/.codex/config.toml  — macOS/Linux: ~/.codex/config.toml | Windows: $HOME\.codex\config.toml
model = "gpt-5.5"
model_provider = "agentrouter"

[model_providers.agentrouter]
name     = "AgentRouter"
base_url = "https://co.agentrouter.org/v1"
env_key  = "AGENTROUTER_API_KEY"
wire_api = "chat"   # important: use 'chat' for /v1/chat/completions
```

```bash
# Set key then run
export AGENTROUTER_API_KEY="sk-YOUR_KEY"   # macOS/Linux
# $env:AGENTROUTER_API_KEY="sk-YOUR_KEY"  # PowerShell
codex "Please reply OK to verify connectivity"
codex --model gpt-5.5 "Explain this repo in 5 bullets"
codex --model claude-opus-4-8 "Refactor this file to use async/await"
```

**Switch models without editing config:**
```bash
AGENTROUTER_API_KEY=sk-... codex --model glm-5.2 "Summarize these logs"
```

### 8. OpenCode — TUI Agent

**Full config for OpenCode** (project-level `opencode.json`):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "agentrouter": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "AgentRouter",
      "options": { "baseURL": "https://co.agentrouter.org/v1" },
      "models": {
        "gpt-5.5": { "name": "GPT-5.5" },
        "claude-opus-4-8": { "name": "Claude Opus 4.8" },
        "claude-sonnet-4-5-20250929": { "name": "Claude Sonnet 4.5" },
        "glm-5.2": { "name": "GLM 5.2" },
        "deepseek-r1": { "name": "DeepSeek R1" },
        "step3p5-code-alpha": { "name": "Step 3.5 Code Alpha" }
      }
    }
  },
  "model": "agentrouter/gpt-5.5"
}
```

```bash
opencode providers login --provider agentrouter  # paste sk-... when prompted
opencode
# Inside TUI: /model agentrouter/claude-opus-4-8  to switch
```

**Alternative — env-driven OpenCode:**

```bash
export AGENTROUTER_API_KEY=sk-YOUR_KEY
# Then opencode.json can omit hardcoded keys and use $AGENTROUTER_API_KEY
```

### 9. Qwen Code — Alibaba's CLI Agent

```bash
npm install -g @qwen-code/qwen-code@latest
qwen --version

export OPENAI_API_KEY="sk-YOUR_KEY"
export OPENAI_BASE_URL="https://co.agentrouter.org/v1"
export OPENAI_MODEL="gpt-5.5"          # or glm-5.2, qwen3-coder-480b
qwen

# Or per-run
OPENAI_API_KEY=sk-... OPENAI_BASE_URL=https://co.agentrouter.org/v1 qwen "Hello"
```

### 10. Crush — Charm's openai-compat

`~/.config/crush/crush.json` (global) or `./crush.json` (project):

```json
{
  "$schema": "https://charm.land/crush.json",
  "providers": {
    "agentrouter": {
      "type": "openai-compat",
      "base_url": "https://co.agentrouter.org/v1",
      "api_key": "$AGENTROUTER_API_KEY",
      "models": [
        {"id": "gpt-5.5","name": "GPT-5.5","context_window": 1000000,"default_max_tokens": 8192},
        {"id": "claude-opus-4-8","name": "Claude Opus 4.8","context_window": 200000,"default_max_tokens": 8192},
        {"id": "glm-5.2","name": "GLM 5.2","context_window": 1000000,"default_max_tokens": 8192},
        {"id": "step3p5-code-alpha","name": "Step 3.5 Code Alpha","context_window": 200000,"default_max_tokens": 8192}
      ]
    }
  }
}
```
```bash
export AGENTROUTER_API_KEY="sk-YOUR_KEY"
crush
```

### 11. Hermes Agent — Nous Research

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
# or: pipx install hermes-agent
hermes --version

hermes setup model
# Prompts: Provider → openai-api / OpenAI Compatible
#          Base URL → https://co.agentrouter.org/v1
#          API Key  → sk-YOUR_KEY
#          Model    → gpt-5.5 or claude-opus-4-8

hermes chat
```

---

## C. Desktop Apps (4)

### 12. Claude App (Desktop) — Gateway mode

1. `Help → Troubleshooting → Enable developer mode`
2. `Developer → Configure third-party inference`:
   ```
   Connection: Gateway
   Gateway base URL: https://co.agentrouter.org
   Gateway API key: sk-YOUR_KEY
   Gateway auth scheme: bearer
   ```
3. `Apply locally → Relaunch now` → bottom-left model picker now shows AgentRouter models

### 13. Trae — AI IDE by ByteDance

`Settings → Models → Add Model → Custom Config`:
- **Claude:** `API Format: Anthropic Messages` → `Request URL: https://co.agentrouter.org` (no trailing slash) → `Model ID: claude-opus-4-8` → `API Key: sk-...`
- **GPT/GLM:** `API Format: OpenAI Completions` → `Request URL: https://co.agentrouter.org/v1` → `Model ID: gpt-5.5` → `API Key: sk-...`
- Save → send `Please reply OK` to verify

### 14. Cursor — AI Editor

1. Login to Cursor (required — free users can't pick model, stuck on `auto`)
2. `Cursor Settings → Models → OpenAI API Key: sk-YOUR_KEY (toggle on)`
3. Toggle `Override OpenAI Base URL: ON` → `https://co.agentrouter.org/v1` (or `https://agentrouter.org/v1`)
4. Add models in picker: `claude-opus-4-8`, `gpt-5.5`, etc. → chat `Please reply OK`

### 15. Craft Agents — Custom Provider

Welcome screen → `Use other provider` → `Custom`:
- **Claude:** `Protocol: Anthropic Compatible` → `Endpoint: https://co.agentrouter.org` → `Model: claude-opus-4-8` → `API Key: sk-...`
- **GPT:** `Protocol: OpenAI Compatible` → `Endpoint: https://co.agentrouter.org/v1` → `Model: gpt-5.5` → `API Key: sk-...`

---

## D. Frameworks & Libraries (Any AI can call AgentRouter)

### 16. Python — OpenAI SDK (all GPT/Claude/GLM/DeepSeek)

```python
from openai import OpenAI
import os
client = OpenAI(api_key=os.environ["AGENTROUTER_API_KEY"], base_url="https://agentrouter.org/v1")

# Claude via OpenAI shape
r = client.chat.completions.create(model="claude-sonnet-4-5-20250929", messages=[{"role":"user","content":"Say hi"}])
print(r.choices[0].message.content)

# GPT-5.5
r = client.chat.completions.create(model="gpt-5.5", messages=[{"role":"user","content":"Write a haiku"}])

# DeepSeek R1 (premium reasoning, cheap)
r = client.chat.completions.create(model="deepseek-r1", messages=[{"role":"user","content":"Solve: 2x+3=7"}])

# Streaming
with client.chat.completions.stream(model="gpt-4o", messages=[{"role":"user","content":"Tell a story"}]) as s:
    for chunk in s:
        if chunk.choices[0].delta.content: print(chunk.choices[0].delta.content, end="")
```

### 17. Node / TypeScript — OpenAI SDK

```typescript
import OpenAI from "openai";
const client = new OpenAI({ apiKey: process.env.AGENTROUTER_API_KEY!, baseURL: "https://agentrouter.org/v1" });
const r = await client.chat.completions.create({ model: "claude-opus-4-8", messages: [{role:"user", content:"Ping"}] });
console.log(r.choices[0].message.content);

// Vercel AI SDK (any AI)
import { createOpenAI } from "@ai-sdk/openai";
const agentrouter = createOpenAI({ baseURL: "https://agentrouter.org/v1", apiKey: process.env.AGENTROUTER_API_KEY! });
const { text } = await generateText({ model: agentrouter("gpt-5.5"), prompt: "Hello" });
```

### 18. LangChain / LangGraph — Python

```python
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
print(llm.invoke("Refactor to async/await: ...").content)

# Multi-model graph: planner=Opus, executor=Sonnet, reviewer=DeepSeek
from langgraph.graph import StateGraph, END
planner = ChatOpenAI(model="claude-opus-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
executor = ChatOpenAI(model="claude-sonnet-4-5-20250929", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
reviewer = ChatOpenAI(model="deepseek-r1", openai_api_key="sk-...", openai_api_base="https://agentrouter.org/v1")
```

### 19. LlamaIndex

```python
from llama_index.llms.openai import OpenAI
from llama_index.core import Settings
Settings.llm = OpenAI(model="gpt-4o", api_key="sk-...", api_base="https://agentrouter.org/v1")
```

### 20. Continue.dev — VS Code autocomplete

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

### 21. LiteLLM — Universal proxy (100+ LLMs via one interface)

```python
from litellm import completion
import os
os.environ["OPENAI_API_BASE"] = "https://agentrouter.org/v1"
os.environ["OPENAI_API_KEY"] = "sk-..."

resp = completion(model="claude-opus-4-8", messages=[{"role":"user","content":"Hello"}])  # LiteLLM maps to AgentRouter
resp = completion(model="gpt-5.5", messages=[{"role":"user","content":"Hello"}])
```

Or `litellm` proxy config:
```yaml
model_list:
  - model_name: claude-opus-4-8
    litellm_params: { model: openai/claude-opus-4-8, api_base: https://agentrouter.org/v1, api_key: os.environ/AGENTROUTER_API_KEY }
  - model_name: gpt-5.5
    litellm_params: { model: openai/gpt-5.5, api_base: https://agentrouter.org/v1, api_key: os.environ/AGENTROUTER_API_KEY }
```

### 22. AnythingLLM / Open WebUI / LibreChat / Chatbox — Chat UIs

All support OpenAI Compatible custom endpoint:
```
Provider: OpenAI
Base URL / API URL: https://agentrouter.org/v1
API Key: sk-YOUR_KEY
Model: gpt-5.5 or claude-sonnet-4-5-20250929 or glm-5.2
```

### 23. Aider — AI pair programmer (terminal)

```bash
pip install aider-chat
export OPENAI_API_KEY=sk-YOUR_KEY
export OPENAI_API_BASE=https://agentrouter.org/v1
aider --model gpt-5.5
aider --model claude-sonnet-4-5-20250929
# or
aider --openai-api-key sk-... --openai-api-base https://agentrouter.org/v1 --model gpt-5.5
```

### 24. Raw HTTP / cURL — every AI that speaks HTTP

```bash
# Chat completions — GPT
curl https://agentrouter.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-YOUR_KEY" \
  -d '{"model":"gpt-5.5","messages":[{"role":"user","content":"Say hello"}],"max_tokens":100}'

# Claude via OpenAI shape (also works)
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer sk-YOUR_KEY" \
  -d '{"model":"claude-opus-4-8","messages":[{"role":"user","content":"Say hello"}]}'

# Streaming
curl https://agentrouter.org/v1/chat/completions -H "Authorization: Bearer sk-..." -H "Content-Type: application/json" \
  -d '{"model":"gpt-4o","stream":true,"messages":[{"role":"user","content":"Write a haiku"}]}'

# Embeddings
curl https://agentrouter.org/v1/embeddings -H "Authorization: Bearer sk-..." -H "Content-Type: application/json" \
  -d '{"model":"text-embedding-3-small","input":"Hello world"}'

# Images
curl https://agentrouter.org/v1/images/generations -H "Authorization: Bearer sk-..." -H "Content-Type: application/json" \
  -d '{"model":"dall-e-3","prompt":"A cat astronaut","n":1}'

# Audio
curl https://agentrouter.org/v1/audio/speech -H "Authorization: Bearer sk-..." -H "Content-Type: application/json" \
  -d '{"model":"tts-1","input":"Hello from AgentRouter","voice":"alloy"}' --output speech.mp3
```

### 25. n8n / Make / Zapier — Automation

`HTTP Request` node:
```
Method: POST
URL: https://agentrouter.org/v1/chat/completions
Header: Authorization: Bearer sk-YOUR_KEY
Header: Content-Type: application/json
Body: {"model":"claude-sonnet-4-5-20250929","messages":[{"role":"system","content":"Email classifier. JSON only."},{"role":"user","content":"Classify: {{$json.body}}"}],"response_format":{"type":"json_object"}}
```

### 26. Generic — Any OpenAI-compatible client

If your AI tool says **“OpenAI compatible”** or **“Custom base URL”**, use:
```
Base URL: https://agentrouter.org/v1  (or https://co.agentrouter.org/v1)
API Key: your AgentRouter sk-...
Model: any from docs/03-models-pricing.md (e.g., claude-opus-4-8, gpt-5.5, glm-5.2)
```

Works with: **Windsurf, Zed, Bolt.new, v0, OpenHands, AutoGPT, BabyAGI, CrewAI, Autogen, AnythingLLM, etc.** — if it accepts `OPENAI_API_BASE` or `baseURL`, it works.

---

## Quick switch chart — All AIs, one key

| Client | Config file / place | Base URL | Key env var |
|--------|---------------------|----------|-------------|
| Codex | `~/.codex/config.toml` + `AGENTROUTER_API_KEY` | `https://co.agentrouter.org/v1` | `AGENTROUTER_API_KEY` |
| OpenCode | `opencode.json` → `provider.agentrouter.options.baseURL` | `https://co.agentrouter.org/v1` | `AGENTROUTER_API_KEY` |
| Claude Code CLI | `export ANTHROPIC_BASE_URL` | `https://co.agentrouter.org` | `ANTHROPIC_AUTH_TOKEN` |
| Claude Code VS Code | `settings.json` → `claudeCode.environmentVariables` | `https://co.agentrouter.org` | `ANTHROPIC_AUTH_TOKEN` |
| Cline/Roo/Kilo | Provider settings | `https://co.agentrouter.org[/v1]` | pasted in UI |
| Cursor | Settings → Override Base URL | `https://co.agentrouter.org/v1` | `OpenAI API Key` |
| Qwen/Crush/Hermes | env `OPENAI_*` | `https://co.agentrouter.org/v1` | `OPENAI_API_KEY` |
| Python/Node SDK | `base_url` / `baseURL` param | `https://agentrouter.org/v1` | `AGENTROUTER_API_KEY` |
| Aider | `OPENAI_API_BASE` env | `https://agentrouter.org/v1` | `OPENAI_API_KEY` |
| AnythingLLM etc. | UI → OpenAI custom URL | `https://agentrouter.org/v1` | pasted in UI |
| LiteLLM | `api_base` param | `https://agentrouter.org/v1` | `OPENAI_API_KEY` |

> **Pro tip:** Start with `glm-4.5-air` (free, $0) to verify connectivity, then switch to `claude-opus-4-8` or `gpt-5.5` for production.

Next → [07 Examples](./07-examples.md)
