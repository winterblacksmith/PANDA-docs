---
id: project-handoff
title: Project handoff
description: A checklist for transferring PANDA to its next maintainer.
last_update:
  date: 2026-11-25
---

# Project handoff

## What PANDA currently provides

- Natural-language exploration of forestry tables and spatial datasets
- SQLite-backed filtering, aggregation, and persistent chat history
- FVS stand summaries and acreage-weighted charts
- Shapefile mapping through `MU_ID`
- TreeMap raster matching through `TM_Value`
- Model answers from Groq or Gemini (free tiers), with local or Irene Qwen as fallback
- Answers checked against verified numbers; flag button for bad answers
- Dataset library with persistent uploads
- Usage indicator for free-tier quotas
- An authenticated FastAPI inference boundary on Irene
- Downloadable conversation reports

## Handoff checklist

- [ ] Confirm the authoritative GitHub branch and deployment branch.
- [ ] Record the Railway project owner and service names.
- [ ] Record the Irene administrator and operational contact.
- [ ] Rotate and transfer secrets through an approved password manager.
- [ ] Verify the current model and model storage volume.
- [ ] Confirm the PANDA service has a volume at `/app/storage` and transfer Groq/Gemini keys.
- [ ] Run the automated and manual test checklist.
- [ ] Confirm backups for persistent application data.
- [ ] Review open limitations and the React migration plan.
- [ ] Confirm that no private data or secrets are committed to Git.

## Known architectural limitation

The current UI uses Streamlit, which reruns the Python script after interactions. The planned React migration separates a persistent browser interface from a FastAPI application backend so chat submission, map updates, and file selection can update independently without dimming and rerendering the whole page.

## Suggested next phase

1. Get UGA approval for the Railway-to-Irene route and stabilize the Irene endpoint.
2. Add sign-in (UGA Microsoft accounts) and per-user chats, flags and usage limits.
3. Add structured application monitoring and model latency metrics.
4. Move the chat interface to React while retaining the tested Python data services.
5. Expand evaluation prompts and regression tests as new forestry datasets are added.