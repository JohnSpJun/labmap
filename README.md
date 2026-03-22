# LabMap

Map the research landscape. Search for a scholar or field of study, explore collaboration networks, and generate AI-powered lab summaries — powered by Semantic Scholar and Claude.

## Deploy to Cloudflare Pages (free, ~5 minutes)

### 1. Push to GitHub

Create a new GitHub repository and push this folder:

```bash
git init
git add .
git commit -m "Initial LabMap"
git remote add origin https://github.com/YOUR_USERNAME/labmap.git
git push -u origin main
```

### 2. Connect to Cloudflare Pages

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and sign in (free account)
2. Click **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. Authorize GitHub and select your `labmap` repository
4. Set build settings:
   - **Framework preset**: None
   - **Build command**: *(leave empty)*
   - **Build output directory**: `public`
5. Click **Save and Deploy**

### 3. Add your Anthropic API key

After deploy, go to your Pages project → **Settings** → **Environment variables** → **Add variable**:

| Variable name | Value |
|---|---|
| `ANTHROPIC_API_KEY` | `sk-ant-...` (your key from [console.anthropic.com](https://console.anthropic.com)) |

Make sure to add it under **Production** (and optionally Preview).

Then go to **Deployments** → click the three dots on your latest deploy → **Retry deployment** to pick up the new variable.

### 4. Done

Your LabMap is live at `https://labmap-xxx.pages.dev`. Share the URL with anyone — no install needed.

---

## Local development

To run locally with the Worker function working:

```bash
npm install
npm run dev
```

Then open `http://localhost:8788`.

For the AI summary to work locally, create a `.dev.vars` file:

```
ANTHROPIC_API_KEY=sk-ant-your-key-here
```

This file is gitignored and never committed.

---

## Project structure

```
labmap/
├── public/
│   └── index.html          # The full app (graph, sidebar, UI)
├── functions/
│   └── api/
│       └── summarize.js    # Cloudflare Worker — proxies Claude API securely
├── package.json
├── wrangler.toml
└── .gitignore
```

## How it works

- **Frontend** (`public/index.html`) — D3.js force-directed graph, Semantic Scholar API for scholar/paper data, clean academic UI
- **Worker** (`functions/api/summarize.js`) — receives paper data from the frontend, calls Claude with your API key (never exposed to users), returns a structured lab summary
- **Semantic Scholar** — free public API, no key required, 200M+ papers

## Customization

- Change the year range: edit `YEAR_MIN` in `index.html`
- Adjust graph size (max co-authors, max field papers): edit the `limit=` parameters in the fetch calls
- Change the summary structure: edit the prompt in `generateSummary()` and the rendering HTML below it
