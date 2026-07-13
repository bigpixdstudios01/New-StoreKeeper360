# StoreKeeper360 — Retail Inventory Manager (Consolidated Edition)

A full-stack retail inventory, sales, and profit/loss tracking app — now consolidated into just **two code files** to make uploading and editing from a phone as simple as possible.

---

## File Structure

```
storekeeper360/
├── index.html          ← EVERYTHING frontend: HTML, CSS, JS, logo, all inline
├── api/
│   └── index.js        ← EVERYTHING backend: all routes, all logic, one file
├── vercel.json          ← Routes all traffic to api/index.js
├── package.json
├── .env                 ← Your real Supabase credentials (never upload this)
├── .env.example          ← Safe template version
├── .gitignore
└── supabase/
    └── schema.sql        ← Run this once in Supabase's SQL Editor (not app code)
```

That's it. Only **one folder** (`api/`) plus the one-time `supabase/schema.sql` setup script — everything else is a single file at the root.

---

## Why This Structure

- **`index.html`** contains all your CSS (inside a `<style>` tag), all your JavaScript (inside a `<script>` tag), and your logo (as inline SVG) — no separate `style.css`, `app.js`, `config.js`, or `logo.svg` files to lose track of.
- **`api/index.js`** contains every backend route — auth/profile, products, sales, reports, settings — all in one file. Vercel automatically treats anything inside `/api` as a serverless function.
- Your Supabase URL and anon key are already filled in directly inside `index.html` (they're meant to be public — that's how Supabase's anon key works). Your secret service_role key lives only in `.env` / Vercel's environment variables, never in `index.html`.

---

## Local Testing

```bash
npm install
npm start
```
Visit **http://localhost:3000**

---

## Deploying to Vercel

1. Push this project to GitHub (see notes below on doing this from a phone)
2. Go to **vercel.com** → **Add New → Project** → import your repo
3. Add these **Environment Variables** in Vercel's project settings:
   ```
   SUPABASE_URL = https://jgpnmdsspzrgxrnpofwc.supabase.co
   SUPABASE_SERVICE_ROLE_KEY = (your service_role key — get it again from Supabase if needed)
   ```
4. **Deploy**
5. Copy your live URL, then in Supabase go to **Authentication → URL Configuration → Redirect URLs** and add your live URL (e.g. `https://your-app.vercel.app`) — this is required for the password reset flow. Since password reset is now handled on the same `index.html` page (no separate `reset-password.html`), just add your bare app URL, not a `/reset-password.html` path.

---

## Pushing From a Phone (Termux)

```bash
cd storekeeper360
git init
git add .
git status   # confirm .env is NOT listed — it should be gitignored
git commit -m "Consolidated single-file structure"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main --force
```

⚠️ **Double-check `git status` does not list `.env`** before committing — it contains your real secret key and must never reach GitHub. The included `.gitignore` already excludes it, but it's worth confirming.

---

## Database Setup (One-Time)

If you haven't already run this on your Supabase project:
1. Supabase Dashboard → **SQL Editor** → **New Query**
2. Paste the entire contents of `supabase/schema.sql`
3. **Run**

This is safe to re-run even if you already ran an earlier version of this schema.

---

## Security Reminder

You shared your `service_role` key in this conversation to have it added to `.env`. That's fine for your own private setup, but as good practice going forward:
- Never paste your service_role key into a public GitHub issue, forum post, or screenshot
- If you ever suspect it's been exposed, regenerate it from **Supabase → Project Settings → API → service_role → Reset**
