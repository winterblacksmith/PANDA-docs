# PANDA documentation

This Docusaurus site documents PANDA: PERSEUS AI for Natural-language Data Analysis.

## Edit online

Open `docs/` on the `main` branch of `winterblacksmith/PANDA-docs`. Documentation pages are ordinary Markdown files and can be edited directly through GitHub.

## Run locally

```bash
git clone https://github.com/winterblacksmith/PANDA-docs.git
cd PANDA-docs
npm install
npm run start
```

## Verify a production build

```bash
npm run typecheck
npm run build
npm run serve
```

## Deploy on Railway

Create a separate Railway service connected to this repository's `main` branch. The included Dockerfile builds Docusaurus and serves the static output with nginx. The included `railway.toml` sets the healthcheck.

```text
Root Directory: /
Builder: Dockerfile
Healthcheck Path: /
```

Under Networking, generate a public domain. The preferred name, if Railway reports it available, is:

```text
https://perseus-panda-docs.up.railway.app
```

The application does not require database credentials or inference secrets. The documentation is static and should be deployed as a separate service from the PANDA application.

## Source

Extracted from `winterblacksmith/PANDA`, branch `docs/panda-handoff-starter`, commit `0672bfc`. Application code and its complete history remain in the PANDA repository. This repository independently deploys only the documentation.
