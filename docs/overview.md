---
id: overview
title: PANDA overview
slug: /overview
description: Purpose, scope, repository, model, and data flow for PANDA.
sidebar_position: 1
last_update:
  date: 2026-11-25
---

# PANDA overview

**PANDA** stands for **PERSEUS AI for Natural-language Data Analysis**. It is a forestry-focused interface for asking natural-language questions about inventory tables, forest simulation results, stand polygons, and raster datasets.

## Project purpose

Forestry data is often distributed across CSV files, databases, shapefiles, and rasters. PANDA brings those sources into one conversational interface while keeping the calculations inspectable.

PANDA is designed to:

- translate natural-language questions into constrained data operations;
- use SQLite for reproducible filtering and aggregation;
- visualize point, polygon, and raster results;
- preserve chat history across application restarts;
- have a language model write each answer from verified results, with numbers checked; and
- expose enough evidence to show where every result came from.

PANDA is not FVS and does not train a new forestry model. It reads, filters, visualizes, and explains outputs produced by FVS, TreeMap, tree inventories, and related workflows.

## Repository

The application is maintained in the [PANDA GitHub repository](https://github.com/winterblacksmith/PANDA). This Docusaurus website is maintained independently in [PANDA-docs](https://github.com/winterblacksmith/PANDA-docs).

## Language models

PANDA tries model providers in order and uses the first that answers:

1. **Groq** (`openai/gpt-oss-120b`, backup `openai/gpt-oss-20b`), free tier;
2. **Google Gemini** (`gemini-3.5-flash-lite` and backups), free tier;
3. **Local `qwen2.5:3b`** through Ollama, on Irene's GPUs behind the authenticated
   FastAPI gateway once Railway can reach it, or on a developer's Mac.

The model writes the answer from verified results; SQLite and the spatial files
remain the source of every number. See [Model providers, fallback and usage](./architecture/model-providers.md).

:::warning What leaves PANDA
With Groq or Gemini enabled, the question, a profile of the selected dataset
(column names, ranges, common values) and the query results are sent to that
provider. Raw dataset files are not sent. Gemini's free tier allows Google to use
prompts to improve its products. For data that must stay on UGA systems, set
`PANDA_LLM_PROVIDERS=local`.
:::

## Data flow

```text
Question
  -> constrained intent and filters
  -> parameterized SQLite query
  -> optional shapefile or TreeMap join
  -> verified table, total, chart, or map
  -> answer written by Groq, Gemini or local Qwen (numbers checked)
  -> persistent saved conversation
```

Continue with the [system architecture](./architecture/system-overview), the [implementation walkthrough](./implementation-walkthrough) or the [release notes](./release-notes).
