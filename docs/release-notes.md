---
id: release-notes
title: Release notes
description: Dated record of PANDA changes, with what each change means for users and maintainers.
last_update:
  date: 2026-11-25
---

# Release notes

The running version is shown in the bottom-right corner of PANDA. Newest first.
Dates are when the change was pushed to the application repository.

## v1.1 — November 2026

### Nov 20, 2026: answers, data and interface

**Model-written answers.** Data answers (counts, summaries, species lists, map
requests, FVS results) are now written by the model from PANDA's verified
results and a profile of the selected dataset. Every number in the answer is
checked against those results, and the model gets one chance to fix a mismatch.
Fixed template text appears only when AI answers are off or no model responds,
and it says so.

**The model knows the selected dataset.** It receives the column names, types,
ranges and most common values (no raw rows), so it no longer asks whether
columns such as `Acres`, `TCuFt` or `Age` exist. Requests like "analyze that
data" or "age class breakdown" now go to PANDA's data tools.

**Dataset library.** A **Dataset library** dialog replaces the import menu and
dataset dropdown. Uploads (CSV, FVS CSV with its shapefile, rasters with their
companion files) are stored on persistent storage instead of inside the app
image. See [Dataset library and storage](./data/dataset-library.md).

**Flag answers.** Each answer has a flag button for wrong or strange output.
Flags are saved with the question, answer and model, and can be downloaded as
CSV from **Model connection**. The three-dots menu holds developer details.

**PDF reports.** Tables, bullet lists and headings now render properly, stray
`[U+202F]` codes are gone, and the report names the model that wrote the summary.

**Interface.** Darker Forest theme with stronger text contrast; PANDA title at
the top of the sidebar; white **New chat** button; rounded floating prompt bar
with centered text and a high-contrast send button; user and assistant icons;
usage circle and report download button in the top-right corner; version label
bottom-right; themed dialogs and menus; sidebar collapse arrow always visible;
the chosen theme is remembered after a refresh; bamboo browser tab icon.

### Nov 20, 2026: usage and limits

A usage circle (top right) shows how much of today's free-tier quota is used.
Hover over it for requests and tokens used, the limit, and what is left per
provider. Groq's request numbers come live from Groq; Gemini's are counted by
PANDA. Quotas reset at midnight Pacific (3 AM Eastern).

### Nov 19–20, 2026: hosted models first, local model as fallback

PANDA now tries **Groq**, then **Google Gemini**, then the **local Qwen2.5:3b**
model (Irene or a developer's Mac). This keeps the deployed app working while the
secure Railway-to-Irene connection waits for UGA approval. Fixes after the first
live test: Groq's reasoning model returned empty answers when the output limit
was too small, and Gemini's original default model returned HTTP 404 for the
project key. See [Model providers, fallback and usage](./architecture/model-providers.md).

## Known limitations (as of Nov 25, 2026)

- **Chats are shared between visitors.** Chat history is stored per dataset, not
  per person, so everyone sees the same chats. Per-user chats need sign-in
  (planned: Microsoft sign-in with UGA accounts). Do not enter private information.
- **Persistent storage must be attached on Railway.** Without a volume at
  `/app/storage`, uploads, chats, flags and usage counts reset on every redeploy.
  The library dialog warns when this is the case.
- **Free-tier quotas are shared by all users** (Groq about 1,000 requests per
  day). Heavy use will need a paid tier or the Irene model as the main engine.
- **Irene is not yet reachable from Railway.** The gateway route needs UGA approval.
