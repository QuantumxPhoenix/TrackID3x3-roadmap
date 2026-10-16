# TrackID3x3 Build Roadmap 🏀

> **From zero to submission in 14 days** — an interactive build roadmap and day-by-day checklist for participants in the ABC 2027 TrackID3x3 Challenge.

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
   git commit -m "feat: publish TrackID3x3 interactive roadmap with checkbox history"
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

- **✅ Persistent Checkbox History**:
  - Automatically records every checked and unchecked action with an exact ISO timestamp.
  - Completed steps display a subtle completion timestamp badge (e.g. `✓ Oct 17 · 01:35`).
  - Progress persists reliably across browser refreshes and sessions via `localStorage`.

- **📜 Activity History & Audit Log**:
  - Click **History** in the top navigation bar to open the chronological audit timeline.
  - Search and filter history events by step ID, keyword, or action type (Checked / Unchecked).
  - **Copy as Markdown**: Copy your full completed history as formatted Markdown checkboxes to paste into GitHub Issues, PRs, or team standup notes.
  - **Jump to Step**: One-click navigation to highlight and jump to any step in the roadmap.

- **💾 Data Backup & Restore**:
  - **Download Backup**: Export full checklist state and history logs as a portable `.json` file.
  - **Import Backup**: Restore your progress on another device or browser.
  - **Reset**: Safely wipe progress and history if restarting.

- **🔍 Search & Multi-Filters**:
  - Instant live search across all tasks, descriptions, and commands.
  - Filter chips for *All*, *Not done*, *Completed*, *Needs confirmation*, and *Runs on Colab GPU*.
  - One-click *Expand all* / *Collapse all* toggle.

- **🌓 Theme Switching**:
  - Full support for **Auto (System)**, **Dark**, and **Light** themes with smooth color transitions.

- **🖨️ Print & PDF Ready**:
  - Dedicated `@media print` stylesheets that automatically format the roadmap cleanly for paper or PDF export (hiding navigation buttons and expanding task details).

---

## 📁 Repository Structure

```
├── index.html                     # Primary entrypoint for GitHub Pages
├── TrackID3x3 Build Roadmap.html  # Standalone mirrored copy
├── .nojekyll                      # Disables Jekyll processing on GitHub Pages
├── README.md                      # Documentation and deployment guide
└── sample.py                      # Python starter script
```

---

## 🛠️ Tech Stack

- **HTML5** & Semantic Elements
- **Vanilla CSS3** (Custom properties, dark mode, glassmorphic modals)
- **Vanilla JavaScript** (`localStorage`, File API, Clipboard API, responsive dialogs)
- **Zero external dependencies** (Google Fonts loaded asynchronously)
# TrackID3x3-roadmap
