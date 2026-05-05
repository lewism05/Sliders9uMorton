# Arkansas Sliders 9U — Coaching Dashboard

A live coaching dashboard for the Arkansas Sliders. One coach (you) edits, every other coach sees the live data on their phone — no apps, no logins.

## What's in here

| File | What it does |
| --- | --- |
| `index.html` | The dashboard itself. Open it on any device. |
| `data.json` | The published season data. Auto-updated by the dashboard. Don't edit by hand. |
| `netlify.toml` | Netlify config — disables CDN caching on `data.json` so updates show up immediately. |

## How it works

```
┌──────────────┐      edit       ┌──────────┐    auto-deploy   ┌───────────┐
│   Editor     │ ──────────────► │  GitHub  │ ───────────────► │  Netlify  │
│  (you)       │   commits       │  (this   │   on push        │  (CDN)    │
│              │   data.json     │   repo)  │                  │           │
└──────────────┘                 └──────────┘                  └─────┬─────┘
                                                                     │
                                                                     │ fetches /data.json
                                                                     ▼
                                                              ┌─────────────┐
                                                              │  Coaches'   │
                                                              │  phones     │
                                                              │ (read-only) │
                                                              └─────────────┘
```

## First-time setup (~5 minutes)

### 1. Push this repo to GitHub
Create a new GitHub repo and push these three files (`index.html`, `data.json`, `netlify.toml`) to it.

### 2. Connect to Netlify
- Log into Netlify, click **Add new site → Import an existing project**.
- Pick this GitHub repo. Leave defaults — Netlify reads `netlify.toml` automatically.
- Wait ~30 seconds for the first deploy.
- You'll get a URL like `https://your-team.netlify.app/`.

### 3. Connect the dashboard to your repo (the editor — you, one device)
- Open the Netlify URL in your browser.
- Tap the **📡 Set up publish** pill in the top bar.
- Follow the 5 steps in the modal:
  1. Open `github.com/settings/personal-access-tokens/new`
     - **Repository access:** *Only select repositories* → choose this repo
     - **Permissions:** *Repository → Contents → Read and write*
     - Click **Generate token**, copy it.
  2. Paste your repo (e.g. `your-username/sliders-dashboard`).
  3. Path: `data.json` (default).
  4. Branch: `main` (default).
  5. Paste the token.
- Click **Connect & Publish**. You should see "Connected!" and the pill turns green.

### 4. Upload your stats and edit
- Click **Upload Stats**, drop in your CSVs.
- Edit lineups, set opponent strengths — every change auto-publishes after 5 seconds.

### 5. Send the URL to your coaches
- Just text them the Netlify URL. That's it.
- They'll see the **Live View · Read-Only** banner with a Refresh button.
- They can't break anything — editing UI is hidden.

## Updating the data later
- Open the URL on any device where you've configured publishing (i.e. your phone or laptop).
- Make changes. They auto-publish.
- ~30-60 seconds later, every coach refreshing the URL sees the new lineup, schedule, etc.
- A "🟢 Published 30s ago" indicator confirms the last successful publish.

## Adding more "editor" devices
- Repeat **Step 3** on each device (your phone, your tablet, etc.). Each device needs its own copy of the token in localStorage. Token never leaves the browser.
- Or just keep editing from one device — simpler.

## Troubleshooting

| Issue | Fix |
| --- | --- |
| Sync pill says "⚠ Token invalid" | Token expired or repo permissions changed. Generate a new fine-grained PAT. |
| Coach sees "Updated 3 days ago" | They need to refresh — tap the **Refresh** button in the banner, or pull to refresh the browser tab. |
| Dashboard shows old data after publishing | Wait 30-60s for Netlify to rebuild. Check the **Deploys** tab on Netlify to see if it's still building. |
| Want to roll back a bad publish | Each publish is a git commit. Revert it in GitHub, Netlify auto-redeploys. |

## Privacy / security notes
- The PAT only ever lives in your browser's localStorage on devices where you set it up. It is sent only to `api.github.com`.
- The PAT is scoped to *only* this repo with Contents:Write — it can't touch any other GitHub data.
- The published data is a JSON file in your public repo. Don't put info in lineups that you don't want public.
