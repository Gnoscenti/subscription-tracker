# Pricing Strategy: Psychological Framing

## 💰 Pricing Tiers (No Free Tier)

All plans include a **14-day free trial** with full access. No credit card required to start.

---

## Plans

### 🥉 Basic Plan
**$12/month** or **$115/year** (save $29)

**Perfect for individuals with 5-15 subscriptions**

✅ **What's Included:**
- Track unlimited subscriptions
- Manual subscription entry
- Receipt upload & AI parsing
- Daily background monitoring
- Free trial expiration alerts
- Upcoming charge warnings (3-day notice)
- Cancellation guides & direct links
- Savings tracker & dashboard
- Email notifications
- Mobile-responsive web app

❌ **Not Included:**
- Bank account connections
- Email scanning
- Priority support
- Assisted cancellation

**ROI Example:** If you're spending $247/month on subscriptions, you're paying $12 to protect $247 = **2,058% ROI**

---

### 🥈 Pro Plan (Most Popular)
**$24/month** or **$230/year** (save $58)

**Perfect for power users & families with 15+ subscriptions**

✅ **Everything in Basic, plus:**
- 🏦 **Bank account connections** (via Plaid)
- 📧 **Email scanning** for automatic detection
- 📊 **Usage insights** (unused subscription alerts)
- 💰 **Price increase detection**
- 🔔 **Advanced notifications** (push + email + SMS)
- 🎯 **Priority support** (24-hour response)
- 👨‍👩‍👧‍👦 **Family sharing** (up to 5 members)

**ROI Example:** Average Pro user tracks $487/month in subscriptions = **2,029% ROI**

---

### 🥇 Business Plan
**$49/month** or **$470/year** (save $118)

**Perfect for small businesses, teams, and subscription-heavy users**

✅ **Everything in Pro, plus:**
- 🤝 **Assisted cancellation service** (we cancel for you)
- 🎩 **Concierge support** (phone + chat)
- 📈 **Advanced analytics & reporting**
- 🔗 **API access** for integrations
- 👥 **Team management** (unlimited members)
- 🔒 **SSO & advanced security**
- 📊 **Custom reports & exports**
- 🎯 **Dedicated account manager**

**ROI Example:** Average Business user tracks $1,247/month in subscriptions = **2,545% ROI**

---

## Psychological Pricing Tactics

### 1. **Anchoring Effect**
Always show the user's total subscription spend BEFORE showing our price:

```
You're spending $247/month on subscriptions
↓
Protect it all for just $12/month
↓
That's a 2,058% return on investment
```

### 2. **Decoy Pricing**
Pro plan is intentionally positioned as "Most Popular" to make it seem like the best value:

- Basic: $12/month (baseline)
- Pro: $24/month (2x price, 3x value) ← **"Most Popular"**
- Business: $49/month (4x price, 10x value)

Most users will choose Pro because it feels like the "smart" middle option.

### 3. **Annual Discount Framing**
Don't say "Save 20%", say:

```
❌ Annual: $115/year (Save 20%)
✅ Annual: $115/year (Save $29 - that's 2 months free!)
```

People respond better to concrete dollar amounts than percentages.

### 4. **Loss Aversion Framing**
Copy should emphasize what they'll LOSE, not what they'll gain:

```
❌ "Track your subscriptions"
✅ "Stop losing $247/month to forgotten subscriptions"

❌ "Get alerts for free trials"
✅ "Never get charged for a forgotten free trial again"

❌ "Save money on subscriptions"
✅ "Your subscriptions are draining $2,964/year - take it back"
```

### 5. **Social Proof Badges**
Add badges to Pro plan:
- "⭐ Most Popular"
- "👥 Chosen by 67% of users"
- "💎 Best Value"

### 6. **Scarcity (Launch Only)**
For first 1,000 users:
```
🔥 Founder's Pricing: $9/month (instead of $12)
⏰ Only 347 spots left at this price
```

### 7. **Trial Conversion Messaging**

**Day 12 of trial:**
```
Your trial ends in 2 days

You've tracked $247.43/month in subscriptions
We've already saved you from 1 forgotten trial ($15.99)

Keep protecting your money for just $12/month
That's less than one Netflix subscription

[Continue Saving Money] ← Not "Subscribe"
```

**Day 13 of trial:**
```
⚠️ Last day of trial tomorrow

Without us, you'll lose access to:
• 12 tracked subscriptions ($247.43/month)
• Alerts for 3 upcoming free trials
• $1,847 in potential annual savings

Don't lose your financial safety net
[Secure My Savings for $12/month]
```

---

## Pricing Page Copy

### Hero Section
```
# Stop Losing Money to Forgotten Subscriptions

You're probably spending 40% more than you think.
We find every subscription, alert you before charges,
and help you cancel the waste.

[Start 14-Day Free Trial] ← No credit card required
```

### Pricing Table Header
```
# Choose Your Plan

All plans include a 14-day free trial with full access.
Cancel anytime, no questions asked.

[Monthly] [Annual (Save up to $118)] ← Toggle
```

### Below Pricing Table
```
## Why Pay for a Subscription Tracker?

Good question. Here's the math:

Average person has 12 subscriptions = $247/month
3 of those are forgotten or unused = $47/month wasted
That's $564/year down the drain

Our Basic plan costs $144/year
If we help you cancel just ONE $12/month subscription,
you've already made your money back.

Everything else is pure profit in your pocket.

## What Our Users Say

"I found a $39/month gym membership I forgot about 2 years ago.
This app paid for itself 20x over in the first week."
— Sarah M., saved $1,847/year

"The free trial alerts alone are worth it. I've avoided
getting charged for 6 trials I meant to cancel."
— Mike T., saved $247/year

"I thought I was spending $150/month on subscriptions.
Turns out it was $284. Canceled 5 of them immediately."
— Jessica L., saved $1,608/year
```

---

## Referral Program Pricing

### Give $10, Get $10

**How it works:**
1. Share your unique referral link
2. Friend signs up and subscribes (after trial)
3. They get $10 off their first month
4. You get $10 account credit (1 month free)

**Example:**
- Refer 12 friends = 1 year free
- Refer 24 friends = 2 years free

**Viral loop copy:**
```
You saved $247/month by canceling subscriptions.
Help a friend do the same - you both get $10.

[Share My Link]
```

---

## Stripe Product Setup

### Products to Create in Stripe:

**1. Basic Monthly**
- Product Name: "Subscription Tracker - Basic"
- Price: $12/month
- Billing: Recurring monthly
- Trial: 14 days

**2. Basic Annual**
- Product Name: "Subscription Tracker - Basic (Annual)"
- Price: $115/year
- Billing: Recurring yearly
- Trial: 14 days

**3. Pro Monthly**
- Product Name: "Subscription Tracker - Pro"
- Price: $24/month
- Billing: Recurring monthly
- Trial: 14 days

**4. Pro Annual**
- Product Name: "Subscription Tracker - Pro (Annual)"
- Price: $230/year
- Billing: Recurring yearly
- Trial: 14 days

**5. Business Monthly**
- Product Name: "Subscription Tracker - Business"
- Price: $49/month
- Billing: Recurring monthly
- Trial: 14 days

**6. Business Annual**
- Product Name: "Subscription Tracker - Business (Annual)"
- Price: $470/year
- Billing: Recurring yearly
- Trial: 14 days

### Coupons to Create:

**1. Referral Credit**
- Coupon Code: AUTO_GENERATED
- Amount: $10 off
- Duration: Once
- Usage: One per customer

**2. Founder's Pricing (Launch Only)**
- Coupon Code: FOUNDER
- Amount: $3 off monthly ($9 instead of $12)
- Duration: Forever
- Usage: First 1,000 customers only

---

## A/B Testing Ideas

### Test 1: Pricing Levels
- Control: $12 / $24 / $49
- Variant A: $9 / $19 / $39
- Variant B: $15 / $29 / $59

### Test 2: Trial Length
- Control: 14 days
- Variant A: 7 days (more urgency)
- Variant B: 30 days (more time to see value)

### Test 3: Annual Discount
- Control: 20% off (2 months free)
- Variant A: 25% off (3 months free)
- Variant B: 15% off (1.8 months free)

### Test 4: Button Copy
- Control: "Start Free Trial"
- Variant A: "Stop Losing Money"
- Variant B: "Find My Hidden Subscriptions"

---

This pricing strategy is designed to maximize conversions while maintaining a premium positioning. The psychological framing makes the price feel like a no-brainer investment rather than an expense.
