---
id: local-setup
title: Local development
description: Run PANDA and its Docusaurus documentation locally.
---

# Local development

Clone the repository or open the authoritative PANDA working copy. Do not assume a particular external-drive path; confirm the folder that contains `app.py`, `requirements.txt`, and `docs-site/`.

## Application

When a project virtual environment already exists:

```bash
source .venv/bin/activate
python -m streamlit run app.py
```

Using `python -m streamlit` avoids relying on a globally installed `streamlit` shell command.

### Model providers

Export one or both free-tier keys (never commit them), or use a local Ollama
model only:

```bash
export GROQ_API_KEY=...          # console.groq.com/keys
export GEMINI_API_KEY=...        # aistudio.google.com/apikey
# or, local only:
export PANDA_LLM_PROVIDERS=local
```

`python scripts/bench_providers.py` compares response times of each configured
provider. Chats, uploads and usage counts are saved under `storage/` (or
`PANDA_STORAGE_DIR`).

## Documentation website

Docusaurus requires Node.js 20 or newer:

```bash
git clone https://github.com/winterblacksmith/PANDA-docs.git
cd PANDA-docs
npm install
npm run start
```

The development site normally opens at `http://localhost:3000/`. Changes to Markdown files are reflected automatically.

Before publishing:

```bash
npm run typecheck
npm run build
npm run serve
```

## Page dates

Every page shows **Last updated** from the `last_update` date in its front
matter. Change that date whenever you edit a page, and add an entry to the
[release notes](../release-notes.md) for user-visible changes.

## Online editing

Every documentation page includes an **Edit this page** link. It opens the Markdown file on the `main` branch of `PANDA-docs` in GitHub, where an authorized maintainer can edit, preview, and commit changes without the PERSEUS drive.
