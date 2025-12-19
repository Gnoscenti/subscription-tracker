# Database Schema V2: Trial-Based Model with Background Monitoring

## Updated Tables for New Strategy

### 1. **users** (Updated)
Added trial tracking and psychological trigger data.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key (Supabase Auth user ID) |
| email | VARCHAR(255) | User email address |
| full_name | VARCHAR(255) | User's full name |
| stripe_customer_id | VARCHAR(255) | Stripe customer ID for billing |
| subscription_status | ENUM | 'trial', 'active', 'canceled', 'past_due' |
| trial_started_at | TIMESTAMP | When trial began |
| trial_ends_at | TIMESTAMP | When trial expires |
| trial_converted | BOOLEAN | Whether trial converted to paid |
| plan_name | VARCHAR(100) | 'basic', 'pro', 'business' |
| plan_price | DECIMAL(10,2) | Monthly price |
| billing_cycle | ENUM | 'monthly', 'annual' |
| total_savings | DECIMAL(10,2) | Total amount saved by canceling subscriptions |
| estimated_monthly_spend | DECIMAL(10,2) | User's estimated subscription spending |
| actual_monthly_spend | DECIMAL(10,2) | Calculated actual spending |
| referral_code | VARCHAR(50) | Unique referral code for this user |
| referred_by | UUID | Foreign key to users.id (nullable) |
| onboarding_completed | BOOLEAN | Whether user finished onboarding |
| onboarding_step | INTEGER | Current step in onboarding (1-7) |
| timezone | VARCHAR(50) | User's timezone for scheduled notifications |
| notification_time | TIME | Preferred time for daily notifications (default 8 AM) |
| last_background_check | TIMESTAMP | Last time background monitoring ran |
| created_at | TIMESTAMP | Account creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 2. **subscriptions** (Updated)
Enhanced with trial tracking and usage monitoring.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| service_name | VARCHAR(255) | Name of the subscription service |
| service_logo_url | TEXT | URL to service logo |
| category | VARCHAR(100) | Category (streaming, software, fitness, etc.) |
| amount | DECIMAL(10,2) | Subscription cost per billing cycle |
| currency | VARCHAR(3) | Currency code (USD, EUR, etc.) |
| billing_cycle | ENUM | 'monthly', 'yearly', 'quarterly', 'weekly' |
| next_billing_date | DATE | Next charge date |
| status | ENUM | 'trial', 'active', 'canceled', 'paused', 'pending_cancellation' |
| is_free_trial | BOOLEAN | Whether this is a free trial subscription |
| trial_ends_at | TIMESTAMP | When free trial ends (nullable) |
| trial_reminder_sent | BOOLEAN | Whether we sent trial ending reminder |
| detection_method | ENUM | 'email_scan', 'bank_connection', 'manual', 'receipt_upload' |
| cancellation_difficulty | ENUM | 'easy', 'medium', 'hard' |
| cancellation_url | TEXT | Direct link to cancel subscription |
| cancellation_instructions | TEXT | Step-by-step cancellation guide |
| last_used_at | TIMESTAMP | Last time user accessed this service (nullable) |
| usage_frequency | INTEGER | Number of times used in last 60 days |
| price_history | JSONB | Array of price changes [{date, old_price, new_price}] |
| notes | TEXT | User notes about this subscription |
| first_detected | TIMESTAMP | When subscription was first detected |
| canceled_at | TIMESTAMP | When subscription was canceled (nullable) |
| savings_amount | DECIMAL(10,2) | Amount saved if canceled |
| created_at | TIMESTAMP | Record creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 3. **background_jobs** (New)
Tracks daily background monitoring tasks.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| job_type | ENUM | 'daily_check', 'trial_alert', 'charge_warning', 'price_change', 'usage_analysis' |
| status | ENUM | 'pending', 'running', 'completed', 'failed' |
| scheduled_for | TIMESTAMP | When job should run |
| started_at | TIMESTAMP | When job started (nullable) |
| completed_at | TIMESTAMP | When job completed (nullable) |
| results | JSONB | Job results and findings |
| error_message | TEXT | Error details if failed (nullable) |
| created_at | TIMESTAMP | Job creation timestamp |

### 4. **alerts** (New)
Stores all user alerts and notifications.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| subscription_id | UUID | Foreign key to subscriptions.id (nullable) |
| alert_type | ENUM | 'trial_ending', 'upcoming_charge', 'price_increase', 'unused_subscription', 'duplicate_detected', 'trial_conversion_reminder' |
| severity | ENUM | 'info', 'warning', 'urgent' |
| title | VARCHAR(255) | Alert title |
| message | TEXT | Alert message |
| action_url | TEXT | URL for primary action (nullable) |
| action_label | VARCHAR(100) | Label for action button (nullable) |
| is_read | BOOLEAN | Whether user has read alert |
| is_dismissed | BOOLEAN | Whether user dismissed alert |
| is_acted_upon | BOOLEAN | Whether user took action |
| notification_sent | BOOLEAN | Whether push/email notification was sent |
| notification_sent_at | TIMESTAMP | When notification was sent (nullable) |
| expires_at | TIMESTAMP | When alert becomes irrelevant (nullable) |
| created_at | TIMESTAMP | Alert creation timestamp |

### 5. **trial_analytics** (New)
Tracks trial user behavior for conversion optimization.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| trial_day | INTEGER | Day of trial (1-14) |
| subscriptions_added | INTEGER | Number of subscriptions added that day |
| logins | INTEGER | Number of logins that day |
| time_spent_seconds | INTEGER | Time spent in app that day |
| features_used | JSONB | Array of features used that day |
| alerts_viewed | INTEGER | Number of alerts viewed |
| alerts_acted_upon | INTEGER | Number of alerts acted upon |
| cancellations_made | INTEGER | Number of subscriptions canceled |
| savings_realized | DECIMAL(10,2) | Money saved that day |
| engagement_score | INTEGER | Calculated engagement score (0-100) |
| created_at | TIMESTAMP | Record creation timestamp |

### 6. **conversion_triggers** (New)
Tracks which psychological triggers led to conversion.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| trigger_type | ENUM | 'loss_aversion', 'social_proof', 'scarcity', 'anchoring', 'endowment', 'reciprocity', 'fomo', 'gamification' |
| trigger_location | VARCHAR(100) | Where trigger was shown (e.g., 'dashboard', 'trial_expiry_modal') |
| was_shown | BOOLEAN | Whether trigger was displayed to user |
| was_clicked | BOOLEAN | Whether user interacted with trigger |
| led_to_conversion | BOOLEAN | Whether this trigger led to subscription |
| shown_at | TIMESTAMP | When trigger was shown |
| clicked_at | TIMESTAMP | When trigger was clicked (nullable) |
| created_at | TIMESTAMP | Record creation timestamp |

### 7. **savings_milestones** (New)
Gamification: Track and celebrate user savings achievements.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| milestone_type | ENUM | 'first_cancellation', 'saved_100', 'saved_500', 'saved_1000', 'trial_saved', 'streak_7', 'streak_30', 'streak_90' |
| milestone_name | VARCHAR(100) | Display name (e.g., "Subscription Slayer") |
| milestone_description | TEXT | Description of achievement |
| badge_icon_url | TEXT | URL to badge icon |
| amount_saved | DECIMAL(10,2) | Amount saved at milestone (nullable) |
| achieved_at | TIMESTAMP | When milestone was achieved |
| is_celebrated | BOOLEAN | Whether user saw celebration modal |
| celebrated_at | TIMESTAMP | When celebration was shown (nullable) |
| created_at | TIMESTAMP | Record creation timestamp |

### 8. **user_preferences** (New)
Stores user notification and monitoring preferences.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id (unique) |
| enable_push_notifications | BOOLEAN | Enable push notifications |
| enable_email_notifications | BOOLEAN | Enable email notifications |
| enable_trial_alerts | BOOLEAN | Alert for free trials ending |
| enable_charge_warnings | BOOLEAN | Alert for upcoming charges |
| enable_price_alerts | BOOLEAN | Alert for price increases |
| enable_usage_insights | BOOLEAN | Alert for unused subscriptions |
| trial_alert_days_before | INTEGER | Days before trial ends to alert (default 2) |
| charge_alert_days_before | INTEGER | Days before charge to alert (default 3) |
| unused_threshold_days | INTEGER | Days of non-use before alerting (default 60) |
| daily_check_enabled | BOOLEAN | Enable daily background monitoring |
| daily_check_time | TIME | Time for daily check (default 8 AM) |
| dark_mode | BOOLEAN | UI dark mode preference |
| created_at | TIMESTAMP | Record creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 9. **ab_tests** (New)
Track A/B test assignments for conversion optimization.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| test_name | VARCHAR(100) | Name of A/B test |
| variant | VARCHAR(50) | Which variant user saw (e.g., 'control', 'variant_a') |
| assigned_at | TIMESTAMP | When user was assigned to variant |
| converted | BOOLEAN | Whether user converted to paid |
| converted_at | TIMESTAMP | When user converted (nullable) |
| created_at | TIMESTAMP | Record creation timestamp |

---

## New Indexes

```sql
-- Users table
CREATE INDEX idx_users_subscription_status ON users(subscription_status);
CREATE INDEX idx_users_trial_ends_at ON users(trial_ends_at);
CREATE INDEX idx_users_last_background_check ON users(last_background_check);

-- Subscriptions table
CREATE INDEX idx_subscriptions_is_free_trial ON subscriptions(is_free_trial);
CREATE INDEX idx_subscriptions_trial_ends_at ON subscriptions(trial_ends_at);
CREATE INDEX idx_subscriptions_next_billing_date ON subscriptions(next_billing_date);
CREATE INDEX idx_subscriptions_user_status ON subscriptions(user_id, status);

-- Background jobs table
CREATE INDEX idx_background_jobs_user_id ON background_jobs(user_id);
CREATE INDEX idx_background_jobs_scheduled_for ON background_jobs(scheduled_for);
CREATE INDEX idx_background_jobs_status ON background_jobs(status);

-- Alerts table
CREATE INDEX idx_alerts_user_id ON alerts(user_id);
CREATE INDEX idx_alerts_is_read ON alerts(is_read);
CREATE INDEX idx_alerts_alert_type ON alerts(alert_type);
CREATE INDEX idx_alerts_expires_at ON alerts(expires_at);

-- Trial analytics table
CREATE INDEX idx_trial_analytics_user_id ON trial_analytics(user_id);
CREATE INDEX idx_trial_analytics_trial_day ON trial_analytics(trial_day);

-- Conversion triggers table
CREATE INDEX idx_conversion_triggers_user_id ON conversion_triggers(user_id);
CREATE INDEX idx_conversion_triggers_led_to_conversion ON conversion_triggers(led_to_conversion);

-- Savings milestones table
CREATE INDEX idx_savings_milestones_user_id ON savings_milestones(user_id);
CREATE INDEX idx_savings_milestones_milestone_type ON savings_milestones(milestone_type);

-- A/B tests table
CREATE INDEX idx_ab_tests_user_id ON ab_tests(user_id);
CREATE INDEX idx_ab_tests_test_name ON ab_tests(test_name);
CREATE INDEX idx_ab_tests_converted ON ab_tests(converted);
```

---

## Background Job Scheduling

### Daily Background Check (Runs at user's preferred time, default 8 AM)

**Job Flow:**
1. Query all users where `last_background_check` < 24 hours ago
2. For each user, check:
   - Free trials ending in next 48 hours
   - Charges coming in next 3 days
   - Subscriptions unused for 60+ days
   - Price changes detected
3. Create alerts in `alerts` table
4. Send notifications (push/email based on preferences)
5. Update `last_background_check` timestamp

**Cron Expression:** `0 0 8 * * *` (8 AM daily, adjusted per user timezone)

---

## Psychological Trigger Tracking

Every time a psychological trigger is shown to a user, log it in `conversion_triggers` table:

```sql
INSERT INTO conversion_triggers (
  user_id,
  trigger_type,
  trigger_location,
  was_shown,
  shown_at
) VALUES (
  'user-uuid',
  'loss_aversion',
  'dashboard_hero',
  true,
  NOW()
);
```

This allows us to analyze which triggers lead to conversions.

---

## Trial Conversion Flow

### Day 1 (Trial Start)
- Create user record with `subscription_status = 'trial'`
- Set `trial_ends_at = NOW() + 14 days`
- Create initial `trial_analytics` record

### Days 2-11 (Engagement Phase)
- Track daily usage in `trial_analytics`
- Show psychological triggers based on behavior
- Send engagement emails if low activity

### Day 12 (First Conversion Prompt)
- Show modal: "Your trial ends in 2 days"
- Display total subscriptions tracked and savings potential
- Create alert with `alert_type = 'trial_conversion_reminder'`

### Day 13 (Urgent Conversion Prompt)
- Show banner: "Last day of trial tomorrow"
- Email: "Don't lose access to your tracked subscriptions"
- Push notification if enabled

### Day 14 (Trial Expiry)
- If not converted: Lock access, show paywall
- If converted: Update `subscription_status = 'active'`, `trial_converted = true`

---

This schema supports all the psychological triggers, background monitoring, and conversion optimization features needed for the new strategy.
