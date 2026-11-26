---
id: faq
title: Frequently asked questions
description: Answers about using PANDA, saved chats, model connections, and deployment.
last_update:
  date: 2026-11-25
---

# Frequently asked questions

## Where do I open PANDA and its documentation?

Open [PANDA](https://perseus-panda.up.railway.app/) to analyze data and [the documentation](https://panda-docs.up.railway.app/) for guides. The former `panda-docs-production.up.railway.app` address is no longer the configured docs domain.

## What can I ask PANDA?

Open **Dataset library** in the sidebar, press **Analyze** on a dataset, then use its example questions as a starting point. Supported operations depend on the dataset: inventory summaries, constrained SQL filters, FVS acreage and volume charts, stand polygons, and TreeMap overlays. See [FVS and TreeMap](./data/fvs-treemap.md) for field meanings and spatial limitations.

## Is the language model the source of the numbers?

No. SQL and spatial code calculate the results, and the model writes the answer from them. PANDA checks every number in the answer against the calculated results and asks the model to correct any mismatch. Generated wording can still be misleading, so check the displayed filters and the developer details (three-dots menu) before relying on a result, and use the flag button when something looks off.

## Which AI model answers my question?

PANDA tries Groq first, then Google Gemini, then the local Qwen model, and uses
the first that answers. **Model connection** in the sidebar shows the order;
**Test model connection** sends a real request and names the provider that
answered and any that failed first.

## Why does PANDA say "AI answer unavailable right now"?

No model provider answered (missing API keys, free-tier quota used up, or the
local model unreachable). PANDA then shows its verified result without AI
wording. Run **Test model connection** to see which provider failed and why.

## What does the circle in the top-right corner show?

How much of today's free-tier model quota is used. Hover over it for requests
and tokens used, the daily limit, and what is left per provider. Quotas reset at
midnight Pacific (3 AM Eastern).

## How do I report a wrong or strange answer?

Press the flag icon under the answer, choose what looks off and add a note.
Flags are saved for reviewers, who can download them from **Model connection**.

## Is my data sent to an outside service?

When Groq or Gemini answers, the question, a dataset profile (column names,
ranges, common values) and query results are sent to that provider; raw dataset
files are not. Gemini's free tier allows Google to use prompts to improve its
products. See [Language models](./overview.md#language-models).

## Is FastAPI running inside the Railway PANDA service?

No. The FastAPI gateway runs on Irene in front of Ollama. The Railway app reads
`PANDA_INFERENCE_URL` and `PANDA_INFERENCE_TOKEN` and calls it as the local
fallback once UGA approves a secure route from Railway to Irene. A healthy
Railway `/_stcore/health` check proves only that Streamlit responds. See the
[Irene/FastAPI guide](./deployment/irene-ollama-fastapi.md).

## Does PANDA still work without the model?

Yes, with plain wording. Tables, charts, maps and verified summaries still work and are labelled as not AI-written. Open-ended conversation needs a working model.

## Are chats saved?

Yes, in SQLite under `/app/storage` on Railway. A Railway volume must be attached there, or chats are lost on every redeploy. Chats are currently shared by everyone using the app (there is no sign-in yet), so do not enter private information. Clearing a chat removes it; download the PDF report first if you need it. See [SQL and saved chats](./architecture/sql-chat.md).

## How do I add my own dataset?

Open **Dataset library**, upload the files under **Add datasets**, and press
**Add to library**. Upload an FVS CSV together with its `.shp`, `.shx` and `.dbf`
to enable stand maps. See [Dataset library and storage](./data/dataset-library.md).

## Why did my theme reset?

It should not anymore: the theme is saved in the page address (for example
`?theme=Light`). Bookmark that address to keep your choice.

## Why do some file names still say canopy?

PANDA is the product name. `storage/canopy.sqlite` and `run_canopy.sh` are legacy file names retained by the application. Renaming them only in the documentation would make the instructions inaccurate.

## Why is the docs site a separate Railway service?

The Docusaurus site builds static HTML and assets and serves them with nginx. It deploys from [PANDA-docs](https://github.com/winterblacksmith/PANDA-docs), independently of the Python application, and does not need inference credentials or a database volume.

## How do I correct a documentation page?

Use **Edit this page** to open its Markdown source in `PANDA-docs` on GitHub. Maintainers should run `npm run typecheck` and `npm run build` before publishing. See [local development](./getting-started/local-setup.md) and [troubleshooting](./operations/testing-troubleshooting.md).
