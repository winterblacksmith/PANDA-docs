---
id: irene-ollama-fastapi
title: Irene, Ollama, and FastAPI
description: Operate the GPU-backed Qwen service on Irene.
---

# Irene, Ollama, and FastAPI

> **Integration status:** This guide describes a separate gateway architecture. The checked Railway application branch (`railway/pdf-reports-model-connection`) starts Streamlit and uses the Ollama client through `OLLAMA_HOST`; it does not contain this gateway or read the `PANDA_INFERENCE_URL`/token settings. Gateway runtime health and application integration must be verified independently.

Irene provides the GPU compute used by PANDA. The current target model is `qwen2.5:3b`, served by Ollama inside Docker. A separate FastAPI gateway validates requests before forwarding them to Ollama.

## Services

```text
gateway  -> authenticated FastAPI API
ollama   -> private model runtime
model-init -> downloads qwen2.5:3b when it is missing
```

## Security rules

- Keep Ollama on an internal Docker network.
- Expose only the FastAPI gateway.
- Require a long random bearer token for every inference request.
- Store the token in environment variables, never in Git.
- Use HTTPS through an approved reverse proxy or tunnel.
- Rotate the token whenever access may have been exposed.

## Standard operator commands

Run these from `~/PANDA` on Irene:

```bash
./scripts/local_llm.sh up
./scripts/local_llm.sh status
./scripts/local_llm.sh test
./scripts/local_llm.sh logs
```

## Current setup note

The Qwen model download is complete when the model initializer reports `writing manifest` followed by `success`. A repeatedly restarting gateway is a gateway problem, not evidence that the model failed to download.

## File-permission repair

The gateway image runs as a non-root `panda` user. Its Dockerfile must copy the package with that ownership:

```dockerfile
COPY --chown=panda:panda inference_gateway /app/inference_gateway
RUN chmod -R u=rwX,go=rX /app/inference_gateway
USER panda
```
