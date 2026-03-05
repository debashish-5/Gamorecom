# Gemorecom — AI Game Asset Generator

<div style="width:100%;padding:18px;border-radius:12px;background:linear-gradient(90deg,#0f1724,#071025);box-shadow:0 10px 40px rgba(2,6,23,0.6);margin-bottom:18px">
  <h1 style="margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;color:#ffffff;letter-spacing:-0.02em">
    Gemorecom
  </h1>
  <p style="margin:6px 0 0;color:#cfe9ff">AI Game Asset Studio — generate names, quests, items and lore with a local, modular generative pipeline.</p>
  <div style="margin-top:12px;display:flex;flex-wrap:wrap;gap:8px">
    <span style="padding:6px 10px;border-radius:999px;background:linear-gradient(90deg,#7c3aed,#06b6d4);color:#fff;font-weight:700;font-size:13px">Production-ready UI</span>
    <span style="padding:6px 10px;border-radius:999px;background:#101827;border:1px solid rgba(255,255,255,0.04);color:#cfe9ff;font-weight:700;font-size:13px">API + Job queue</span>
    <span style="padding:6px 10px;border-radius:999px;background:#0b1220;border:1px solid rgba(124,58,237,0.06);color:#dbeafe;font-weight:700;font-size:13px">Experiments-ready</span>
    <span style="padding:6px 10px;border-radius:999px;background:#071025;border:1px solid rgba(6,182,212,0.06);color:#dff8fb;font-weight:700;font-size:13px">Local-first</span>
  </div>
</div>

---

## Key technologies

* LangChain — orchestration & prompt chains.
* Ollama — local model runner (pluggable).
* Mistral — recommended model to pull into Ollama.
* Flask — lightweight API server.
* Blender (optional) — preview / validate 3D outputs for future upgrades.

> Note: the project is modular — replace the model backend with any supported engine (Ollama, local Llama, remote API) by implementing the same generator interface.

---

<div style="border-radius:10px;padding:14px;background:linear-gradient(180deg,#071025,#08122a);margin:18px 0;border:1px solid rgba(255,255,255,0.03)">
  <h2 style="margin:0 0 6px;color:#fff;font-size:1.15rem">What this repo is (and what reviewers want)</h2>
  <ul style="margin:0;padding-left:18px;color:#cfe9ff">
    <li>Small, working ML demo that produces **game assets** (text: names, items, quests, lore). 3D generation is optional.</li>
    <li>Shows **parameter experimentation** (temperature, seed, prompt wording) and saves sample outputs for comparison.</li>
    <li>Includes a **clean UI** (index + chat), a **/api/generate** endpoint, and an example experiments runner.</li>
    <li>Designed to be upgraded to 3D workflows later (mesh outputs, GLB, LODs, Blender validation).</li>
  </ul>
</div>

---

## Quick links

* **Live UI**: `http://127.0.0.1:5000` (after starting Flask)
* **Chat UI**: `http://127.0.0.1:5000/aichat`
* **API endpoint**: `POST /api/generate` → `{ prompt, temperature, seed }`
* **Experiments**: `python experiments.py` (saves side-by-side outputs to `outputs/experiments/`)

---

## One-line install (local, CPU)

```bash
git clone <your-repo>
cd gemorecom
python -m venv .venv && source .venv/bin/activate   # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
# Optional: run ollama daemon and pull a model
# ollama pull mistral
python app.py
```

---

### Project layout

```
gemorecom/
├─ app.py                 # single-file Flask + LangChain + Ollama orchestration
├─ templates/
│  ├─ index.html
│  └─ aichat.html
├─ static/
│  ├─ css/style.css
│  ├─ videos/
│  └─ images/
├─ generator.py           # pluggable generator abstraction (Ollama / dummy)
├─ experiments.py         # parameter sweeps and saving outputs
├─ outputs/               # auto-saved generation results
├─ samples/               # curated sample outputs for README
├─ requirements.txt
└─ README.md
```

---

## How the AI pipeline is organized (modern, production-minded)

1. **Prompt templates** live in `generator.py` (clear templates per asset type).
2. **LangChain** builds an `LLMChain` that feeds the template into the chosen LLM wrapper.
3. **App server** (`app.py`) exposes `/api/generate` and serves the UI; it also provides a safe fallback if the local model is unavailable.
4. **experiments.py** performs controlled sweeps over `temperature` and `seed`, writes JSON outputs and a short summary for each run.
5. **Outputs** are committed (a subset) in `samples/` and the rest saved in `outputs/` for review.

---

<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px;margin-top:18px">
  <div style="padding:12px;border-radius:10px;background:linear-gradient(180deg,#071025,#08112a);border:1px solid rgba(255,255,255,0.02)">
    <strong style="display:block;margin-bottom:8px;color:#fff">Features</strong>
    <ul style="margin:0;padding-left:16px;color:#cfe9ff">
      <li>AI chat UI (aichat.html) with temperature & seed controls</li>
      <li>Full-screen hero video + responsive card gallery (index.html)</li>
      <li>LangChain integration for modular prompt + chain design</li>
      <li>Local-first using Ollama (or fallback dummy generator)</li>
    </ul>
  </div>

  <div style="padding:12px;border-radius:10px;background:linear-gradient(180deg,#071025,#08112a);border:1px solid rgba(255,255,255,0.02)">
    <strong style="display:block;margin-bottom:8px;color:#fff">Experiment capabilities</strong>
    <ul style="margin:0;padding-left:16px;color:#cfe9ff">
      <li>Temperature sweep: low → consistent, high → creative</li>
      <li>Seed control for reproducibility</li>
      <li>Side-by-side JSON outputs + short analysis template</li>
    </ul>
  </div>

  <div style="padding:12px;border-radius:10px;background:linear-gradient(180deg,#071025,#08112a);border:1px solid rgba(255,255,255,0.02)">
    <strong style="display:block;margin-bottom:8px;color:#fff">Why reviewers like it</strong>
    <ul style="margin:0;padding-left:16px;color:#cfe9ff">
      <li>Clean separation of concerns (generator / server / UI / experiments)</li>
      <li>Reproducible experiments and clear README for evaluation</li>
      <li>Pluggable design to upgrade to 3D generation later</li>
    </ul>
  </div>
</div>

---

## API — usage examples

**Request**

```http
POST /api/generate
Content-Type: application/json

{
  "prompt": "Generate 5 epic dragon names for an ice kingdom",
  "temperature": 0.6,
  "seed": 42
}
```

**Response**

```json
{
  "output": "Zyphorax\nDrakarion\nValdrakon\nThalrex\nAbyssal Nytherion",
  "model": "mistral",
  "temperature": 0.6
}
```

---

## Prompt engineering tips (high quality outputs)

* **Be explicit**: include format instructions and length limits.
* **Use templates**: ask for exact fields (TITLE / Description / Abilities).
* **Control creativity**: lower `temperature` for game-balance friendly names; increase for lore & epithets.
* **Include examples** in the prompt for desired structure (few-shot style).

---

## Experiments & what to record

For each experiment, save:

* `prompt`, `temperature`, `seed`, `model`
* `output` (raw text)
* `notes` (human observation: coherence, novelty, suitability for game)

Example short observation:

> temperature=0.2 → short, consistent names (game-ready).
> temperature=1.0 → creative epithets, sometimes verbose — good for lore.

Commit representative outputs into `samples/` so reviewers can quickly inspect.

---

## How to swap backends (fast upgrade path)

1. Implement `generator.py` with a `generate(prompt, temperature, seed)` API.
2. Replace the internal call from the Flask endpoint to call your new generator.
3. If using 3D generation later: change `generate` to produce a glTF/OBJ link, run post-processing (decimation, LOD generation), and save metrics (polycount). Blender can be used to validate import.

---

## Submission checklist (what to include in GitHub & email)

* [ ] `app.py`, `templates/`, `static/` (UI + CSS)
* [ ] `generator.py` (Ollama/LangChain wrapper)
* [ ] `experiments.py` + `outputs/experiments/*` (committed subset)
* [ ] `samples/` (2–6 polished outputs)
* [ ] `README.md` (this file)
* [ ] README short summary (3–5 sentences) for email body

**Email template (paste in your email client):**

```
Subject: ML Assignment Submission – <Your Name>

Hello,

Please find my submission for the ML assignment: Gemorecom — an AI Game Asset Generator. The project includes a small web UI, a LangChain + Ollama generation backend, an experiments runner to demonstrate parameter effects (temperature & seed), and example outputs.

GitHub: <repo link>

Best,
<Your Name>
```

---

## Final tips — make it stand out

1. Add 2–3 **high-quality sample outputs** (well-edited lore, balanced item stats) in `samples/`.
2. Include short **video clips** of using the UI (screen recording) — reviewers appreciate demos.
3. Add an `experiments.md` that lists 6–8 observations (concrete and concise).
4. If possible, demonstrate one upgrade path to 3D generation (even if mock): e.g., a script that converts structured text → placeholder OBJ/GLB (shows you considered the game pipeline).

---

If you want, I can now:

* generate a **ready-to-commit `README.md` file** (this exact text saved to a file), and/or
* produce the **experiments.md** pre-filled with example outputs and observations, and/or
* create `generator.py` that uses **LangChain + Ollama** (fully wired) and `experiments.py` that saves outputs in structured JSON.

Which one should I produce next?
