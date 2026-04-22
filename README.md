# af-poc

Application Factory PoC — standalone demo without Coolify.

Shows the full AF user journey: app selection → parameter config → cloud-init generation → server simulation.

## Quick Start (standalone, no af-api needed)

```bash
git clone https://github.com/tirufege/af-poc.git
cd af-poc
pip install -r requirements.txt
python3 app.py
```

Open http://localhost:5050

## With real af-api (recommended)

Run the real AF API alongside the PoC for authentic cloud-init generation with JWE tokens.

```bash
# Clone all three repos into the same directory
git clone https://github.com/tirufege/af-poc.git
git clone https://github.com/IONOS-Server-Technology/af-api.git
git clone https://github.com/IONOS-Server-Technology/af-recipes.git

# Switch af-api and af-recipes to the feature branches
cd af-api   && git checkout feature/IF-547-api-implementation && cd ..
cd af-recipes && git checkout feature/IF-545-recipe-schema-and-first-apps && cd ..

# Terminal 1 — af-api  (af-recipes must be a sibling directory, which it is)
cd af-api
pip install -r requirements.txt
DEV_MODE=true uvicorn app.main:app --port 8000

# Terminal 2 — af-poc
cd af-poc
pip install -r requirements.txt
python3 app.py
```

Open http://localhost:5050

The PoC auto-detects whether af-api is running:
- **af-api reachable** → proxies `/api/catalogue` and `/api/compose` to `http://localhost:8000/api/v1/`
- **af-api not running** → falls back to built-in mock (local YAML recipes)

Override the API URL: `AF_API_URL=http://other-host:8000/api/v1 python3 app.py`

> **Note:** af-api loads recipes from `../af-recipes/recipes` by default. Both repos must be cloned as siblings.

## Sharing with others (public URL)

To give someone outside your network a clickable link, use [Cloudflare Quick Tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/do-more-with-tunnels/trycloudflare/) — no account or firewall changes needed:

```bash
# Install once
curl -sLo ~/bin/cloudflared https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64
chmod +x ~/bin/cloudflared

# Start tunnel (while af-poc is running on port 5050)
cloudflared tunnel --url http://localhost:5050
```

Cloudflare prints a public HTTPS URL (e.g. `https://some-random-name.trycloudflare.com`) that anyone can open directly. The URL is temporary and changes on every restart.

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
