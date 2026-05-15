# Supabase Integration Design for Acrylics

## Overview
This document outlines the design for upgrading the Acrylics Whitelist Application from `localStorage` to a centralized Supabase PostgreSQL database. The application will remain a single `CODE.html` file, utilizing the Supabase JS CDN for data operations while securing admin access via Supabase Auth and Row Level Security (RLS).

## Architecture
- **Frontend**: Vanilla HTML/CSS/JS (single file).
- **Backend**: Supabase (PostgreSQL, Auth, RLS).
- **Client Library**: `@supabase/supabase-js` via CDN.

## Database Schema

### Table: `individual_applications`
- `id` (uuid, primary key)
- `name` (text)
- `wallet` (text)
- `email` (text, nullable)
- `twitter` (text, nullable)
- `discord` (text, nullable)
- `experience` (text)
- `source` (text)
- `why` (text)
- `favorite` (text, nullable)
- `status` (text, default 'pending')
- `created_at` (timestamptz, default now())

### Table: `community_applications`
- `id` (uuid, primary key)
- `name` (text)
- `ctype` (text)
- `members` (text)
- `contact` (text)
- `email` (text, nullable)
- `twitter` (text, nullable)
- `discord` (text, nullable)
- `telegram` (text, nullable)
- `spots` (text)
- `wallet` (text, nullable)
- `platforms` (text)
- `about` (text)
- `why` (text)
- `collab` (text, nullable)
- `status` (text, default 'pending')
- `created_at` (timestamptz, default now())

## Authentication & Security (RLS)
1. **Public Submissions (Anon)**: 
   - Both tables will have an RLS policy allowing `INSERT` for anyone.
   - Public users cannot `SELECT`, `UPDATE`, or `DELETE`.
2. **Admin Dashboard (Authenticated)**:
   - The Admin login UI will require an Email and Password.
   - Using Supabase Auth (`supabase.auth.signInWithPassword`), the admin will receive a session.
   - Both tables will have RLS policies allowing `SELECT`, `UPDATE`, and `DELETE` exclusively for authenticated users (the admin).

## Frontend Implementation Steps
1. **Import Supabase**: Add the Supabase CDN script to the `<head>`.
2. **Initialize Client**: Set up the `supabase` instance using the project URL and Anon Key.
3. **Update Submit Functions**: Rewrite `submitIndividual()` and `submitCommunity()` to asynchronously `INSERT` into their respective tables.
4. **Update Admin Login**: Rewrite `adminLogin()` to use `supabase.auth.signInWithPassword(email, password)`.
5. **Update Admin Dashboard Data**: Rewrite `renderInd()`, `renderCom()`, and `updateStats()` to fetch data via `supabase.from(...).select()`.
6. **Update Admin Actions**: Rewrite status updates (approve/reject/pending) and deletions to asynchronously `UPDATE` or `DELETE` records in Supabase.
