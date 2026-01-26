# External Integrations

**Analysis Date:** 2026-01-25

## APIs & External Services

**Package Registry:**
- npm registry - version check used by `.opencode/hooks/gsd-check-update.js`
  - SDK/Client: Node.js `child_process.execSync` shelling out to `npm view`
  - Auth: None

## Data Storage

**Databases:**
- Not detected

**File Storage:**
- Local filesystem only (cache in `~/.claude/cache/gsd-update-check.json` written by `.opencode/hooks/gsd-check-update.js`)

**Caching:**
- Local JSON cache at `~/.claude/cache/gsd-update-check.json`

## Authentication & Identity

**Auth Provider:**
- Not detected

## Monitoring & Observability

**Error Tracking:**
- Not detected

**Logs:**
- Hook scripts are silent by default (no logging in `.opencode/hooks/gsd-check-update.js` or `.opencode/hooks/gsd-statusline.js`)

## CI/CD & Deployment

**Hosting:**
- Not detected

**CI Pipeline:**
- Not detected

## Environment Configuration

**Required env vars:**
- Not detected

**Secrets location:**
- Not detected

## Webhooks & Callbacks

**Incoming:**
- None

**Outgoing:**
- None

---

*Integration audit: 2026-01-25*
