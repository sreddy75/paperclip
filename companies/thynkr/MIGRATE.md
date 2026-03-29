# Migrating from NanoClaw to Paperclip

## Changes Required in the Thynkr Repository

### 1. Update Signup Webhook Comment (apps/web/src/lib/auth.ts:88)

The webhook itself doesn't need code changes — just update the `SIGNUP_WEBHOOK_URL` environment variable in Railway to point to Paperclip instead of NanoClaw.

**Current comment** (line 88):
```typescript
// Fire signup webhook to NanoClaw → WhatsApp "Thynkr Signups" group
```

**Update to**:
```typescript
// Fire signup webhook to Paperclip → triggers growth lead agent
```

**Railway env var change**:
```
# Old (NanoClaw)
SIGNUP_WEBHOOK_URL=<nanoclaw-endpoint>

# New (Paperclip — use internal networking)
SIGNUP_WEBHOOK_URL=http://paperclip.railway.internal:3100/api/webhooks/signup
```

### 2. Update or Remove GitHub Workflow (.github/workflows/sync-nanoclaw.yml)

The NanoClaw doc sync workflow should be removed or replaced.

**Option A: Remove entirely**
Paperclip agents access the Thynkr repo directly via git clone (using GH_TOKEN). No webhook needed — agents read current state each heartbeat.

```bash
rm .github/workflows/sync-nanoclaw.yml
```

**Option B: Replace with Paperclip notification**
If you want Paperclip to be notified when Thynkr docs change:

```yaml
name: Notify Paperclip of doc changes

on:
  push:
    branches: [main]
    paths:
      - '.project/**'
      - 'CLAUDE.md'
      - 'TODOS.md'

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Paperclip webhook
        run: |
          curl -X POST \
            -H "Content-Type: application/json" \
            -H "Authorization: Bearer ${{ secrets.PAPERCLIP_API_KEY }}" \
            https://<paperclip-service>.up.railway.app/api/webhooks/docs-updated \
            -d '{"source":"thynkr","event":"docs_updated"}'
```

### 3. Decommission NanoClaw

Once Paperclip is running and the Thynkr company is imported:

1. Verify all NanoClaw agent functions are covered by Paperclip agents
2. Stop NanoClaw containers on DigitalOcean
3. Export any historical data you want to keep (content briefs, research archives)
4. Remove the NanoClaw GitHub repository reference from Thynkr secrets
5. Delete the DigitalOcean droplet (or repurpose it)

### What NanoClaw Did → What Paperclip Replaces It With

| NanoClaw Agent | Paperclip Agent | Notes |
|---------------|----------------|-------|
| content-engine | Content Writer | Same function, better governance |
| seo-writer | Content Writer | Consolidated — one agent handles all content |
| community-scout | Growth Lead | Monitoring is part of growth lead's daily routine |
| competitor-watch | Growth Lead | Weekly competitor check is a growth lead task |
| analytics-reporter | Growth Lead | Weekly metrics is a growth lead task |
| lead-nurture | Growth Lead (task) | Event-driven, assigned as needed |
| event-amplifier | Growth Lead (task) | Event-driven, assigned as needed |

### Blog Pipeline Migration

**Old (NanoClaw + R2 bucket):**
```
community-scout → R2/research/ → seo-writer → R2/briefs/pending/ → Thynkr pipeline
```

**New (Paperclip):**
```
Growth Lead (community research task) → Content Writer (blog pipeline task) → posts draft as issue comment → Suren reviews → merges to Thynkr repo
```

No more R2 bucket. No more cross-service file handoff. Content lives in Paperclip issues as comments until approved, then gets committed to the Thynkr blog directory.
