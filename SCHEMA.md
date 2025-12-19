# Database Schema Design

## Overview
This document outlines the database schema for the Subscription Tracker application, designed to help users identify, track, and cancel unwanted subscriptions.

## Core Tables

### 1. **users**
Stores user account information and authentication data.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key (Supabase Auth user ID) |
| email | VARCHAR(255) | User email address |
| full_name | VARCHAR(255) | User's full name |
| stripe_customer_id | VARCHAR(255) | Stripe customer ID for billing |
| subscription_tier | ENUM | Current plan: 'free', 'basic', 'pro', 'premium' |
| total_savings | DECIMAL(10,2) | Total amount saved by canceling subscriptions |
| referral_code | VARCHAR(50) | Unique referral code for this user |
| referred_by | UUID | Foreign key to users.id (nullable) |
| created_at | TIMESTAMP | Account creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 2. **subscriptions**
Tracks all user subscriptions (both active and canceled).

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
| status | ENUM | 'active', 'canceled', 'paused', 'pending_cancellation' |
| detection_method | ENUM | 'email_scan', 'bank_connection', 'manual', 'receipt_upload' |
| cancellation_difficulty | ENUM | 'easy', 'medium', 'hard' |
| cancellation_url | TEXT | Direct link to cancel subscription |
| notes | TEXT | User notes about this subscription |
| first_detected | TIMESTAMP | When subscription was first detected |
| canceled_at | TIMESTAMP | When subscription was canceled (nullable) |
| created_at | TIMESTAMP | Record creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 3. **receipts**
Stores parsed receipt data from email/PDF uploads.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| subscription_id | UUID | Foreign key to subscriptions.id (nullable) |
| source_type | ENUM | 'email', 'pdf', 'image' |
| raw_content | TEXT | Original receipt content |
| parsed_data | JSONB | Structured parsed data |
| merchant_name | VARCHAR(255) | Extracted merchant name |
| amount | DECIMAL(10,2) | Extracted amount |
| transaction_date | DATE | Extracted transaction date |
| processing_status | ENUM | 'pending', 'processed', 'failed' |
| created_at | TIMESTAMP | Upload timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 4. **user_billing**
Tracks user's subscription to our service (via Stripe).

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| stripe_subscription_id | VARCHAR(255) | Stripe subscription ID |
| plan_name | VARCHAR(100) | Plan name (Basic, Pro, Premium) |
| plan_price | DECIMAL(10,2) | Monthly price |
| status | ENUM | 'active', 'canceled', 'past_due', 'trialing' |
| current_period_start | TIMESTAMP | Billing period start |
| current_period_end | TIMESTAMP | Billing period end |
| cancel_at_period_end | BOOLEAN | Whether subscription will cancel at end |
| created_at | TIMESTAMP | Subscription start timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 5. **referrals**
Tracks referral program activity and rewards.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| referrer_id | UUID | Foreign key to users.id (person who referred) |
| referred_id | UUID | Foreign key to users.id (person who was referred) |
| reward_type | ENUM | 'account_credit', 'discount', 'free_month' |
| reward_amount | DECIMAL(10,2) | Monetary value of reward |
| reward_status | ENUM | 'pending', 'applied', 'expired' |
| stripe_coupon_id | VARCHAR(255) | Stripe coupon ID if applicable |
| created_at | TIMESTAMP | Referral timestamp |
| applied_at | TIMESTAMP | When reward was applied (nullable) |

### 6. **cancellation_requests**
Tracks user requests for help canceling difficult subscriptions.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| subscription_id | UUID | Foreign key to subscriptions.id |
| status | ENUM | 'pending', 'in_progress', 'completed', 'failed' |
| request_type | ENUM | 'self_service', 'assisted', 'concierge' |
| notes | TEXT | User notes or special instructions |
| admin_notes | TEXT | Internal notes from support team |
| created_at | TIMESTAMP | Request creation timestamp |
| completed_at | TIMESTAMP | When cancellation was completed (nullable) |
| updated_at | TIMESTAMP | Last update timestamp |

### 7. **bank_connections**
Stores bank/financial account connections (via Plaid or similar).

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| plaid_item_id | VARCHAR(255) | Plaid item ID |
| plaid_access_token | TEXT | Encrypted Plaid access token |
| institution_name | VARCHAR(255) | Bank/institution name |
| institution_id | VARCHAR(255) | Plaid institution ID |
| status | ENUM | 'active', 'disconnected', 'error' |
| last_sync | TIMESTAMP | Last successful sync timestamp |
| created_at | TIMESTAMP | Connection creation timestamp |
| updated_at | TIMESTAMP | Last update timestamp |

### 8. **notifications**
Stores user notifications and alerts.

| Column | Type | Description |
|--------|------|-------------|
| id | UUID | Primary key |
| user_id | UUID | Foreign key to users.id |
| type | ENUM | 'upcoming_charge', 'price_increase', 'new_subscription_detected', 'savings_milestone' |
| title | VARCHAR(255) | Notification title |
| message | TEXT | Notification message |
| related_subscription_id | UUID | Foreign key to subscriptions.id (nullable) |
| is_read | BOOLEAN | Whether user has read notification |
| created_at | TIMESTAMP | Notification creation timestamp |

## Indexes

```sql
-- Users table
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_referral_code ON users(referral_code);
CREATE INDEX idx_users_stripe_customer_id ON users(stripe_customer_id);

-- Subscriptions table
CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_status ON subscriptions(status);
CREATE INDEX idx_subscriptions_next_billing_date ON subscriptions(next_billing_date);
CREATE INDEX idx_subscriptions_user_status ON subscriptions(user_id, status);

-- Receipts table
CREATE INDEX idx_receipts_user_id ON receipts(user_id);
CREATE INDEX idx_receipts_subscription_id ON receipts(subscription_id);
CREATE INDEX idx_receipts_processing_status ON receipts(processing_status);

-- User billing table
CREATE INDEX idx_user_billing_user_id ON user_billing(user_id);
CREATE INDEX idx_user_billing_stripe_subscription_id ON user_billing(stripe_subscription_id);

-- Referrals table
CREATE INDEX idx_referrals_referrer_id ON referrals(referrer_id);
CREATE INDEX idx_referrals_referred_id ON referrals(referred_id);

-- Cancellation requests table
CREATE INDEX idx_cancellation_requests_user_id ON cancellation_requests(user_id);
CREATE INDEX idx_cancellation_requests_status ON cancellation_requests(status);

-- Bank connections table
CREATE INDEX idx_bank_connections_user_id ON bank_connections(user_id);

-- Notifications table
CREATE INDEX idx_notifications_user_id ON notifications(user_id);
CREATE INDEX idx_notifications_is_read ON notifications(is_read);
```

## Row Level Security (RLS) Policies

All tables will implement RLS policies to ensure users can only access their own data:

```sql
-- Example for subscriptions table
ALTER TABLE subscriptions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view their own subscriptions"
  ON subscriptions FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert their own subscriptions"
  ON subscriptions FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update their own subscriptions"
  ON subscriptions FOR UPDATE
  USING (auth.uid() = user_id);

CREATE POLICY "Users can delete their own subscriptions"
  ON subscriptions FOR DELETE
  USING (auth.uid() = user_id);
```

Similar policies will be applied to all user-specific tables.

## Relationships

- `users` ← `subscriptions` (one-to-many)
- `users` ← `receipts` (one-to-many)
- `users` ← `user_billing` (one-to-one)
- `users` ← `referrals` (one-to-many as referrer)
- `users` ← `referrals` (one-to-many as referred)
- `users` ← `cancellation_requests` (one-to-many)
- `users` ← `bank_connections` (one-to-many)
- `users` ← `notifications` (one-to-many)
- `subscriptions` ← `receipts` (one-to-many)
- `subscriptions` ← `cancellation_requests` (one-to-many)
