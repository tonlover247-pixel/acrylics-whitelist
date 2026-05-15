# Supabase Integration Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Upgrade the Acrylics Whitelist form to use Supabase PostgreSQL via the CDN for data storage and Admin Auth.

**Architecture:** Single HTML file using the `@supabase/supabase-js` CDN. Form submissions are inserted directly. Admin authenticates via Supabase Auth to read and manage submissions securely via RLS.

**Tech Stack:** HTML/JS, Supabase (PostgreSQL, Auth, Row Level Security)

---

### Task 1: Add Supabase CDN and Initialize Client

**Files:**
- Modify: `/Users/israeloyewole/Desktop/ACRYLICS/CODE.html`

**Step 1: Add Supabase JS Script**
Insert the Supabase CDN script inside the `<head>` of `CODE.html`.

**Step 2: Initialize Supabase Client**
Replace the local config constants with Supabase initialization at the top of the `<script>` section. Define placeholder constants `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY`.
```javascript
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
const supabase = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
```

**Step 3: Commit**
```bash
git add CODE.html
git commit -m "feat: add supabase CDN and initialize client"
```

### Task 2: Update Application Submission Logic

**Files:**
- Modify: `/Users/israeloyewole/Desktop/ACRYLICS/CODE.html`

**Step 1: Refactor Submission Logic**
Update `submitIndividual()` and `submitCommunity()` to be `async`. Remove `localStorage` logic (`saveInd`, `saveCom`). Use `await supabase.from('individual_applications').insert([app])` (and similarly for community apps). Add a `try/catch` block for error handling.

**Step 2: Commit**
```bash
git add CODE.html
git commit -m "feat: use supabase for form submissions"
```

### Task 3: Update Admin Login and Session Management

**Files:**
- Modify: `/Users/israeloyewole/Desktop/ACRYLICS/CODE.html`

**Step 1: Update Login UI**
Change the single password input in the `.admin-gate` section to require both **Email** and **Password** inputs.

**Step 2: Refactor Admin Login Logic**
Make `adminLogin()` async. Use `await supabase.auth.signInWithPassword({ email, password })`. Handle invalid credentials gracefully. Upon success, load the dashboard.
Update `adminLogout()` to `await supabase.auth.signOut()` and handle the UI reset.

**Step 3: Commit**
```bash
git add CODE.html
git commit -m "feat: implement supabase auth for admin"
```

### Task 4: Update Admin Dashboard Data Fetching

**Files:**
- Modify: `/Users/israeloyewole/Desktop/ACRYLICS/CODE.html`

**Step 1: Fetch from Supabase**
Remove `getInd()` and `getCom()` local storage reads. Create global arrays `indData = []` and `comData = []`. Create async fetch functions `fetchInd()` and `fetchCom()` that run `await supabase.from('...').select('*')` and update the global arrays.

**Step 2: Update Render Functions**
Update `renderAll()` to be async, call the fetch functions, and then call the UI renderers (`updateStats`, `renderInd`, `renderCom`). The render functions will now read from the global arrays instead of `localStorage`.

**Step 3: Commit**
```bash
git add CODE.html
git commit -m "feat: fetch dashboard data from supabase"
```

### Task 5: Update Admin Actions (Approve/Reject/Delete)

**Files:**
- Modify: `/Users/israeloyewole/Desktop/ACRYLICS/CODE.html`

**Step 1: Update Status and Delete Logic**
Refactor `setIStatus`, `deleteInd`, `setCStatus`, `deleteCom`, and `approveFiltered` to execute async Supabase mutations:
- `await supabase.from('...').update({status}).eq('id', id)`
- `await supabase.from('...').delete().eq('id', id)`
After the async mutation, re-fetch data and re-render.

**Step 2: Commit**
```bash
git add CODE.html
git commit -m "feat: wire up admin actions to supabase"
```
