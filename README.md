# NEURIX V3.5

Production AI reliability and runtime protection for **OpenAI** and **Gemini** applications.

NEURIX is runtime infrastructure — not a chatbot builder. It validates structured outputs, runs an AI firewall, monitors reliability drift, stress-tests adversarial cases, and surfaces operational dashboards.

## Stack

| Layer | Technology |
|-------|------------|
| Frontend | React, TailwindCSS, Framer Motion, Recharts |
| Backend | FastAPI |
| Database | PostgreSQL |
| Hosting | Netlify (frontend), backend on Render/Railway/Fly recommended |

## Project structure

```
├── frontend/          # Landing page + operational dashboard
├── backend/           # FastAPI API
├── docker-compose.yml # Local PostgreSQL
├── netlify.toml       # Netlify deployment
└── .env.example       # Environment template
```

## Local development

### 1. Database

```bash
docker compose up -d
```

### 2. Backend

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate        # Windows
pip install -r requirements.txt
copy ..\.env.example .env     # edit DATABASE_URL if needed
uvicorn app.main:app --reload --app-dir .
```

API: http://localhost:8000  
Docs: http://localhost:8000/docs

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

App: http://localhost:5173

## Deployment

### GitHub

1. Install [Git for Windows](https://git-scm.com/download/win)
2. Create repo `Neurix` on GitHub (`teamgenesisstudio-prog`)
3. From project root:

```bash
git init
git add .
git commit -m "NEURIX V3.5 — platform, landing page, API"
git branch -M main
git remote add origin https://github.com/teamgenesisstudio-prog/Neurix.git
git push -u origin main
```

### Netlify (frontend)

1. [Netlify](https://app.netlify.com) → **Add new site** → **Import from GitHub**
2. Select `teamgenesisstudio-prog/Neurix`
3. Build settings (auto-read from `netlify.toml`):
   - Base directory: `frontend`
   - Build: `npm ci && npm run build`
   - Publish: `frontend/dist`
4. Environment variables:
   - `VITE_API_URL` = your deployed backend URL (e.g. `https://neurix-api.onrender.com`)
5. Update `netlify.toml` redirect `YOUR-BACKEND-URL` to match, or proxy via Netlify redirects.

### Backend (production)

Deploy `backend/` to Render, Railway, or Fly.io with:

- `DATABASE_URL` (managed PostgreSQL)
- `CORS_ORIGINS` = your Netlify URL
- `OPENAI_API_KEY` / `GEMINI_API_KEY` (optional, for future provider calls)

Start command:

```bash
uvicorn app.main:app --host 0.0.0.0 --port $PORT
```

## API overview

| Endpoint | Description |
|----------|-------------|
| `POST /api/validate` | JSON validation + auto-repair |
| `POST /api/firewall/analyze` | Prompt/output threat analysis |
| `GET /api/monitoring/metrics` | Dashboard reliability metrics |
| `GET /api/monitoring/events` | Runtime event feed |
| `POST /api/stress/run` | Adversarial stress suite |
| `POST /api/monitoring/scrub` | PII scrubbing |

## Version lineage

- **V1** — Validation, repair, basic firewall, health scoring
- **V2** — Stress testing, hallucination signals, PII scrubbing, live monitoring
- **V3** — AI firewall system, drift monitoring, observability, red-team simulation
- **V3.5** — Premium UX, landing redesign, operational dashboards (this release)

## Troubleshooting

### `npm install` — `UNABLE_TO_VERIFY_LEAF_SIGNATURE`

Your network (corporate proxy, antivirus HTTPS scan, or a missing root CA) is intercepting TLS to `registry.npmjs.org`.

**Fix (preferred):** Install your organization’s root certificate, or turn off HTTPS scanning for npm in antivirus, or use a network without TLS interception.

**Temporary workaround (less secure):** only if you understand the risk:

```bash
npm config set strict-ssl false
```

Then run `npm install` again in `frontend/`. Re-enable strict SSL when possible: `npm config set strict-ssl true`.

### `npm run build` — `tsc` is not recognized

Dependencies did not install. Fix `npm install` first, then rebuild.

### Slow installs under OneDrive

If installs hang, copy the project to a path outside OneDrive (e.g. `C:\dev\neurix`) and run `npm install` there.

## License

Proprietary — Team Genesis Studio
