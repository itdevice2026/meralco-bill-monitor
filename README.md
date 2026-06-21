# Meralco Bill Monitoring System

A web app to track Meralco electricity consumption and compute **kWh used** and
**cost per kWh** from one billing period to the next. Frontend is a single static
file (`index.html`); the backend is **Supabase** (Postgres + Auth).

---

## Architecture

```
Browser  ──►  index.html (GitHub Pages, free static hosting)
                  │
                  ▼
            Supabase  (project: bom-system)
              • meralco_accounts   — accounts / meters
              • meralco_bills      — billing entries
              kWh and ₱/kWh are computed by the database (generated columns)
```

- Sign-in uses your existing company Supabase account (email + password).
- Data is protected by Row Level Security — only authenticated users can read/write.

---

## Deploy with GitHub Desktop + GitHub Pages

### 1. Put the repo on GitHub
1. Open **GitHub Desktop**.
2. **File → Add Local Repository…** and choose this folder
   (`MERALCO BILL MONITORING`).
3. If it says the folder isn't a Git repository, click
   **"create a repository"** in that prompt → **Create Repository**.
4. Click **Publish repository** (top bar).
   - Name it `meralco-bill-monitor`.
   - Untick *Keep this code private* if you want the simplest Pages setup
     (or keep it private — Pages still works on paid/free plans).
   - **Publish**.

### 2. Turn on GitHub Pages
1. On github.com open the repo → **Settings → Pages**.
2. Under **Build and deployment → Source**, pick **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)** → **Save**.
4. Wait ~1 minute. Your site goes live at:

   ```
   https://itdevice2026.github.io/meralco-bill-monitor/
   ```

### 3. Use it
Open the URL, sign in with your company account, add an account/meter, then
start entering billing readings. Future edits: save the file, **commit** in
GitHub Desktop, **push** — Pages redeploys automatically.

---

## How the numbers work

| Field | Formula |
|-------|---------|
| kWh consumed | `current_reading − previous_reading` |
| Effective rate (₱/kWh) | `total_amount ÷ kWh consumed` |
| Δ kWh / Δ Cost | this period − previous period (month-over-month) |

The previous reading auto-fills from the last saved bill's current reading.

## Features
- Multiple accounts / meters
- Live calculation as you type
- Billing history with month-over-month deltas
- Consumption & cost trend chart
- CSV export
