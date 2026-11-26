---
id: railway
title: Railway application deployment
description: Connect the hosted PANDA interface to its private inference gateway.
last_update:
  date: 2026-11-25
---

# Railway application deployment

Railway hosts PANDA's public-facing application. Model answers come from Groq
or Google Gemini (free tiers) first, with the Irene gateway as the fallback once
UGA approves a secure route. Tables, charts and maps keep working without a model.

## Application variables

| Variable | Purpose |
|---|---|
| `GROQ_API_KEY` | Enables Groq. Seal it in Railway. |
| `GEMINI_API_KEY` | Enables Gemini. Seal it in Railway. |
| `PANDA_LLM_PROVIDERS` | Optional order, e.g. `groq,gemini,local` or `local` only |
| `PANDA_INFERENCE_URL` | HTTPS address of the Irene FastAPI gateway (when approved) |
| `PANDA_INFERENCE_TOKEN` | Shared bearer token accepted by the gateway |
| `PANDA_OLLAMA_MODEL` | Local model name, currently `qwen2.5:3b` |
| `PANDA_STORAGE_DIR` | `/app/storage` (set in the Dockerfile) |
| `PANDA_GROQ_DAILY_REQUESTS`, `PANDA_GEMINI_DAILY_REQUESTS` | Optional daily limits shown in the usage indicator |

Do not place secret values in screenshots, documentation, source code, or commit history.

## Persistent storage (required)

Attach a **volume** to the PANDA service with mount path `/app/storage`. It
holds saved chats, flags, uploaded datasets and usage counts. Without it,
everything resets on each redeploy; the Dataset library shows a warning when no
volume is detected.

## Connection flow

```text
Railway PANDA app
  -> Groq API (HTTPS, API key)          first choice
  -> Google Gemini API (HTTPS, API key) if Groq fails or is rate limited
  -> Irene FastAPI gateway (HTTPS + bearer token) -> private Ollama -> qwen2.5:3b
```

## Deployment check

1. Confirm the Railway service is healthy and the volume is attached at `/app/storage`.
2. Confirm the provider variables exist without displaying their values.
3. Open **Model connection** and run **Test model connection**; it names the provider that answered.
4. Ask a general question, then a data question, and check both answers are model-written.
5. Hover the usage circle to confirm requests are being counted.

## Failure behavior

If every provider fails, PANDA labels the answer "AI answer unavailable right now" and continues offering verified dataset summaries, SQL queries, charts, and maps.
