# Omnigent Guide

Public guide and reviewed deployment bundle for running Omnigent locally or on Databricks Apps.

## What is included

- `deploy/databricks/` - Databricks Asset Bundle deployment code for the Omnigent server, reviewed from Omnigent `0.3.0` source commit `15c6460`.
- `docs/omnigent-three-ways-setup.html` - visual guide covering local, Databricks Apps, and managed workspace setup paths.
- `examples/agents/` - minimal agent YAML examples with placeholder/local profile values only.

## Sensitive-data review

Before publishing, the bundle and examples were checked for tokens, PATs, API keys, real app URLs, workspace hostnames, emails, host IDs, runner tokens, and local auth paths. The raw local wiki note was not uploaded because it contained real deployment metadata.

The GitHub wiki for this repo is enabled but currently has no pages, so there was no remote wiki content to sanitize.

## Databricks Apps Free Edition notes

This bundle is intended to live inside an Omnigent source checkout because `deploy.py` builds Omnigent wheels from the repository root.

Free Edition defaults used here:

- Catalog examples use `workspace`, not `main`.
- App compute defaults to `MEDIUM`.
- Databricks Apps telemetry export is disabled in `databricks.yml` because default-storage workspaces reject that export destination.
- Run deployment through the Databricks extra: `uv run --extra databricks python deploy/databricks/deploy.py ...`.

Typical deploy command:

```bash
uv run --extra databricks python deploy/databricks/deploy.py \
  --app-name omnigent \
  --profile <your-profile> \
  --lakebase-branch projects/omnigent/branches/production \
  --lakebase-database projects/omnigent/branches/production/databases/databricks-postgres \
  --volume-name workspace.omnigent.artifacts
```

After the app is deployed, attach a local host:

```bash
omnigent login https://omnigent-<workspace-id>.aws.databricksapps.com
omni host --server https://omnigent-<workspace-id>.aws.databricksapps.com
```
