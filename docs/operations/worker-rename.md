# Cloudflare Worker identity after GitHub repository rename

Date: 2026-10-31. Scope: `emotigom/gomdori-lesson-factory`.

## Facts verified in GitHub

The GitHub repository ID remains `1334987272` and the new slug is `gomdori-lesson-factory`. The repository still contains `wrangler.jsonc` with Worker name `nextbridge-lesson-factory`, and `worker/src/index.js` returns the same legacy identifier at `/api/health`. These fields are not GitHub repo names; changing them would target a *different* Cloudflare Worker identity.

The existing `.github/workflows/deploy-worker.yml` explicitly blocks production and its preview branch only echoes a Wrangler dry-run command. No Cloudflare Worker migration was executed by the GitHub rename.

## Migration gate (manual authorization required)

1. Confirm which Cloudflare account/zone owns the old Worker, current route/custom domain, DNS, R2 assets/bindings and any external consumers. Collect only non-secret configuration in the tracking issue.
2. Determine whether to preserve the original Worker URL or register a new `gomdori-lesson-factory` identity. If changing, configure both endpoints in parallel with read-only health checks and verify exact response behavior.
3. Validate release index, `/api/releases/:courseId`, static asset delivery, R2 storage and authentication boundary. Use preview first; don't open production gates or publish private package files just for rebranding.
4. Update DNS/routing, service identifiers and `wrangler.jsonc` as one reviewed cutover only after live verification. Keep an explicit rollback route and monitor before decommissioning any legacy hostname.
5. Preserve historical commit SHAs, previous release evidence, original Worker/URL references as dated records; do not rewrite approved course evidence or project provenance.

**Current status:** repository name verified; Cloudflare Worker name intentionally unchanged. External Worker/route, old URLs and R2 data have not been independently verified.
