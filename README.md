# af-poc

Application Factory PoC — standalone demo without Coolify.

Shows the full AF user journey: app selection → parameter config → cloud-init generation → server simulation.

## Quick Start (standalone, no af-api needed)

```bash
git clone https://github.com/tirufege/af-poc.git
cd af-poc
pip install -r requirements.txt
python app.py
```

Open http://localhost:5050

## With real af-api (recommended)

Run the real AF API alongside the PoC for authentic cloud-init generation with JWE tokens.

```bash
# Terminal 1 — af-api
git clone https://github.com/IONOS-Server-Technology/af-api.git
cd af-api
git checkout feature/IF-547-api-implementation
pip install -r requirements.txt
DEV_MODE=true uvicorn app.main:app --reload --port 8000

# Terminal 2 — af-poc
cd af-poc
pip install -r requirements.txt
python app.py
```

The PoC auto-detects whether af-api is running:
- **af-api reachable** → proxies `/api/catalogue` and `/api/compose` to `http://localhost:8000/api/v1/`
- **af-api not running** → falls back to built-in mock (local YAML recipes)

Override the API URL: `AF_API_URL=http://other-host:8000/api/v1 python app.py`

## What it demonstrates

1. **App Catalogue** — 6 apps from af-recipes (n8n, Ollama, OpenClaw, Portainer, Claude Code, Gemini CLI)
2. **Selection & Config** — max 5 apps, incompatibility handling (ollama ↔ openclaw), per-app parameter input
3. **Resource Calculation** — OS baseline + app requirements in the summary panel
4. **cloud-init Generation** — POST /api/compose returns a ready-to-use cloud-init YAML
5. **Server Simulation** — animated terminal output simulating the bootstrap process

## Related

- [af-api](https://github.com/IONOS-Server-Technology/af-api) — Real AF API (FastAPI, JWE tokens)
- [af-recipes](https://github.com/IONOS-Server-Technology/af-recipes) — App recipes (branch: feature/IF-545-recipe-schema-and-first-apps)
- [af-coolify-poc](https://github.com/tirufege/af-coolify-poc) — Previous PoC (Coolify-based)
