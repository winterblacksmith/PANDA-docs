---
id: system-overview
title: System architecture
description: How PANDA connects its interface, data layer, spatial tools, and language models.
last_update:
  date: 2026-11-25
---

# System architecture

PANDA stands for **PERSEUS AI for Natural-language Data Analysis**. It is a forestry data assistant that separates conversational reasoning from evidence-producing calculations.

```text
User question
     |
     v
Streamlit interface
     |
     +--> SQLite query layer --> tables, totals, and charts
     |
     +--> spatial layer ------> points, stand polygons, and raster overlays
     |
     +--> model router (llm_client.py), first provider that answers:
            1. Groq  (gpt-oss-120b)
            2. Google Gemini  (gemini-3.5-flash-lite)
            3. local: Irene FastAPI gateway -> Ollama + qwen2.5:3b
                      |
                      v
            answer written from verified results (numbers checked)
```

## Main components

| Component | Responsibility |
|---|---|
| `app.py` | Streamlit application, dataset selection, and chat experience |
| `conversation.py` | Conversation prompts, dataset context and request routing |
| `llm_client.py` | Provider chain (Groq, Gemini, local), fallbacks and cooldowns |
| `llm_usage.py` | Daily request and token counts, Groq quota headers |
| `grounded_answers.py` | Model-written answers checked against verified numbers |
| `dataset_library.py` | Built-in and uploaded datasets, persistent storage |
| `response_actions.py` | Flag button and three-dots menu under each answer |
| `model_connection.py` | Connection test, usage indicator, flag export |
| `fvs_integration.py` | FVS filtering, SQL evidence, vector joins, and raster masks |
| `inference_gateway/` | Authenticated FastAPI service in front of Ollama on Irene |
| `PANDA_STORAGE_DIR` (`/app/storage`) | `canopy.sqlite` (tables, chats, flags), `library/`, `llm_usage.sqlite` |

## Trust boundary

The LLM does not replace the database. PANDA first calculates answers with SQL and spatial code, then gives verified results to the model, which writes the answer; numbers in the answer are checked against those results. This keeps numeric answers reproducible while preserving a natural conversational interface.

## Deployment shape

Railway hosts the public application and calls Groq and Gemini over HTTPS with server-side API keys. Irene hosts the private inference service and GPUs as the fallback; Railway should call only the authenticated FastAPI gateway, and Ollama itself should never be exposed directly to the public internet.