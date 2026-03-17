# monday.com Column Audit Tool

Scans your monday.com workspace and reports column fill rates so you can identify unused columns to clean up.

---

## Setup

### 1. Push to GitHub

Create a new repo on GitHub and push this folder:

```bash
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/YOUR_USERNAME/monday-column-audit.git
git push -u origin main
```

### 2. Deploy to Netlify

1. Go to [netlify.com](https://netlify.com) → **Add new site** → **Import an existing project**
2. Connect your GitHub repo
3. Build settings are auto-detected from `netlify.toml` — no changes needed
4. Click **Deploy**

### 3. Set your monday.com API key

In Netlify: **Site settings** → **Environment variables** → **Add a variable**

| Key | Value |
|-----|-------|
| `MONDAY_API_KEY` | Your monday.com API token |

To get your API token: monday.com → your avatar (top right) → **Administration** → **API** → copy the v2 token.

Then trigger a redeploy: **Deploys** → **Trigger deploy** → **Deploy site**.

---

## How it works

- The frontend calls `/api/monday` which routes to a Netlify serverless function
- The function holds your API key server-side and proxies requests to `api.monday.com/v2`
- Scans up to 50 boards, samples up to 100 items per board
- Skips system columns (name, IDs, auto-number, etc.)
- Calculates fill rate: % of items with a non-empty value in each column

## Recommendations

| Fill rate | Suggestion |
|-----------|-----------|
| 0% | Consider deleting — no data at all |
| Below threshold (default 20%) | Review — very low usage |
| At or above threshold | Keep |

The threshold is adjustable in the UI. Export results to CSV for sharing.
