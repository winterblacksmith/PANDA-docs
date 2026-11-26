---
id: testing-troubleshooting
title: Testing and troubleshooting
description: Reproducible checks for PANDA's data, model, documentation, and deployment layers.
last_update:
  date: 2026-11-25
---

# Testing and troubleshooting

Test each layer independently before testing the entire application.

## Recommended order

1. Verify Python imports and unit tests.
2. Verify SQLite synchronization and saved chats.
3. Verify the FVS shapefile and TreeMap raster joins.
4. Run **Test model connection** in PANDA; it names the provider that answered.
5. Verify Ollama lists `qwen2.5:3b` and the FastAPI gateway generates on Irene.
6. Verify Railway can reach the gateway (after the UGA route is approved).
7. Build the documentation site.

## Documentation checks

```bash
cd PANDA-docs
npm install
npm run typecheck
npm run build
npm run start
```

## Common failures

| Symptom | Likely cause | Response |
|---|---|---|
| `command not found: streamlit` | Virtual environment is missing or inactive | Use the project's launcher or recreate the environment |
| CSV UTF-8 decoding error | A macOS `._` metadata sidecar was selected | Ignore files whose names begin with `._` |
| Ollama healthy, gateway restarting | Gateway application import or permission error | Inspect gateway logs and rebuild only the gateway |
| Model unavailable on Railway | Missing URL/token, blocked network path, or gateway down | Test each connection boundary separately |
| Raster metadata unavailable | Native GDAL/raster dependency missing | Install the system library required by Rasterio |
| "AI answer unavailable right now" | No provider answered | Run **Test model connection**; check keys, quota, Irene |
| Groq "empty answer" | Output limit too small for the reasoning model | Fixed Nov 20; update to the latest app build |
| Gemini HTTP 404 | Model retired or not available to the key | PANDA tries backup models; set `PANDA_GEMINI_MODEL` |
| Usage circle back at 0% | No Railway volume, so counts reset on redeploy | Attach a volume at `/app/storage` |
| Uploads or chats gone after redeploy | No Railway volume | Attach a volume at `/app/storage` |
| Usage circle shows "!" | Usage database could not be read | Refresh; check the storage volume |

## Evidence to retain

For handoff, keep the test date, branch or commit, commands used, pass/fail result, and relevant non-secret log excerpt. Never copy access tokens into the test record.
