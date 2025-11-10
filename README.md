# Cotopia — README / Launch, Deploy & Setup Guide

Version: 1.1  
Last updated: November 2025  
Parent company: Cotopia (Cotopia)  
Contact / Support: Support@cotopia.org

Table of contents
- Overview
- Quick start (developer)
- System architecture & components
- Prerequisites
- Environment & configuration
- Local development setup
- Build & distribution
- Deployment (dev / staging / prod)
- Infrastructure & hosting recommendations
- Storage, file chunking & manifest workflow
- Database & migrations
- Secrets management & environment variables
- TLS, CORS, CSP and secure headers
- CI / CD (example pipelines)
- Monitoring, logging & alerting
- Backups & disaster recovery
- Security & vulnerability management
- Operational runbooks (onboarding, support, escalations)
- Pricing / Contract / Legal pointers (partner-facing)
- Troubleshooting & FAQs
- Checklist: pre-launch -> launch -> post-launch
- Useful commands & sample env template
- Appendix: links to terms, privacy, partner guide

--------------------------------------------------------------------------------
Overview
--------------------------------------------------------------------------------
Cotopia (parent company: Cotopia) provides the Cotopia QR & SKU Maker / Scanner application and related partner services. The product is a web-based single-file app with optional server integration for:

- Generating QR codes and SKU labels
- Embedding small payloads in QR payloads (≤ ~6KB) or producing manifests + ZIPs for larger files
- Client-side file chunking and reassembly with manifest (SHA‑256 verification)
- CSV SKU import, templates, export PNG/SVG labels
- Scanner: camera + file scan, manifest verification, chunk reassembly

This README describes how to build, deploy, host, secure, operate and support the app in production, and includes partner/reseller, legal, and pricing pointers.

--------------------------------------------------------------------------------
Quick start (developer)
--------------------------------------------------------------------------------
1. Clone repo:
   git clone git@github.org:yourorg/cotopia.git
2. Install:
   cd cotopia
   yarn install        # or npm ci
3. Create `.env.local` from `.env.example` (see sample at bottom)
4. Run dev server:
   yarn dev            # typically starts at https://localhost:3000
5. Build:
   yarn build
6. Start production preview:
   yarn start

(Replace `yarn` with `npm` if needed.)

--------------------------------------------------------------------------------
System architecture & components
--------------------------------------------------------------------------------
- Frontend: single-file web app (vanilla JS / Svelte / React / Preact / plain HTML) — distributable as one file for easy embedding.
- Backend (optional): Node/Express or serverless endpoints for authorization, partner portal, hostable manifests/ZIPs, payments/webhooks, admin.
- Storage: S3 or S3-compatible storage + CDN for ZIPs/assets.
- DB: Postgres (managed recommended) for accounts, billing, manifest metadata.
- Queue: Redis / SQS for background jobs (packaging, antivirus scanning).
- Monitoring: Sentry for errors; Prometheus/Grafana or Datadog for metrics.
- CI/CD: GitHub Actions, GitLab CI, CircleCI, or your platform of choice.
- Auth: JWT / OAuth / API keys for partner integrations.

--------------------------------------------------------------------------------
Prerequisites
--------------------------------------------------------------------------------
- Node.js >= 16 (or project-specified runtime)
- Yarn or npm
- Docker (recommended)
- PostgreSQL >= 12 (or managed service)
- S3-compatible storage + CDN (AWS S3 + CloudFront recommended)
- Domain and TLS (e.g., app.cotopia.org)
- Payment provider (Stripe, PayPal) if billing is used

--------------------------------------------------------------------------------
Environment & configuration
--------------------------------------------------------------------------------
Use environment files per stage; never commit secrets. Key env groups:

- APP: APP_HOST, APP_PORT, APP_ORIGIN, NODE_ENV
- DB: DATABASE_URL, DB_SSL
- STORAGE: S3_BUCKET, S3_REGION, S3_ACCESS_KEY_ID, S3_SECRET_ACCESS_KEY
- AUTH: JWT_SECRET, SESSION_SECRET, OAUTH_CLIENT_ID/SECRET
- PAYMENT: STRIPE_SECRET_KEY, STRIPE_WEBHOOK_SECRET
- MONITORING: SENTRY_DSN
- OPS: EMAIL_FROM, SMTP creds
- FEATURE FLAGS: HOST_UPLOADS (cotopia|client|distributor), MAX_INLINE_QR_BYTES

Provide a `.env.example` and `ENV_VARS.md` documenting each variable.

--------------------------------------------------------------------------------
Local development setup
--------------------------------------------------------------------------------
1. Install dependencies:
   yarn install
2. Start local infra (recommended via docker-compose):
   docker-compose up -d postgres redis
3. Copy `.env.example` -> `.env.local` and populate
4. Run DB migrations:
   yarn migrate up
5. Start dev server:
   yarn dev
6. Use mkcert or ngrok for HTTPS to test camera access (`getUserMedia` requires secure context).

--------------------------------------------------------------------------------
Build & distribution
--------------------------------------------------------------------------------
- Frontend: `yarn build` → `dist/` or a single `cotopia-app.html`
- Server: `yarn build:server` or Docker image
- Distribution modes:
  - Cotopia-hosted: upload artifacts to S3 + CDN and serve from app.cotopia.org
  - Partner-hosted: provide single-file and integration docs (iframe/embed + postMessage)
- Versioning: semver; embed build SHA in `window.__COTOPIA_VERSION__`

--------------------------------------------------------------------------------
Deployment (dev / staging / prod)
--------------------------------------------------------------------------------
Environments:
- dev: ephemeral branches and local dev
- staging: mirrors prod, tests webhooks & integrations
- prod: HA, monitoring, backups

Example deployment flow (CI):
1. CI builds artifacts and Docker image, tags `sha-<commit>`
2. Run migrations (staging first)
3. Deploy to staging, run smoke tests
4. Deploy to prod using blue/green or canary
5. Run post-deploy smoke and health checks; monitor metrics

Rollback: Keep previous image/tag and use health-check-driven rollback.

--------------------------------------------------------------------------------
Infrastructure & hosting recommendations
--------------------------------------------------------------------------------
- Static hosting: S3 + CloudFront with immutable content-hash filenames
- App servers: managed platform (ECS, k8s, Fly, Render, Vercel)
- DB: managed Postgres with Multi-AZ and automated backups
- Storage: S3 with SSE-KMS, versioning, lifecycle rules
- CDN: CloudFront, Fastly, or equivalent
- Webhooks: validate signatures and restrict IPs where possible

--------------------------------------------------------------------------------
Storage, file chunking & manifest workflow
--------------------------------------------------------------------------------
Flows:
- Inline small payloads: embed compressed/base64 payload in QR (set MAX_INLINE_QR_BYTES ≈ 6KB safe default)
- Manifest + ZIP:
  1. Client uploads parts to pre-signed S3 URLs or to Distributor server
  2. Generate `manifest.json` with parts list, filenames, sizes, SHA‑256 checksums
  3. Optionally produce a ZIP and store at `/manifests/<id>/package.zip`
  4. QR points to manifest URL or shortlink
- Chunked uploads:
  - Upload parts in parallel, track part status in DB, provide resumable uploads and reassembly utilities

Security & retention:
- Short TTL on pre-signed URLs, virus-scan hosted uploads, bucket policies to prevent unintended public access, lifecycle rules for temp parts.

--------------------------------------------------------------------------------
Database & migrations
--------------------------------------------------------------------------------
- Use Flyway / Knex / Sequelize / Alembic or equivalent
- Test migrations in staging; run backups before prod migrations
- Recommended tables:
  - users, organizations, manifests, parts, uploads, invoices, subscriptions, audit_logs

--------------------------------------------------------------------------------
Secrets management & environment variables
--------------------------------------------------------------------------------
- Use AWS Secrets Manager, HashiCorp Vault, or equivalent
- Encrypt secrets at rest, avoid printing secrets in logs, rotate keys on schedule

--------------------------------------------------------------------------------
TLS, CORS, CSP and secure headers
--------------------------------------------------------------------------------
- Enforce HTTPS and HSTS, redirect HTTP → HTTPS
- CSP with `frame-ancestors` allowlist for partner embedding
- CORS limited to allowed origins per environment
- Set X-Content-Type-Options, Referrer-Policy, Permissions-Policy

--------------------------------------------------------------------------------
CI / CD (example pipelines)
--------------------------------------------------------------------------------
Suggested pipeline (GitHub Actions):
- PR: lint, unit tests, build, preview deploy
- Merge to `staging`: build, push image, migrate (staging), deploy staging, smoke tests
- Release (main): tag, run integration tests, backup DB, migrate (prod), deploy canary/blue-green, promote

Include pre-migration backups and automated smoke tests.

--------------------------------------------------------------------------------
Monitoring, logging & alerting
--------------------------------------------------------------------------------
- Errors: Sentry (map releases)
- Metrics: Prometheus/Grafana or Datadog (latency, error rate)
- Logs: structured JSON into ELK/Loki/CloudWatch
- Synthetic checks: monitor QR generation, manifest fetch, chunk reassembly
- Alerts with runbook links and escalation contacts

--------------------------------------------------------------------------------
Backups & disaster recovery
--------------------------------------------------------------------------------
- DB: nightly backups + PITR; retention per policy (e.g., 30 days)
- Storage: S3 versioning + cross-region replication for critical assets
- Test restores quarterly; document RTO/RPO and escalation matrix

--------------------------------------------------------------------------------
Security & vulnerability management
--------------------------------------------------------------------------------
- Dependency scanning (Dependabot/Snyk)
- Regular security patching cadence and periodic penetration tests
- Bug bounty or responsible disclosure via Support@cotopia.org
- Principle of least privilege for IAM/service accounts

--------------------------------------------------------------------------------
Operational runbooks (onboarding, support, escalations)
--------------------------------------------------------------------------------
Onboarding (Distributor):
- Execute Product Agreement & Tier selection with Cotopia (parent company)
- Pay Admin Fee and initial 90-day Continued License & Tech Support Fee
- Provision partner accounts and roles
- Access partner portal, docs, sample CSVs, and training
- Validate integration snippet in partner staging

Support model:
- Distributor-as-first-line (default); Cotopia provides Included Tech Support to Distributor personnel per Product Agreement
- Escalation levels (Distributor → Cotopia Support → Incident Bridge for Sev1)
- Required escalation info: org id, manifest id, sample files, reproduction steps, browser/device, timestamps, Sentry event IDs

--------------------------------------------------------------------------------
Pricing / Contract / Legal pointers (partner-facing)
--------------------------------------------------------------------------------
- Admin Fees (one-time): Tier 1 $349 | Tier 2 $549 | Tier 3 $749 | Tier 4 $949+ (examples)
- Continued License & Tech Support Fee (90-day cadence): initial $79 (due at execution), then $149 every 90 days
- Recruitment flow: Distributors may retain Admin Fee from recruited distributors but must remit recruited-distributor initial $79 to Cotopia within 5 business days (see Product Agreement)
- Do not promise Cotopia direct customer support or bespoke SLAs unless in Product Agreement/SOW
- Use Cotopia marks only per partner agreement; contact Support@cotopia.org for trademark use

Resources:
- Terms: /terms.html
- Privacy: /privacy.html
- Partner Guide: /guide.html

--------------------------------------------------------------------------------
Troubleshooting & FAQs
--------------------------------------------------------------------------------
- Camera not accessible locally: serve via HTTPS; use mkcert or ngrok
- Payload too large for QR: switch to manifest + hosted ZIP or compress payload
- Chunk upload failures: implement retry/resume, persist uploaded part indices
- Manifest mismatch: verify part SHA‑256, ensure correct concatenation order
- CORS download issues: set appropriate CORS headers on storage/CDN
- Webhook failures: validate webhook signature and public accessibility

--------------------------------------------------------------------------------
Checklist: pre-launch -> launch -> post-launch
--------------------------------------------------------------------------------
Pre-launch
- [ ] Prod/staging infra provisioned
- [ ] TLS certificates configured
- [ ] DB backups & restore tested
- [ ] CI/CD pipeline validated with canary
- [ ] Security scan completed
- [ ] Partner docs (terms/privacy/guide) published
- [ ] On-call support and escalation roster established

Launch
- [ ] Deploy via canary / blue-green
- [ ] Run smoke tests & e2e
- [ ] Verify monitoring & alerts
- [ ] Notify partners and update partner portal

Post-launch
- [ ] Track early tickets & metrics
- [ ] Collect partner feedback and iterate
- [ ] Review pricing & SLAs after pilot

--------------------------------------------------------------------------------
Useful commands & sample env template
--------------------------------------------------------------------------------
Migrations
- yarn migrate up
- yarn migrate down <version>

Build & dev
- yarn dev
- yarn build
- yarn start

Tests
- yarn test
- yarn test:e2e

Docker
- docker build -t cotopia-app:latest .
- docker run -e NODE_ENV=production -p 3000:3000 cotopia-app:latest

Sample `.env.example`
```
# App
NODE_ENV=development
APP_HOST=0.0.0.0
APP_PORT=3000
APP_ORIGIN=http://localhost:3000

# DB
DATABASE_URL=postgres://postgres:password@localhost:5432/cotopia?sslmode=disable

# Storage / S3
S3_BUCKET=cotopia-uploads-dev
S3_REGION=us-west-2
S3_ACCESS_KEY_ID=yourkey
S3_SECRET_ACCESS_KEY=yoursecret

# JWT / session
JWT_SECRET=replace-with-secure-random
SESSION_SECRET=replace-with-secure-random

# Payments (test)
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# Monitoring
SENTRY_DSN=

# Feature flags
HOST_UPLOADS=cotopia     # options: cotopia|client|distributor
MAX_INLINE_QR_BYTES=6000
```

--------------------------------------------------------------------------------
Appendix: links & references
--------------------------------------------------------------------------------
- Parent company: Cotopia — Wyoming  
- Partner & Distributor Guide: /guide.html  
- Terms of Service: /terms.html  
- Privacy Policy: /privacy.html  
- Support / contact: Support@cotopia.org

--------------------------------------------------------------------------------
Notes
--------------------------------------------------------------------------------
- Keep legal and pricing language synchronized with executed Product Agreements and the Master Agreement with Cotopia (parent company). This README is an operational guide — consult legal for contract wording and the Product Agreement for binding terms.

--------------------------------------------------------------------------------
End of README
--------------------------------------------------------------------------------