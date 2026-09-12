# My Referral — AgentRouter OG Credits

> **My personal referral link. OG credits. No card. Supports this guide.**

## My Referral Link (use this)

```
https://agentrouter.org/register?aff=v3aG
```

- **Standard signup:** $100
- **Via `aff=v3aG` (OG — my referral):** **$200** for you + **+$100** bonus to me to keep this guide updated
- **Auth:** GitHub OAuth only · **No credit card** · Credits appear immediately at `https://agentrouter.org/console/token`
- **`aff=v3aG` must be in the URL at registration** — can't be added later. Always click the referral link, not plain `agentrouter.org/register`.

---

## Get OG Credits — 3 Steps (personal referral)

1. **Register via my link**
   Click `https://agentrouter.org/register?aff=v3aG` and sign in with GitHub. Verify balance shows **$200 OG** after auth.

2. **Generate your key**
   `https://agentrouter.org/console/token` → Generate → `sk-...` → store in `.env` (`AGENTROUTER_API_KEY`), never commit.

3. **Verify $0 test**
   ```bash
   curl https://agentrouter.org/v1/chat/completions \
     -H "Authorization: Bearer $AGENTROUTER_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"model":"glm-4.5-air","messages":[{"role":"user","content":"ping"}],"max_tokens":5}'
   ```
   Expect `200`. If `402 Budget pool quota exhausted` with balance left → global per-model cap, switch model (e.g., `glm-4.5-air` → `gpt-4o-mini`) or retry later.

Thanks for using my referral — it directly supports this open guide!

---

## Copy-Paste to Share (supports me)

> **Get $200 OG credits on AgentRouter (vs $100 standard) — my referral:**
> **https://agentrouter.org/register?aff=v3aG**
> GitHub login, no card needed. Thanks for supporting this guide. After signup, grab your key at `https://agentrouter.org/console/token`.

Short:
```
OG credits via my referral: https://agentrouter.org/register?aff=v3aG — $200 vs $100 standard. GitHub auth, no card.
```

---

## FAQ

**Why does the link matter?**
Plain `agentrouter.org/register` = $100. `?aff=v3aG` = $200 OG for you + $100 bonus to me. It's my personal referral.

**Can I add the referral after signup?**
No. Must be in URL at first registration. If you got $100, contact support or re-register with a different GitHub using `?aff=v3aG`.

**GitHub too new?**
Accounts <1 year often get `GitHub account too new`. Use an older GitHub.

**How long do $200 last?**
~25 days of heavy mixed prototyping to ~2,000 days of `glm-4.5-air`/`deepseek-v2-lite` (free tier) usage. See `docs/03-models-pricing.md`.

**Do we share one key?**
You *can* (one key works across Cline/Roo/Claude Code, quota shared), but for OG bonus tracking each person should register via `aff=v3aG` individually.

---

## Links

- My OG referral: **https://agentrouter.org/register?aff=v3aG**
- Console / token: https://agentrouter.org/console/token
- Portal docs: https://co.agentrouter.org/portal/guide
- Full guide: [README.md](./README.md) · [GUIDE.md](./GUIDE.md)

*Last updated 2026-09-12 — verify live pricing at `co.agentrouter.org/portal/models`.*
