# TrackID3x3 Build Roadmap 🏀

> **From zero to submission in 14 days** — an interactive build roadmap and day-by-day collaborative checklist for participants in the ABC 2027 TrackID3x3 Challenge.

---

## 🚀 Live Demo & GitHub Pages

Once published, this interactive roadmap is accessible at:
```
https://<your-username>.github.io/<your-repository-name>/
```

### How to Enable GitHub Pages (1-Minute Setup)
1. Push this repository to GitHub:
   ```bash
   git add .
   git commit -m "feat: enable shared persistent checklist with Supabase Realtime"
   git push origin main
   ```
2. In your GitHub repository, click **Settings** (top navigation bar).
3. In the left sidebar, click **Pages** (under the "Code and automation" section).
4. Under **Build and deployment**:
   - **Source**: Select `Deploy from a branch`
   - **Branch**: Select `main` (or `master`) and directory `/ (root)`
   - Click **Save**.
5. Within 1–2 minutes, your live site URL will be active!

---

## ✨ Features

- **🌐 Shared, Persistent Checklist (Supabase Cloud Sync & Realtime)**:
  - **Single Source of Truth**: All checkmarks are saved to a PostgreSQL database hosted on Supabase (`checklist_items` table).
  - **Live Realtime Collaboration**: When any visitor checks or unchecks a box, the change appears instantly on other visitors' screens without refreshing via Supabase Realtime webhooks.
  - **Optimistic Updates & Graceful Fallback**: Local UI toggles instantly with zero lag. If an update fails due to network issues, it rolls back gracefully and notifies the user.
  - **Offline & Local Cache**: Works seamlessly even without Supabase credentials using `localStorage` caching.

- **✅ Persistent Checkbox History**:
  - Automatically records every checked and unchecked action with an exact ISO timestamp.
  - Completed steps display a subtle completion timestamp badge (e.g. `✓ Oct 17 · 01:35`).
  - Progress persists reliably across browser refreshes and sessions.

- **📜 Activity History & Audit Log**:
  - Click **History** in the top navigation bar to open the chronological audit timeline.
  - Search and filter history events by step ID, keyword, or action type (Checked / Unchecked).
  - **Copy as Markdown**: Copy your full completed history as formatted Markdown checkboxes to paste into GitHub Issues, PRs, or team standup notes.
  - **Jump to Step**: One-click navigation to highlight and jump to any step in the roadmap.

- **💾 Data Backup & Restore**:
  - **Download Backup**: Export full checklist state and history logs as a portable `.json` file.
  - **Import Backup**: Restore your progress on another device or browser.
  - **Reset**: Safely wipe progress and history locally and on the cloud.

- **🔍 Search & Multi-Filters**:
  - Instant live search across all tasks, descriptions, and commands.
  - Filter chips for *All*, *Not done*, *Completed*, *Needs confirmation*, and *Runs on Colab GPU*.
  - One-click *Expand all* / *Collapse all* toggle.

- **🌓 Theme Switching**:
  - Full support for **Auto (System)**, **Dark**, and **Light** themes with smooth color transitions.

- **🖨️ Print & PDF Ready**:
  - Dedicated `@media print` stylesheets that automatically format the roadmap cleanly for paper or PDF export (hiding navigation buttons and expanding task details).

---

## 🗄️ Supabase Setup Instructions (Shared Database & Realtime)

Follow these steps to connect your roadmap checklist to a shared Supabase PostgreSQL database:

### 1. Create a Free Supabase Project
1. Go to [supabase.com](https://supabase.com/) and sign in (or create a free account).
2. Click **New Project**.
3. Enter a project name (e.g., `trackid3x3-roadmap`), set a secure database password, and choose your preferred region.
4. Click **Create new project** and wait 1–2 minutes for the database to provision.

### 2. Run the Database Schema & Realtime SQL
1. In your Supabase Dashboard, click **SQL Editor** in the left sidebar.
2. Click **New query**.
3. Open [`supabase-schema.sql`](supabase-schema.sql) from this repository, copy its entire contents, and paste into the SQL Editor.
4. Click **Run** (or press `Ctrl+Enter`).
5. This script automatically:
   - Creates the `checklist_items` table (`id TEXT PRIMARY KEY`, `checked BOOLEAN`, `updated_at TIMESTAMPTZ`).
   - Enables **Row Level Security (RLS)**.
   - Configures public policies allowing `SELECT`, `UPDATE`, and `INSERT` (upsert).
   - Adds `checklist_items` to `supabase_realtime` publication with `REPLICA IDENTITY FULL`.
   - Seeds all 336 checklist step records (idempotent, won't overwrite existing progress).

### 3. Obtain Credentials
1. In your Supabase Dashboard, go to **Project Settings** (gear icon) → **API**.
2. Copy:
   - **Project URL** (e.g. `https://your-project-id.supabase.co`)
   - **Project API Keys** → `anon` `public` key (e.g. `eyJhbGciOiJIUzI1Ni...`)

> [!WARNING]
> **SECURITY RULE**:
> ONLY use your `anon` `public` key in the frontend. **NEVER** expose or commit your `service_role` secret key! The `service_role` key bypasses Row Level Security and must remain strictly private.

### 4. Configure Credentials

You can configure credentials in either of two ways:

#### Option A: In the Web Application UI (Recommended for quick testing)
1. Open your roadmap in the browser.
2. Click **Cloud Sync** in the top navigation bar.
3. Paste your **Project URL** and **Publishable / Anon Key**.
4. Click **⚡ Test Connection** to verify database connectivity.
5. Click **💾 Save & Connect**. Credentials are securely saved in your browser's local storage.

#### Option B: In `supabase-config.js` (Recommended for GitHub Pages deployment)
1. Open [`supabase-config.js`](supabase-config.js) in your repository.
2. Enter your credentials:
   ```javascript
   window.SUPABASE_CONFIG = {
     url: "https://your-project-id.supabase.co",
     anonKey: "your-anon-publishable-key-here"
   };
   ```
3. Commit and push to GitHub:
   ```bash
   git add supabase-config.js
   git commit -m "config: set Supabase project credentials for live sync"
   git push origin main
   ```
   *(Note: Since the `anon` key only has restricted permissions defined by RLS policies, it is designed for public client-side use. If you prefer to keep your repository completely keyless, leave `supabase-config.js` blank and use Option A).*

---

## 🔒 Security & Row Level Security (RLS) Policies

The checklist items table is secured using PostgreSQL Row Level Security (RLS):

```sql
-- Enable RLS
ALTER TABLE public.checklist_items ENABLE ROW LEVEL SECURITY;

-- Allow public read access
CREATE POLICY "Allow public read checklist items"
    ON public.checklist_items FOR SELECT TO public, anon, authenticated USING (true);

-- Allow public update access (checkbox toggle)
CREATE POLICY "Allow public update checklist items"
    ON public.checklist_items FOR UPDATE TO public, anon, authenticated USING (true) WITH CHECK (true);

-- Allow public insert access (required for upsert / initial setup)
CREATE POLICY "Allow public insert checklist items"
    ON public.checklist_items FOR INSERT TO public, anon, authenticated WITH CHECK (true);
```

### Abuse Protection & Hardening Recommendations
- **Public Collaborative Mode**: By design, anyone visiting the website can view and toggle checkboxes without logging in.
- **Restricted Privileges**: Notice that the `DELETE` privilege is **NOT** granted to public/anon users. Visitors cannot delete rows from the table.
- **Team Authentication (Optional)**: If you want only team members to modify the checklist:
  1. Enable Supabase Auth (Email/Password or GitHub OAuth).
  2. Modify the RLS policies in `supabase-schema.sql` so that `UPDATE` is restricted to `authenticated` users:
     ```sql
     CREATE POLICY "Allow authenticated update only"
         ON public.checklist_items FOR UPDATE TO authenticated USING (true) WITH CHECK (true);
     ```
  3. Non-logged-in visitors will still be able to view progress, but only logged-in team members can change checkmarks.

---

## 📁 Repository Structure

```
├── index.html                     # Primary entrypoint for GitHub Pages with Supabase sync
├── TrackID3x3 Build Roadmap.html  # Standalone mirrored copy
├── supabase-schema.sql            # PostgreSQL schema, RLS policies, Realtime publication & seed data
├── supabase-config.js             # Supabase client credentials configuration
├── supabase-config.example.js     # Template configuration file
├── .nojekyll                      # Disables Jekyll processing on GitHub Pages
├── .gitignore                     # Git ignore rules for node_modules and secrets
└── README.md                      # Documentation, SQL guide, and deployment instructions
```

---

## 🛠️ Tech Stack

- **HTML5 & Vanilla CSS3**: Custom properties, dark/light modes, glassmorphism, responsive layout.
- **Vanilla JavaScript**: Pure modern JavaScript, no build step or node server needed.
- **Supabase PostgreSQL & Supabase JS v2 Client**: Loaded via CDN, provides database persistence and Realtime replication.
- **GitHub Pages**: 100% static hosting compatibility.
