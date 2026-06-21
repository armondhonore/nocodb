# Nexlayer — nocodb

<!-- nexlayer:meta version=1 analyzed=2026-06-21T15:32:52Z repo=https://github.com/armondhonore/nocodb branch=nexlayer -->

> **For AI agents (Claude Code, Cursor, Gemini CLI, Copilot):**
> This file is the **project context** for this Nexlayer deployment — tech stack, env vars, secrets, live URL.
> For full platform detail (nexlayer.yaml schema, Dockerfile rules, CI/CD, task recipes) read **`nexlayer.skills`** in this repo.
>
> **Critical rules (full detail in `nexlayer.skills`):**
> - Inter-pod refs: `${podName:port}` only — never `localhost` or bare hostnames
> - Docker Hub images: prefix with `mirror.gcr.io/library/` — bare tags fail on the cluster
> - Secrets: set in the Nexlayer dashboard — never commit to `nexlayer.yaml` or Dockerfile
>
> **This file:** `agent-managed` sections update automatically. `user-editable` sections (Local Development Setup, Nexlayer Deployment Plan, Build Notes) are yours — preserved across re-analysis.

## Project Summary
<!-- nexlayer:section agent-managed=project_summary -->
NocoDB is an open-source no-code platform that transforms any database into a smart spreadsheet, allowing users to build databases and applications online quickly.
<!-- nexlayer:end -->

## Technology Stack
<!-- nexlayer:section agent-managed=tech_stack -->
| Name | Kind | Version | Detected From |
|------|------|---------|---------------|
| Node.js | language | 24.14.0 | .npmrc |
| Nuxt | framework | latest | packages/nc-gui/package.json |
| Express | framework | ^4.17.1 | packages/nc-lib-gui/package.json |
| pnpm | tool | latest | package.json |
| Lerna | tool | ^8.2.2 | package.json |
| TypeScript | language | 5.8.3 | package.json |
<!-- nexlayer:end -->

## Repository Structure
<!-- nexlayer:section agent-managed=structure_map -->
- packages/nocodb — Backend core logic and API
- packages/nc-gui — Nuxt-based frontend user interface
- packages/nocodb-sdk — Client-side SDK for interacting with NocoDB
- packages/nc-secret-mgr — Secret management utility
- packages/noco-integrations — Integration modules for external services
<!-- nexlayer:end -->

## External Services Required
<!-- nexlayer:section agent-managed=external_deps -->
Services that must be configured separately (not deployed by Nexlayer):

- Stripe API (via @stripe/stripe-js)
- AWS Amplify (via @aws-amplify/core)
<!-- nexlayer:end -->

## Local Development Setup
<!-- nexlayer:section user-editable=local_setup -->
### Prerequisites

- Node.js 24.14.0
- pnpm

### Environment variables

Copy `.env.example` to `.env.local` and fill in:

```
NC_DB=postgresql://localhost:5432/nocodb
NC_AUTH_JWT_SECRET=your_jwt_secret
```

### Steps

1. `pnpm install` — Install root and workspace dependencies
2. `pnpm run bootstrap` — Build SDKs and register integrations
3. `pnpm run start:backend` — Start the NocoDB backend
4. `pnpm run start:frontend` — Start the Nuxt frontend dev server

<!-- nexlayer:end -->

## Nexlayer Setup
<!-- nexlayer:section agent-managed=nexlayer_setup -->
### Pod Environment Variables

| Pod | Variable | Value | Kind |
|-----|----------|-------|------|
| `app` | `NC_DB` | `"sqlite3:/usr/app/data/?database=noco.db"` | plain |
| `data` | `mountPath` | `/usr/app/data` | plain |
| `data` | `size` | `5Gi` | plain |

### nexlayer.yaml

```yaml
application:
  name: nocodb
  pods:
    - name: app
      image: "mirror.gcr.io/nocodb/nocodb:latest"
      path: /
      servicePorts:
        - 8080
      vars:
        NC_DB: "sqlite3:/usr/app/data/?database=noco.db"
      volumes:
        - name: data
          mountPath: /usr/app/data
          size: 5Gi
```

<!-- nexlayer:end -->

## Nexlayer Deployment Plan
<!-- nexlayer:section user-editable=deployment_plan -->
### Pod Topology

| Pod | Image | Port | Role |
|-----|-------|------|------|
| nocodb-app | mirror.gcr.io/library/node:22-alpine | 8080 | web |
| nocodb-gui | mirror.gcr.io/library/node:22-alpine | 3000 | web |
| nocodb-db | mirror.gcr.io/library/postgres:16-alpine | 5432 | database |

### Deployment notes

- The application backend communicates with the database pod via nocodb-db.pod:5432
- The frontend pod communicates with the backend pod via nocodb-app.pod:8080
-  images are mirrored to mirror.gcr.io to comply with Nexlayer platform rules

<!-- nexlayer:end -->

## Build Notes
<!-- nexlayer:section user-editable=build_notes -->
<!-- Add notes for future builds here — preserved across re-analysis -->
<!-- nexlayer:end -->

## Nexlayer Configuration
<!-- nexlayer:section agent-managed=nexlayer_config -->
**Last deployed:** 2026-06-21T15:34:10Z  
**Live URL:** https://relaxed-weasel-nocodb.cloud.nexlayer.ai  
**Runtime:**  · **Port:** auto-detected  
**Deploy branch:** nexlayer  

```yaml
application:
  name: nocodb
  pods:
    - name: app
      image: "mirror.gcr.io/nocodb/nocodb:latest"
      path: /
      servicePorts:
        - 8080
      vars:
        NC_DB: "sqlite3:/usr/app/data/?database=noco.db"
      volumes:
        - name: data
          mountPath: /usr/app/data
          size: 5Gi
```
<!-- nexlayer:end -->

## Build History
<!-- nexlayer:section agent-managed=build_history -->
| Date | Status | Notes |
|------|--------|-------|
| 2026-06-21T15:32:52Z | analyzed | initial repo analysis |
| 2026-06-21T15:34:10Z | success | deployed https://relaxed-weasel-nocodb.cloud.nexlayer.ai |
<!-- nexlayer:end -->
