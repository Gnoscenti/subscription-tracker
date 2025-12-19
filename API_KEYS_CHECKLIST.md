# API Keys Checklist

Quick reference for gathering the required API keys.

## ✅ Critical Services

### 1. Supabase
- [ ] `NEXT_PUBLIC_SUPABASE_URL`
- [ ] `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- [ ] `SUPABASE_SERVICE_ROLE_KEY`

**Where to find:** [supabase.com](https://supabase.com) → Your Project → Settings → API

---

### 2. Stripe
- [ ] `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`
- [ ] `STRIPE_SECRET_KEY`
- [ ] `STRIPE_WEBHOOK_SECRET` (we'll set this up after deployment)

**Where to find:** [stripe.com](https://stripe.com) → Developers → API keys

---

### 3. Cloudflare
- [ ] `CLOUDFLARE_ACCOUNT_ID`
- [ ] `CLOUDFLARE_API_TOKEN`

**Where to find:** [cloudflare.com](https://cloudflare.com) → Workers & Pages → Account ID in sidebar

---

## 🟡 Optional (Can Add Later)

### 4. Plaid (Bank Connections)
- [ ] `PLAID_CLIENT_ID`
- [ ] `PLAID_SECRET`
- [ ] `PLAID_ENV` (use "sandbox" for testing)

**Where to find:** [plaid.com](https://plaid.com) → Team Settings → Keys

---

### 5. OpenAI (AI Parsing)
- [ ] `OPENAI_API_KEY`

**Where to find:** [platform.openai.com](https://platform.openai.com) → API keys

---

## 📝 Format for Sharing

You can share the keys in this format:

```
SUPABASE:
- URL: https://xxxxx.supabase.co
- Anon Key: eyJhbGc...
- Service Role: eyJhbGc...

STRIPE:
- Publishable: pk_test_...
- Secret: sk_test_...

CLOUDFLARE:
- Account ID: abc123...
- API Token: xyz789...
```

Or just paste them however is convenient - I'll parse them out!
