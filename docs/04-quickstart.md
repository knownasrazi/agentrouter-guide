# 04 — Quickstart: 0 → First Call in 3 Minutes

## Step 1: Register (<1 min) — MY OG REFERRAL

1. **Use my OG link: `https://agentrouter.org/register?aff=v3aG`** (plain `/register` = only $100 — don’t use it). This gives **you $200 OG** vs $100 — supports this guide.
2. **Sign in with GitHub** → Authorize OAuth.
   - New GitHub accounts (<1 yr) often get `GitHub account too new` — use an older account.
3. Check console balance shows **$200 OG**. If you see $100, you missed `?aff=v3aG` — re-register with my link. You get $200, I get +$100 bonus.

## Step 2: Generate API key

- Go to `https://agentrouter.org/console/token` → **Generate New Token** → copy `sk-...` immediately (won’t show again).
- Enterprise alternative: `https://co.agentrouter.org/portal/contact` → Get API Key.

```bash
# .env (never commit)
AGENTROUTER_API_KEY=sk-your-key-here

# macOS Keychain
security add-generic-password -a "$USER" -s "agentrouter" -w "sk-your-key-here"
```

## Step 3: Verify with curl (free model, $0)

```bash
curl https://agentrouter.org/v1/chat/completions \
  -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"glm-4.5-air","messages":[{"role":"user","content":"ping"}],"max_tokens":5}'
```

Expect `200` + `choices[0].message.content`. If `401`, check `Bearer ` prefix. If `404`, check base URL (see next).

## Step 4: Pick your surface

| You are… | Next doc |
|----------|----------|
| Terminal coder | [06 Integrations — Claude Code CLI](./06-integrations.md#claude-code-cli) |
| VS Code daily | [06 Integrations — Cline / Roo / Continue](./06-integrations.md) |
| Python app | [07 Examples — Python SDK](./07-examples.md#python-sdk) |
| Node app | [07 Examples — Node SDK](./07-examples.md#nodejs--typescript-sdk) |
| Automating | [07 Examples — n8n](./07-examples.md#n8n) |

## Step 5: Graduate free → premium

```python
model = "glm-4.5-air"                  # Phase 1: validate $0
model = "gpt-4o-mini"                  # Phase 2: test quality (cents)
model = "claude-sonnet-4-5-20250929"   # Phase 3: prod
```

## Step 6: Monitor usage

Console at `agentrouter.org/console` (or enterprise admin at `co.agentrouter.org`) shows per-model token accounting. Check before you assume free credits are infinite — `402 pool exhausted` can block even with balance (see [08 Troubleshooting](./08-troubleshooting.md)).

Next → [05 API Reference](./05-api-reference.md)
