# Deployment Guide — GitHub Pages

## Prerequisites
- A GitHub account ([github.com](https://github.com))
- Git installed on your computer
- Claude Code installed (optional — for auto-sync)

---

## Method 1 — GitHub Website (Easiest, no terminal needed)

### Step 1 — Create a new GitHub repository
1. Go to [github.com/new](https://github.com/new)
2. **Repository name**: `nepse-dashboard`
3. **Visibility**: Public ← required for free GitHub Pages
4. ✅ Check **"Add a README file"**
5. Click **"Create repository"**

### Step 2 — Upload the dashboard file
1. Inside your new repository, click **"Add file"** → **"Upload files"**
2. Drag and drop `index.html` from your computer into the upload area
3. In the **"Commit changes"** section at the bottom, type: `Add NEPSE dashboard`
4. Click **"Commit changes"**

### Step 3 — Enable GitHub Pages
1. Click **"Settings"** tab (top of repository)
2. In the left sidebar, click **"Pages"**
3. Under **"Source"**, select:
   - Branch: `main`
   - Folder: `/ (root)`
4. Click **"Save"**
5. Wait 1–2 minutes, then your site will be live at:
   ```
   https://YOUR-USERNAME.github.io/nepse-dashboard
   ```

### Step 4 — Update the dashboard in future
1. Go to your repository on GitHub
2. Click `index.html`
3. Click the **pencil icon** (Edit)
4. Delete all content and paste your new `index.html` content
5. Click **"Commit changes"**
6. GitHub Pages auto-deploys within 1–2 minutes

---

## Method 2 — Terminal / Command Line (Recommended)

### Step 1 — Install Git
- **Windows**: Download from [git-scm.com](https://git-scm.com/download/win)
- **macOS**: Run `xcode-select --install` in Terminal
- **Linux (Ubuntu/Debian)**: `sudo apt install git`

Verify: `git --version`

### Step 2 — Configure Git (first time only)
```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
```

### Step 3 — Create repository on GitHub
1. Go to [github.com/new](https://github.com/new)
2. Name: `nepse-dashboard`, Public, **no README** (we'll push our own)
3. Click **"Create repository"**
4. Copy the repository URL shown — looks like:
   `https://github.com/YOUR-USERNAME/nepse-dashboard.git`

### Step 4 — Push from your computer
```bash
# Navigate to where you saved the project
cd /path/to/nepse-dashboard

# Initialize git
git init

# Add all files
git add .

# First commit
git commit -m "Initial NEPSE Wyckoff dashboard"

# Connect to GitHub (paste YOUR repository URL)
git remote add origin https://github.com/YOUR-USERNAME/nepse-dashboard.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 5 — Enable GitHub Pages
Same as Method 1, Step 3 above.

### Step 6 — Update in future (after making changes)
```bash
cd /path/to/nepse-dashboard
git add index.html
git commit -m "Update NEPSE data and features"
git push
```
GitHub Pages auto-deploys within 1–2 minutes after each push.

---

## Method 3 — Claude Code (Automated sync)

Claude Code lets Claude directly push changes to your GitHub repository.

### Step 1 — Install Claude Code
```bash
npm install -g @anthropic-ai/claude-code
```

### Step 2 — Authenticate Claude Code with GitHub
```bash
claude-code auth github
```
Follow the browser prompt to authorise Claude Code access to your repositories.

### Step 3 — Set up the project
```bash
# In your project folder
cd /path/to/nepse-dashboard
claude-code init
```

### Step 4 — Let Claude push changes
In any Claude conversation, tell Claude:

> "Push the updated nepse_wyckoff_analysis.html to my GitHub repo nepse-dashboard as index.html"

Claude Code will:
1. Copy the updated file
2. Commit with a descriptive message
3. Push to your `main` branch
4. GitHub Pages deploys automatically

---

## Method 4 — Auto-deploy with GitHub Actions

This automatically rebuilds the page every weekday at 4 PM NPT (after market close).

Create file `.github/workflows/deploy.yml` in your repository:

```yaml
name: Deploy NEPSE Dashboard

on:
  push:
    branches: [main]
  schedule:
    # Run Mon-Fri at 4:00 PM NPT (10:15 AM UTC = UTC+5:45)
    - cron: '15 10 * * 1-5'
  workflow_dispatch:  # Allow manual trigger

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
          publish_branch: gh-pages
```

To create this file on GitHub:
1. In your repository, click **"Add file"** → **"Create new file"**
2. Filename: `.github/workflows/deploy.yml`
3. Paste the YAML above
4. Commit the file

---

## After Deployment

### Your live URL
```
https://YOUR-USERNAME.github.io/nepse-dashboard
```

### Share with team
Send the URL to anyone — they can open it in any browser. The dashboard will:
1. Load instantly with last known NEPSE data
2. Auto-fetch live data via Claude API (15–30 sec)
3. Show correct date based on their device's clock and NPT timezone

### Custom domain (optional)
1. In Settings → Pages, add your custom domain (e.g. `nepse.lscapital.com.np`)
2. At your DNS provider, add a CNAME record pointing to `YOUR-USERNAME.github.io`
3. GitHub Pages will auto-provision an SSL certificate

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Page shows 404 | Wait 2 min after enabling Pages. Check branch is `main` and folder is `/ (root)` |
| Live data not loading | The Claude API only works when opened through Claude.ai. For standalone use, fallback data always shows |
| Changes not appearing | Hard refresh: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac) |
| Git push rejected | Run `git pull origin main --rebase` then push again |
| "Permission denied" | Make sure repository is Public, not Private |
