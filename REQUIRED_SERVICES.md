# Required API Keys and Services

This document lists all the external services, API keys, and credentials you need to provide for the Subscription Tracker application to function properly.

## 🔴 Critical Services (Required for Core Functionality)

### 1. **Supabase**
**Purpose:** Database, authentication, and storage backend

**What you need:**
- Create a project at [supabase.com](https://supabase.com)
- Get your project credentials:
  - `NEXT_PUBLIC_SUPABASE_URL` - Your Supabase project URL
  - `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Public anonymous key
  - `SUPABASE_SERVICE_ROLE_KEY` - Service role key (keep secret!)

**Setup steps:**
1. Sign up at Supabase
2. Create a new project
3. Go to Settings → API to find your keys
4. Enable Email authentication in Authentication → Providers
5. (Optional) Enable OAuth providers (Google, GitHub, etc.)

**Cost:** Free tier available, Pro starts at $25/month

---

### 2. **Stripe**
**Purpose:** Payment processing and subscription billing for your app

**What you need:**
- Create an account at [stripe.com](https://stripe.com)
- Get your API keys:
  - `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` - Public key for frontend
  - `STRIPE_SECRET_KEY` - Secret key for backend
  - `STRIPE_WEBHOOK_SECRET` - Webhook signing secret

**Setup steps:**
1. Sign up at Stripe
2. Go to Developers → API keys
3. Copy your publishable and secret keys
4. Create Products and Prices for your subscription tiers:
   - Basic: $9/month
   - Pro: $19/month
   - Premium: $39/month
5. Set up webhook endpoint (we'll provide the URL after deployment)
6. Subscribe to these webhook events:
   - `customer.subscription.created`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
   - `invoice.payment_succeeded`
   - `invoice.payment_failed`

**Cost:** Pay-as-you-go, ~2.9% + $0.30 per transaction

---

### 3. **Vercel**
**Purpose:** Frontend hosting and deployment

**What you need:**
- Create an account at [vercel.com](https://vercel.com)
- Connect your GitHub repository
- No API keys needed initially - we'll configure during deployment

**Setup steps:**
1. Sign up at Vercel
2. Import your GitHub repository
3. We'll add environment variables during deployment

**Cost:** Free tier available, Pro starts at $20/month

---

### 4. **Cloudflare Workers**
**Purpose:** Serverless receipt parsing and processing

**What you need:**
- Create an account at [cloudflare.com](https://cloudflare.com)
- Get your credentials:
  - `CLOUDFLARE_ACCOUNT_ID` - Your account ID
  - `CLOUDFLARE_API_TOKEN` - API token with Workers permissions

**Setup steps:**
1. Sign up at Cloudflare
2. Go to Workers & Pages
3. Create an API token with "Edit Cloudflare Workers" permissions
4. Get your Account ID from the Workers dashboard

**Cost:** Free tier available (100,000 requests/day), Paid starts at $5/month

---

## 🟡 Important Services (Enhanced Functionality)

### 5. **Plaid** (Optional but Recommended)
**Purpose:** Bank account connection for automatic subscription detection

**What you need:**
- Create an account at [plaid.com](https://plaid.com)
- Get your credentials:
  - `PLAID_CLIENT_ID` - Your client ID
  - `PLAID_SECRET` - Your secret key (sandbox or production)
  - `PLAID_ENV` - Environment (sandbox, development, or production)

**Setup steps:**
1. Sign up at Plaid
2. Go to Team Settings → Keys
3. Start with sandbox keys for testing
4. Apply for production access when ready to launch

**Cost:** Free for development, production pricing varies by volume

**Note:** Without Plaid, users can only add subscriptions manually or via receipt upload.

---

### 6. **OpenAI API** (Optional)
**Purpose:** AI-powered receipt parsing and subscription detection

**What you need:**
- Create an account at [platform.openai.com](https://platform.openai.com)
- Get your API key:
  - `OPENAI_API_KEY` - Your API key

**Setup steps:**
1. Sign up at OpenAI
2. Go to API keys section
3. Create a new API key
4. Add credits to your account

**Cost:** Pay-as-you-go, ~$0.002 per 1K tokens (GPT-4o-mini)

**Alternative:** You can use the pre-configured OpenAI-compatible API in the Manus environment, or use open-source models from Hugging Face.

---

## 🟢 Optional Services (Nice to Have)

### 7. **SendGrid or Resend**
**Purpose:** Transactional emails (notifications, alerts, receipts)

**What you need:**
- **SendGrid:** `SENDGRID_API_KEY`
- **Resend:** `RESEND_API_KEY`

**Setup steps:**
1. Sign up at [sendgrid.com](https://sendgrid.com) or [resend.com](https://resend.com)
2. Verify your sender domain
3. Create an API key

**Cost:** SendGrid free tier (100 emails/day), Resend free tier (3,000 emails/month)

---

### 8. **Sentry** (Optional)
**Purpose:** Error tracking and monitoring

**What you need:**
- `SENTRY_DSN` - Your Sentry DSN

**Setup steps:**
1. Sign up at [sentry.io](https://sentry.io)
2. Create a new project
3. Copy your DSN

**Cost:** Free tier available (5K errors/month)

---

### 9. **Upstash Redis** (Optional)
**Purpose:** Caching and rate limiting

**What you need:**
- `UPSTASH_REDIS_REST_URL` - Redis REST URL
- `UPSTASH_REDIS_REST_TOKEN` - Redis REST token

**Setup steps:**
1. Sign up at [upstash.com](https://upstash.com)
2. Create a Redis database
3. Copy your REST credentials

**Cost:** Free tier available (10K requests/day)

---

## Environment Variables Summary

Create a `.env.local` file in your project root with these variables:

```bash
# Supabase (Required)
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# Stripe (Required)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
STRIPE_SECRET_KEY=your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret

# Cloudflare (Required)
CLOUDFLARE_ACCOUNT_ID=your_cloudflare_account_id
CLOUDFLARE_API_TOKEN=your_cloudflare_api_token

# Plaid (Optional but Recommended)
PLAID_CLIENT_ID=your_plaid_client_id
PLAID_SECRET=your_plaid_secret
PLAID_ENV=sandbox

# OpenAI (Optional)
OPENAI_API_KEY=your_openai_api_key

# Email (Optional)
SENDGRID_API_KEY=your_sendgrid_api_key
# OR
RESEND_API_KEY=your_resend_api_key

# Monitoring (Optional)
SENTRY_DSN=your_sentry_dsn

# Caching (Optional)
UPSTASH_REDIS_REST_URL=your_upstash_redis_url
UPSTASH_REDIS_REST_TOKEN=your_upstash_redis_token

# App Configuration
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

---

## What to Set Up First

**Minimum to get started (MVP):**
1. ✅ Supabase (database + auth)
2. ✅ Stripe (billing)
3. ✅ Vercel (hosting)
4. ✅ Cloudflare Workers (receipt parsing)

**Add these next for full functionality:**
5. Plaid (bank connections)
6. OpenAI or alternative (AI parsing)

**Add these when scaling:**
7. SendGrid/Resend (emails)
8. Sentry (monitoring)
9. Upstash (caching)

---

## Next Steps

Once you have the required API keys ready, I will:
1. Initialize the Next.js project with the proper scaffold
2. Set up the database schema in Supabase
3. Configure Stripe products and webhooks
4. Deploy the Cloudflare Worker for receipt parsing
5. Build the frontend UI
6. Integrate all services
7. Test the complete flow
8. Prepare deployment documentation

**Please provide the API keys for at least the Critical Services (Supabase, Stripe, Cloudflare) to proceed with the build.**
