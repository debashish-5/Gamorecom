# Gemorecom — AI Game Asset Generator

<div style="width:100%;padding:18px;border-radius:12px;background:linear-gradient(90deg,#0f1724,#071025);box-shadow:0 10px 40px rgba(2,6,23,0.6);margin-bottom:18px">
  <h1 style="margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;color:#ffffff;letter-spacing:-0.02em">
    Gemorecom
  </h1>
  <p style="margin:6px 0 0;color:#cfe9ff">AI Game Asset Studio — generate names, quests, items and lore with a local, modular generative pipeline.</p>
  <div style="margin-top:12px;display:flex;flex-wrap:wrap;gap:8px">
    <span style="padding:6px 10px;border-radius:999px;background:linear-gradient(90deg,#7c3aed,#06b6d4);color:#fff;font-weight:700;font-size:13px">Polished UI</span>
    <span style="padding:6px 10px;border-radius:999px;background:#101827;border:1px solid rgba(255,255,255,0.04);color:#cfe9ff;font-weight:700;font-size:13px">API + Experiments</span>
    <span style="padding:6px 10px;border-radius:999px;background:#0b1220;border:1px solid rgba(124,58,237,0.06);color:#dbeafe;font-weight:700;font-size:13px">Local-first</span>
    <span style="padding:6px 10px;border-radius:999px;background:#071025;border:1px solid rgba(6,182,212,0.06);color:#dff8fb;font-weight:700;font-size:13px">Pluggable Generator</span>
  </div>
</div>

---

## Overview

Gemorecom is a local-first, modular toolkit for generating game-ready text assets: names, quests, items, and lore. It ships with a polished web UI (landing + chat), a `/api/generate` endpoint, and an experiments runner to measure how parameters like temperature and seed affect outputs.

This repo is designed for quick evaluation by reviewers: runnable locally, reproducible experiments, and a clear upgrade path to 3D generation later.

---

## Key features

* Clean landing page with fullscreen hero video and responsive asset gallery.
* AI chat UI with controls for **temperature** and **seed**.
* `/api/generate` endpoint wired to a pluggable generator (LangChain → Ollama → direct ollama.chat → dummy fallback).
* `experiments.py` — controlled sweeps across temperature and seed, saving JSON outputs.
* Safe offline fallback so the UI works without a local model.

---

## Tech stack

* LangChain — orchestration & prompt chains.
* Ollama — local model runtime (optional).
* Mistral — recommended model to pull into Ollama.
* Flask — server for UI and API.
* Blender (optional) — preview / validate 3D assets for future upgrades.

> The generator backend is intentionally pluggable. If `langchain` or `ollama` are not installed the system falls back to a deterministic generator so you can demo the UI and run experiments offline.

---

## Project structure

```
gemorecom/
├─ app.py                 # Flask app (serves UI + /api/generate)
├─ generator.py           # pluggable generator abstraction (LangChain / Ollama / dummy)
├─ experiments.py         # parameter sweeps and saving outputs
├─ templates/
│  ├─ index.html
│  └─ aichat.html
├─ static/
│  ├─ css/style.css
│  ├─ videos/
│  └─ images/
├─ uploads/               # optional: user uploads / demo screenshots
├─ outputs/               # generated outputs (experiments saved here)
├─ samples/               # curated sample outputs for reviewers
├─ requirements.txt
└─ README.md
```

---

## Quick start (local, CPU-friendly)

1. Clone repo:

```bash
git clone <your-repo>
cd gemorecom
```

2. Create & activate virtual environment:

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. (Optional) Install Ollama and pull a model:

```bash
# install Ollama app from https://ollama.com
ollama pull mistral
```

5. Run the app:

```bash
python app.py
```

Open `http://127.0.0.1:5000` (landing) and click **Meet Gemorecom** to open the chat.

---

## API

### `POST /api/generate`

**Request body (JSON):**

```json
{
  "type": "name",               // name | item | quest | lore | default
  "prompt": "Generate 5 dragon names for an ice kingdom",
  "temperature": 0.6,
  "seed": 42,
  "num": 5,
  "variables": { "theme": "ice", "rarity": "legendary" }
}
```

**Response:**

```json
{
  "output": "Zyphorax\nDrakarion\nValdrakon\nThalrex\nAbyssal Nytherion",
  "meta": { "backend": "ollama_direct", "model": "mistral", "temperature": 0.6 },
  "asset_type": "name",
  "variables": { "theme": "ice", "num": 5 }
}
```

Notes:

* The API maps `type` → internal prompt templates in `generator.py`.
* If `langchain` + `ollama` are available the chain is used; otherwise it tries direct `ollama.chat`, then the dummy fallback.

---

## Experiments

Use `experiments.py` to run controlled sweeps and save outputs to `outputs/experiments/`. Each JSON file contains:

* `meta`: `{ prompt, variables, model, temperature, seed, timestamp }`
* `output`: raw generated text
* `generator_meta`: backend information (langchain/ollama/dummy)

Recommended sweep settings:

* Temperatures: `0.2`, `0.6`, `1.0`
* Seeds: `42`, `123`, `999`
* `num` (number of items per run): `3–5`

Record short human notes for each run in `experiments.md` (coherence, novelty, game-readiness).

---

## Prompt engineering tips

* Be explicit: require exact output fields (TITLE / Description / Stats / Abilities).
* Use templates: structured output is easier to parse and convert to in-game data.
* Control creativity: use low temperature (0.1–0.4) for deterministic names/stats; high temperature (0.8–1.0) for evocative lore.
* Include a short system preface (e.g., "You are an expert game content designer") for consistent tone.
* For reproducibility, include `seed` and commit representative outputs.

---

## Usage examples

**Generate dragon names (curl):**

```bash
curl -X POST http://127.0.0.1:5000/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "type":"name",
    "prompt":"Generate 5 epic dragon names for an ice kingdom",
    "temperature":0.6,
    "seed":42,
    "num":5
  }'
```

**Generate a quest with file context (UI / API):** include `file_content` in the payload (first ~5000 chars of uploaded file) to bias results toward your lore.

---

## UI

* `index.html`: fullscreen hero video, responsive gallery of video/image cards, **Meet Gemorecom** button that opens `/aichat`. Cards autoplay on hover/focus.
* `aichat.html`: conversation panel, temperature & seed controls, send button triggers `/api/generate`, shows live outputs and preserves session history in the browser.

---

## Configuration & env

Environment variables:

```bash
PORT=5000
FLASK_ENV=development
```

Settings in `app.py` to review:

* `MODEL_NAME` — default model to call (e.g., `"mistral"`)
* `UPLOAD_FOLDER` — where uploads are temporarily stored
* `MAX_CONTENT_LENGTH` — max upload size (default in code)

---

## Troubleshooting

**Ollama connection refused**

* Ensure the Ollama app/daemon is running and you pulled a model:

```bash
ollama pull mistral
ollama serve
```

Verify with `curl http://localhost:11434/api/status` (if using Ollama daemon).

**Model not found**

* Run `ollama list` after `ollama pull <model>`; change `MODEL_NAME` if needed.

**Out of memory**

* Close other apps, use a smaller model, or increase swap. For low-RAM demo, rely on the dummy fallback.

**CORS**

* Frontend and backend are served from the same Flask app; enable `flask-cors` only if you separate origins.

---

## Development

**Run in debug mode**

```bash
export FLASK_ENV=development
python app.py
```

(Windows PowerShell: `set FLASK_ENV=development`)

**Run experiments**

```bash
python experiments.py
```

---

## Contributing

Contributions welcome:

1. Fork repository
2. Create a feature branch
3. Open a pull request with tests / example outputs
4. Report issues in the tracker

---

## Future enhancements

* Add structured JSON output parsers for items (title / stats / abilities) to convert directly to game data.
* Add optional 3D backend: generate placeholder GLB/OBJ metadata and a mock LOD pipeline.
* Add authentication, user projects, and saved histories.
* Add streaming (SSE or WebSocket) for incremental token display.

---

## License

This project is open source under the MIT License.

---

## Resources

| Resource       | Link                                                                   |
| -------------- | ---------------------------------------------------------------------- |
| Ollama docs    | [https://ollama.ai](https://ollama.ai)                                 |
| LangChain docs | [https://langchain.readthedocs.io](https://langchain.readthedocs.io)   |
| Flask docs     | [https://flask.palletsprojects.com](https://flask.palletsprojects.com) |
| Mistral        | [https://mistral.ai](https://mistral.ai)                               |

---

If you want, I can now:

* write this README into a `README.md` file for you, ready to commit,
* generate `experiments.md` with example runs and human notes, or
* produce a `samples/` folder with 6 polished sample outputs to include in the repo. Which would you like next?
