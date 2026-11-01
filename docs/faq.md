---
id: faq
title: Frequently asked questions
description: Answers about using PANDA, saved chats, model connections, and deployment.
---

# Frequently asked questions

## Where do I open PANDA and its documentation?

Open [PANDA](https://perseus-panda.up.railway.app/) to analyze data and [the documentation](https://panda-docs.up.railway.app/) for guides. The former `panda-docs-production.up.railway.app` address is no longer the configured docs domain.

## What can I ask PANDA?

Choose a dataset in the sidebar, then use its example questions as a starting point. Supported operations depend on the dataset: inventory summaries, constrained SQL filters, FVS acreage and volume charts, stand polygons, and TreeMap overlays. See [FVS and TreeMap](./data/fvs-treemap.md) for field meanings and spatial limitations.

## Is the language model the source of the numbers?

Deterministic SQL and spatial operations calculate supported results. The language model can help interpret or explain them. Check **SQL and data proof** and the displayed filters before relying on a result; generated prose can still be wrong.

## Why does PANDA say model answers are unavailable?

The application can be healthy while its model server is unreachable or a model is missing. Open **Model connection** in the sidebar and run **Test model connection**. The test checks the model list and requests a short generated answer. A model name in a dropdown alone does not prove inference works.

## Is FastAPI running inside the Railway PANDA service?

The checked GitHub application branch, `railway/pdf-reports-model-connection`, starts Streamlit from its Dockerfile and uses the Ollama client with `OLLAMA_HOST`. It does not include a FastAPI gateway or read `PANDA_INFERENCE_URL` and `PANDA_INFERENCE_TOKEN`.

The [Irene/FastAPI guide](./deployment/irene-ollama-fastapi.md) describes a separate gateway architecture. Its runtime health must be checked independently; a successful Railway `/_stcore/health` check proves only that Streamlit responds. Pointing the Ollama client at a gateway also requires compatible Ollama routes and any required authentication handling.

## Does PANDA still work without the model?

Supported deterministic dataset summaries, filters, tables, charts, and maps can remain available. Open-ended AI answers require a working model connection; not every natural-language question has a deterministic fallback.

## Are chats saved?

PANDA stores chat history in SQLite. Persistent Railway storage is required to retain that database across redeployments. See [SQL and saved chats](./architecture/sql-chat.md). Clearing a chat removes its saved content, so export anything you need first.

## Why do some file names still say canopy?

PANDA is the product name. `storage/canopy.sqlite` and `run_canopy.sh` are legacy file names retained by the application. Renaming them only in the documentation would make the instructions inaccurate.

## Why is the docs site a separate Railway service?

The Docusaurus site builds static HTML and assets and serves them with nginx. It deploys from [PANDA-docs](https://github.com/winterblacksmith/PANDA-docs), independently of the Python application, and does not need inference credentials or a database volume.

## How do I correct a documentation page?

Use **Edit this page** to open its Markdown source in `PANDA-docs` on GitHub. Maintainers should run `npm run typecheck` and `npm run build` before publishing. See [local development](./getting-started/local-setup.md) and [troubleshooting](./operations/testing-troubleshooting.md).
