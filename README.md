# af-poc

Application Factory PoC — standalone demo without Coolify.

Shows the full AF user journey: app selection → parameter config → cloud-init generation → server simulation.

## Start

```bash
pip install -r requirements.txt
python app.py
```

Open http://localhost:5050

## What it demonstrates

1. **App Catalogue** — 6 real apps from `af-recipes` (n8n, Ollama, OpenClaw, Portainer, Claude Code, Gemini CLI)
2. **Selection & Config** — max 5 apps, incompatibility handling, per-app parameter input
3. **Resource Calculation** — OS baseline + app requirements in the summary panel
4. **cloud-init Generation** — POST /api/compose returns a ready-to-use cloud-init YAML
5. **Server Simulation** — animated terminal output simulating the bootstrap process

## Related

- [af-recipes](https://github.com/IONOS-Server-Technology/af-recipes) — App recipes (branch: feature/IF-545-recipe-schema-and-first-apps)
- [af-api](https://github.com/IONOS-Server-Technology/af-api) — API spec (branch: feature/IF-546-api-design-spec)
- [af-coolify-poc](https://github.com/tirufege/af-coolify-poc) — Previous PoC (Coolify-based)
