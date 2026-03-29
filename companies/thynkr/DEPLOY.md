# Deploying Paperclip on Railway (Thynkr)

## Prerequisites

- Railway account with the Thynkr project already set up
- Paperclip GitHub repository (fork or the main repo)

## Step 1: Add PostgreSQL Plugin

In your Railway project (where Thynkr already lives):

1. Click **+ New** → **Database** → **PostgreSQL**
2. Name it `paperclip-db`
3. Note the `DATABASE_URL` from the plugin's **Connect** tab

## Step 2: Add Paperclip Service

1. Click **+ New** → **GitHub Repo**
2. Select the Paperclip repository
3. Railway will detect the Dockerfile automatically

### Build Settings

| Setting | Value |
|---------|-------|
| Builder | Dockerfile |
| Dockerfile Path | `Dockerfile` (root) |
| Watch Patterns | `server/**`, `ui/**`, `packages/**`, `cli/**`, `skills/**` |

### Deploy Settings

| Setting | Value |
|---------|-------|
| Health Check Path | `/health` |
| Health Check Timeout | 600 |
| Restart Policy | ON_FAILURE |
| Max Retries | 5 |
| Port | 3100 |

## Step 3: Environment Variables

Set these in the Railway service settings:

### Required

```
DATABASE_URL=${{paperclip-db.DATABASE_URL}}
PORT=3100
HOST=0.0.0.0
NODE_ENV=production
SERVE_UI=true

# Authentication
BETTER_AUTH_SECRET=<generate: openssl rand -base64 32>
PAPERCLIP_DEPLOYMENT_MODE=authenticated
PAPERCLIP_DEPLOYMENT_EXPOSURE=public
PAPERCLIP_PUBLIC_URL=https://<your-service>.up.railway.app

# Auto-apply migrations on startup
PAPERCLIP_MIGRATION_AUTO_APPLY=true

# Agent API key (for claude_local adapter)
ANTHROPIC_API_KEY=<your Anthropic API key>
```

### Optional but Recommended

```
# Disable embedded DB backup (use Railway's built-in backups instead)
PAPERCLIP_DB_BACKUP_ENABLED=false

# GitHub token for engineering agents
GH_TOKEN=<GitHub personal access token with repo scope>
```

## Step 4: Persistent Volume

1. In the Paperclip service settings, go to **Volumes**
2. Add a volume:
   - **Mount Path**: `/paperclip`
   - **Size**: 10 GB (can increase later)

This stores the secrets master key, agent workspaces, and local file storage.

## Step 5: Deploy

1. Push to the connected branch (or trigger manual deploy)
2. Wait for build + health check to pass (~2-3 minutes first time)
3. Visit `https://<your-service>.up.railway.app`
4. Complete the one-time board setup (create your admin account)

## Step 6: Import the Thynkr Company

Once Paperclip is running:

```bash
# From the paperclip repo root
paperclipai company import --from ./companies/thynkr
```

Or use the Paperclip web UI to create the company and agents manually.

## Step 7: Connect Thynkr Signup Webhook

Update Thynkr's environment to point signup notifications to Paperclip:

```
SIGNUP_WEBHOOK_URL=https://<paperclip-service>.up.railway.app/api/webhooks/signup
```

Or use Railway's internal networking (faster, no egress):

```
SIGNUP_WEBHOOK_URL=http://paperclip.railway.internal:3100/api/webhooks/signup
```

## Architecture on Railway

```
Railway Project: "Thynkr"
┌─────────────────────────────────────────┐
│                                         │
│  thynkr-api ◄──── thynkr-db (Postgres) │
│       │           thynkr-redis (Redis)  │
│       │                                 │
│       │ signup webhook                  │
│       ▼                                 │
│  paperclip ◄──── paperclip-db (Postgres)│
│   :3100           :5432                 │
│                                         │
│  Internal network: *.railway.internal   │
└─────────────────────────────────────────┘

Vercel: thynkr-web (Next.js frontend)
```

## Monthly Cost Estimate

| Service | Estimated Cost |
|---------|---------------|
| Paperclip service (2GB RAM) | ~$10-15/mo |
| Paperclip PostgreSQL | ~$5-7/mo |
| Persistent volume (10GB) | ~$2/mo |
| **Total Railway addition** | **~$17-24/mo** |
| Anthropic API (agent usage) | ~$50-200/mo (depends on agent activity) |

## Monitoring

- **Health**: `GET https://<service>.up.railway.app/health`
- **Logs**: Railway dashboard → Paperclip service → Logs
- **Dashboard**: `https://<service>.up.railway.app` (Paperclip UI)
- **Agent budgets**: Visible in Paperclip UI per-agent, auto-pause at limit

## Troubleshooting

### First deploy hangs
Database migrations run on first start. Set `PAPERCLIP_MIGRATION_AUTO_APPLY=true` and allow 60s for health check.

### "BETTER_AUTH_SECRET must be set"
Generate one: `openssl rand -base64 32` and add to env vars.

### Agents not running
- Check `ANTHROPIC_API_KEY` is set
- Check agent budgets haven't been exhausted
- Check the Paperclip logs for adapter errors

### Volume permission issues
The Dockerfile runs as `node` user. Ensure the volume mount is writable by UID 1000.
