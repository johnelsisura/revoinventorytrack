# ⚙️ Supabase Setup Guide

Follow these steps **before** opening the app for the first time.

---

## Step 1 — Go to Supabase SQL Editor

1. Open [https://supabase.com](https://supabase.com)
2. Go to your project → **SQL Editor**
3. Click **New Query**
4. Copy and paste the SQL below, then click **Run**

---

## Step 2 — Run This SQL

```sql
-- =============================================
-- PUP REVO 2026 — Database Setup
-- Run this entire block in Supabase SQL Editor
-- =============================================

-- USERS TABLE
CREATE TABLE IF NOT EXISTS users (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  username text UNIQUE NOT NULL,
  pin text NOT NULL,
  position text,
  created_at timestamptz DEFAULT now()
);

-- SESSIONS TABLE (cross-device login tokens)
CREATE TABLE IF NOT EXISTS sessions (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id uuid REFERENCES users(id) ON DELETE CASCADE,
  username text NOT NULL,
  token text UNIQUE NOT NULL,
  created_at timestamptz DEFAULT now(),
  expires_at timestamptz DEFAULT now() + interval '7 days'
);

-- SPONSORS TABLE
CREATE TABLE IF NOT EXISTS sponsors (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  company text,
  contact_person text,
  contact_number text,
  email text,
  tier text CHECK (tier IN ('Gold','Silver','Bronze','In-Kind')),
  notes text,
  created_at timestamptz DEFAULT now()
);

-- ITEMS TABLE
CREATE TABLE IF NOT EXISTS items (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  name text NOT NULL,
  variant text,
  category text,
  total_qty integer DEFAULT 0,
  sponsor_id uuid REFERENCES sponsors(id) ON DELETE SET NULL,
  created_at timestamptz DEFAULT now()
);

-- TRANSACTIONS TABLE
CREATE TABLE IF NOT EXISTS transactions (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  item_id uuid REFERENCES items(id) ON DELETE CASCADE,
  sponsor_id uuid REFERENCES sponsors(id) ON DELETE SET NULL,
  qty integer NOT NULL,
  movement_type text CHECK (movement_type IN ('IN','OUT')),
  distribution_type text CHECK (distribution_type IN ('Raffle','Partners','Tokens','Others')),
  given_to_or_by text,
  received_by text,
  date date DEFAULT CURRENT_DATE,
  notes text,
  created_at timestamptz DEFAULT now()
);

-- =============================================
-- DISABLE ROW LEVEL SECURITY
-- (This is an internal tool — no public access)
-- =============================================
ALTER TABLE users DISABLE ROW LEVEL SECURITY;
ALTER TABLE sessions DISABLE ROW LEVEL SECURITY;
ALTER TABLE sponsors DISABLE ROW LEVEL SECURITY;
ALTER TABLE items DISABLE ROW LEVEL SECURITY;
ALTER TABLE transactions DISABLE ROW LEVEL SECURITY;
```

---

## Step 3 — Verify Tables Were Created

After running, go to **Table Editor** in Supabase.

You should see these 5 tables:

- ✅ `users`
- ✅ `sessions`
- ✅ `sponsors`
- ✅ `items`
- ✅ `transactions`

---

## Step 4 — Get Your Project Credentials

Go to **Project Settings** → **API**

You will need:
- **Project URL** → `https://xxxx.supabase.co`
- **Anon public key** → `eyJhbGci...`

These are already hardcoded in `index.html` for PUP REVO 2026.

---

## ✅ Done!

You can now open the app and sign up for your account.

---

## 🔁 Resetting the Database

If you need to start fresh (e.g., for testing):

```sql
-- ⚠️ WARNING: This deletes ALL data
DROP TABLE IF EXISTS transactions;
DROP TABLE IF EXISTS items;
DROP TABLE IF EXISTS sponsors;
DROP TABLE IF EXISTS sessions;
DROP TABLE IF EXISTS users;
```

Then re-run the setup SQL above.

---

## ❓ Common Issues

| Problem | Fix |
|---|---|
| "relation does not exist" error | Tables not created yet — run the SQL above |
| Can't log in after signup | Check if `users` table has your row in Table Editor |
| Data not showing | Check browser console for Supabase errors |
| Session expired | Log out and log back in — tokens last 7 days |
