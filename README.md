# 📺 Ads API

[![Fastify](https://img.shields.io/badge/Fastify-5-000000?logo=fastify&logoColor=white)](https://github.com/fastify/fastify)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://github.com/microsoft/TypeScript)
[![Node.js](https://img.shields.io/badge/Node.js-24-5FA04E?logo=nodedotjs&logoColor=white)](https://github.com/nodejs/node)
[![Zod](https://img.shields.io/badge/Zod-3-3068B7?logo=zod&logoColor=white)](https://github.com/colinhacks/zod)
[![Pino](https://img.shields.io/badge/Pino-9-687634?logo=pino&logoColor=white)](https://github.com/pinojs/pino)
[![pnpm](https://img.shields.io/badge/pnpm-10-F69220?logo=pnpm&logoColor=white)](https://github.com/pnpm/pnpm)
[![ESLint](https://img.shields.io/badge/ESLint-10-4B32C3?logo=eslint&logoColor=white)](https://github.com/eslint/eslint)
[![Prettier](https://img.shields.io/badge/Prettier-3-F7B93E?logo=prettier&logoColor=black)](https://github.com/prettier/prettier)
[![Vitest](https://img.shields.io/badge/Vitest-3-6E9F18?logo=vitest&logoColor=white)](https://github.com/vitest-dev/vitest)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![All Rights Reserved](https://img.shields.io/badge/License-All%20Rights%20Reserved-red.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-scottdreinhart%2Fads-api-181717?logo=github&logoColor=white)](https://github.com/scottdreinhart/ads-api)

Fastify API backend for ad serving, impression tracking, and revenue analytics across the game portfolio

**⚠️ PROPRIETARY SOFTWARE — All Rights Reserved**

© 2026 Scott Reinhart. This software is proprietary and confidential.
Unauthorized reproduction, distribution, or use is strictly prohibited.
See [LICENSE](LICENSE) file for complete terms and conditions.

> [!CAUTION]
> **LICENSE TRANSITION PLANNED** — This project is currently proprietary. The license will change to open source once the project has reached a suitable state to allow for it.

[Project Structure](#project-structure) · [Getting Started](#getting-started) · [Tech Stack](#tech-stack) · [API Reference](#api-reference) · [Architecture](#architecture) · [Environment Variables](#environment-variables) · [Authentication](#authentication) · [Error Handling](#error-handling) · [Rate Limiting](#rate-limiting) · [Data Models](#data-models) · [Deployment](#deployment) · [Supported Clients](#supported-clients) · [Versioning](#versioning) · [Remaining Work](#remaining-work) · [Future Improvements](#future-improvements) · [Contributing](#contributing) · [Portfolio Services](#portfolio-services) · [Portfolio Games](#portfolio-games)

## Project Structure

```
src/
├── domain/                           # Pure, framework-agnostic logic
│   ├── types.ts                      # Central type definitions (ad placements, campaigns, creatives, impressions, click events, revenue records, and frequency caps)
│   ├── errors.ts                     # Domain error classes (NotFoundError, ValidationError, etc.)
│   ├── constants.ts                  # Domain constants (page sizes, timeouts)
│   ├── schemas.ts                    # Zod validation schemas (pagination, ID params)
│   └── index.ts                      # Barrel export — re-exports all domain modules
├── app/
│   ├── app.ts                        # Fastify instance builder (plugins, routes, middleware)
│   └── index.ts                      # Barrel export — re-exports app builder
├── infra/
│   ├── config.ts                     # Environment configuration (validates env vars)
│   ├── logger.ts                     # Pino structured logger (standalone, non-request)
│   └── index.ts                      # Barrel export — re-exports infra modules
├── routes/
│   ├── health.ts                     # GET /health + GET /ready system endpoints
│   └── index.ts                      # Barrel export — re-exports all route modules
├── __tests__/
│   └── health.test.ts                # Health endpoint integration tests
└── server.ts                         # Entrypoint — starts the Fastify server

Dockerfile                            # Multi-stage production Docker image
.dockerignore                         # Docker build exclusions
.env.example                          # Environment variable template
package.json                          # Dependencies & scripts
pnpm-lock.yaml                        # pnpm lockfile
pnpm-workspace.yaml                   # pnpm workspace config
LICENSE                               # Proprietary license terms

tsconfig.json                         # TypeScript config (strict mode + @/ path aliases)
tsconfig.build.json                   # TypeScript build config (emits to dist/)
eslint.config.js                      # ESLint flat config (TypeScript + Prettier + boundary enforcement)
vitest.config.ts                      # Vitest test runner config (path aliases + coverage)
.prettierrc                           # Prettier formatting rules
.gitignore                            # Git ignore rules
.nvmrc                                # Node.js version pin (v24)
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) v24+ (pin via [nvm](https://github.com/nvm-sh/nvm) — see `.nvmrc`)
- [pnpm](https://pnpm.io/) v10+

### Install & Run

```bash
# Install dependencies
pnpm install

# Start development server (auto-restart on file changes)
pnpm dev

# Start production server (no file watching)
pnpm start

# Build for production (compiles TypeScript to dist/)
pnpm build

# Run compiled production build
pnpm preview
```

### Code Quality

```bash
# Individual tools
pnpm lint           # ESLint — check for issues
pnpm eslint:fix     # ESLint — auto-fix issues
pnpm prettier:fix   # Prettier — format all source files
pnpm format:check   # Prettier — check formatting without writing
pnpm typecheck      # TypeScript type check (tsc --noEmit)

# Chains
pnpm check          # lint + format:check + typecheck in one pass (quality gate)
pnpm fix            # eslint:fix + prettier:fix in one pass (auto-fix everything)
pnpm validate       # check + build — full pre-push validation
```
## Tech Stack

| Technology | Version | Purpose |
|---|---|---|
| [Fastify](https://github.com/fastify/fastify) | 5 | High-performance web framework |
| [TypeScript](https://github.com/microsoft/TypeScript) | 5.9 | Static type checking (strict mode) |
| [Node.js](https://github.com/nodejs/node) | 24 | Runtime (pinned via `.nvmrc`) |
| [Zod](https://github.com/colinhacks/zod) | 3 | Runtime schema validation |
| [Pino](https://github.com/pinojs/pino) | 9 | Structured JSON logging |
| [@fastify/swagger](https://github.com/fastify/fastify-swagger) | 9 | OpenAPI spec generation |
| [@fastify/swagger-ui](https://github.com/fastify/fastify-swagger-ui) | 5 | Interactive API documentation (`/docs`) |
| [@fastify/cors](https://github.com/fastify/fastify-cors) | 11 | Cross-origin resource sharing |
| [@fastify/helmet](https://github.com/fastify/fastify-helmet) | 13 | Security headers |
| [@fastify/rate-limit](https://github.com/fastify/fastify-rate-limit) | 10 | Request rate limiting |
| [Vitest](https://github.com/vitest-dev/vitest) | 3 | Test runner (V8 coverage) |
| [ESLint](https://github.com/eslint/eslint) | 10 | Linting (flat config + boundary enforcement) |
| [Prettier](https://github.com/prettier/prettier) | 3 | Code formatting |
| [pnpm](https://github.com/pnpm/pnpm) | 10 | Fast, disk-efficient package manager |
| [Docker](https://www.docker.com/) | — | Containerized production deployment |

## API Reference

Once the server is running, interactive OpenAPI documentation is available at:

```
http://localhost:3000/docs
```

### Built-in Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/health` | Health check — returns status, uptime, timestamp |
| `GET` | `/ready` | Readiness check — returns `{ ready: true }` when all dependencies are available |
| `GET` | `/docs` | Swagger UI — interactive API documentation |

### Proposed Endpoints

#### Campaigns

| Method | Path | Description |
|---|---|---|
| `POST` | `/campaigns` | Create an ad campaign (name, budget, date range, targeting rules) |
| `GET` | `/campaigns` | List campaigns (paginated, filterable by status/date) |
| `GET` | `/campaigns/:id` | Get campaign details and performance summary |
| `PATCH` | `/campaigns/:id` | Update campaign settings (budget, targeting, schedule) |
| `POST` | `/campaigns/:id/pause` | Pause a running campaign |
| `POST` | `/campaigns/:id/resume` | Resume a paused campaign |
| `DELETE` | `/campaigns/:id` | Archive a campaign |

#### Creatives

| Method | Path | Description |
|---|---|---|
| `POST` | `/creatives` | Upload an ad creative (image, text, CTA, destination URL) |
| `GET` | `/creatives` | List creatives (paginated, filterable by format/campaign) |
| `GET` | `/creatives/:id` | Get creative details and asset URLs |
| `PATCH` | `/creatives/:id` | Update creative metadata or replace assets |
| `DELETE` | `/creatives/:id` | Archive a creative |
| `POST` | `/creatives/:id/approve` | Approve a creative for serving (admin) |
| `POST` | `/creatives/:id/reject` | Reject a creative with reason (admin) |

#### Ad Placements

| Method | Path | Description |
|---|---|---|
| `POST` | `/placements` | Register an ad slot in a game (position, size, format, game ID) |
| `GET` | `/placements` | List all registered ad placements across games |
| `GET` | `/placements/:id` | Get placement configuration details |
| `PATCH` | `/placements/:id` | Update placement settings (size, position, refresh interval) |
| `DELETE` | `/placements/:id` | Deactivate a placement |

#### Ad Serving (Decision)

| Method | Path | Description |
|---|---|---|
| `GET` | `/serve` | Request an ad to display (`?placementId=&userId=&gameId=`) — returns the winning creative |
| `POST` | `/serve/batch` | Request multiple ads in a single call (for pre-caching) |

#### Tracking

| Method | Path | Description |
|---|---|---|
| `POST` | `/impressions` | Record an ad impression (placement, creative, user, timestamp) |
| `POST` | `/clicks` | Record an ad click event (placement, creative, user, timestamp) |
| `POST` | `/conversions` | Record a conversion event (purchase/signup triggered by an ad) |

#### Frequency Caps

| Method | Path | Description |
|---|---|---|
| `POST` | `/frequency-caps` | Create a frequency cap rule (max impressions per user/time window) |
| `GET` | `/frequency-caps` | List frequency cap rules (filterable by campaign/placement) |
| `PATCH` | `/frequency-caps/:id` | Update a frequency cap rule |
| `DELETE` | `/frequency-caps/:id` | Remove a frequency cap rule |

#### Revenue & Analytics

| Method | Path | Description |
|---|---|---|
| `GET` | `/analytics/revenue` | Revenue report (filterable by campaign/game/date range) |
| `GET` | `/analytics/impressions` | Impression report with fill rate and eCPM metrics |
| `GET` | `/analytics/clicks` | Click-through rate report by campaign/placement |
| `GET` | `/analytics/summary` | Dashboard summary — total revenue, impressions, CTR, top campaigns |

#### Audiences & Targeting

| Method | Path | Description |
|---|---|---|
| `POST` | `/audiences` | Create an audience segment (age range, geo, device, interests, game history) |
| `GET` | `/audiences` | List audience segments |
| `GET` | `/audiences/:id` | Get audience details and estimated reach |
| `PATCH` | `/audiences/:id` | Update audience targeting criteria |
| `DELETE` | `/audiences/:id` | Archive an audience segment |
| `POST` | `/audiences/:id/estimate` | Estimate daily impressions for an audience segment |

#### Ad Formats

| Method | Path | Description |
|---|---|---|
| `POST` | `/formats` | Register a supported ad format (banner, interstitial, rewarded video, native) |
| `GET` | `/formats` | List all supported ad formats with spec requirements |
| `GET` | `/formats/:id` | Get format details (dimensions, file types, max size, duration) |
| `PATCH` | `/formats/:id` | Update format specifications (admin) |

#### Rewarded Ads

| Method | Path | Description |
|---|---|---|
| `POST` | `/rewarded/start` | Signal that a user started watching a rewarded ad |
| `POST` | `/rewarded/complete` | Confirm rewarded ad completion and trigger reward callback |
| `GET` | `/rewarded/availability` | Check if a rewarded ad is available for a user + placement |
| `GET` | `/rewarded/history` | List reward grants for a user (paginated) |

#### Budgets & Pacing

| Method | Path | Description |
|---|---|---|
| `GET` | `/budgets/:campaignId` | Get budget status (spent, remaining, daily cap, pacing) |
| `PATCH` | `/budgets/:campaignId` | Update budget or daily spending cap |
| `GET` | `/budgets/:campaignId/forecast` | Forecast budget exhaustion date at current spend rate |

#### Blocklists

| Method | Path | Description |
|---|---|---|
| `POST` | `/blocklists` | Add an advertiser, category, or domain to the blocklist |
| `GET` | `/blocklists` | List all blocklist entries |
| `DELETE` | `/blocklists/:id` | Remove a blocklist entry |

#### Consent (GDPR / CCPA)

| Method | Path | Description |
|---|---|---|
| `GET` | `/consent/:userId` | Get user's current ad-tracking consent status |
| `POST` | `/consent` | Record or update user consent (opt-in/opt-out, consent string) |
| `DELETE` | `/consent/:userId` | Withdraw consent and purge tracking data for a user |

#### Scheduling

| Method | Path | Description |
|---|---|---|
| `POST` | `/schedules` | Create an ad schedule (dayparting — specify hours/days a campaign runs) |
| `GET` | `/schedules/:campaignId` | Get schedule for a campaign |
| `PATCH` | `/schedules/:campaignId` | Update schedule windows |
| `DELETE` | `/schedules/:campaignId` | Remove schedule (campaign runs continuously) |

### Client UI Pairings

Endpoints that pair directly with a visible UI element in game clients or the Ad Network admin app:

| UI Element | Icon / Control | Endpoint | Surface |
|---|---|---|---|
| Banner ad component | Rectangular ad slot rendered in game UI | `GET /serve` | Game client — bottom/top banner area |
| Interstitial overlay | Full-screen ad overlay between game rounds | `GET /serve` | Game client — between-game transition |
| Native ad card | Styled ad card blending with game content | `GET /serve` | Game client — menu/feed |
| Ad click-through | The ad creative itself (tappable/clickable) | `POST /clicks` | Game client — any ad surface |
| "Watch Ad for Reward" button | ▶ play icon + gift/coin icon | `POST /rewarded/start` | Game client — hint, extra life, or currency prompt |
| Reward claim animation | 🎁 gift box opening animation + "Claim" button | `POST /rewarded/complete` | Game client — post-ad reward overlay |
| Reward available indicator | 🎁 gift icon badge (pulsing when available) | `GET /rewarded/availability` | Game client — reward button visibility |
| "Close Ad" button | ✕ close icon (top-right corner, after timer) | Fires `POST /impressions` on close | Game client — interstitial/rewarded ad overlay |
| Ad-free badge | 🚫 "No Ads" badge for premium subscribers | `GET /entitlements/check` (via Billing API) | Game client — premium user indicator |
| Privacy consent toggle | 🔒 toggle switch ("Allow personalized ads") | `POST /consent` | Game client — settings / privacy screen |
| Consent status indicator | 🟢 / 🔴 dot next to privacy setting | `GET /consent/:userId` | Game client — settings screen |
| "Withdraw Consent" button | 🗑 trash icon + confirmation | `DELETE /consent/:userId` | Game client — privacy settings |
| Campaign pause button | ⏸ pause icon | `POST /campaigns/:id/pause` | Admin app — campaign list row |
| Campaign resume button | ▶ play icon | `POST /campaigns/:id/resume` | Admin app — campaign list row |
| Campaign status badge | 🟢 Active / 🟡 Paused / 🔴 Ended pill | `GET /campaigns/:id` | Admin app — campaign card |
| Creative approve button | ✅ green checkmark | `POST /creatives/:id/approve` | Admin app — creative review queue |
| Creative reject button | ❌ red X | `POST /creatives/:id/reject` | Admin app — creative review queue |
| Creative preview | Image/video preview in review panel | `GET /creatives/:id` | Admin app — creative detail |
| Budget progress bar | Progress bar (spent / total) | `GET /budgets/:campaignId` | Admin app — campaign detail |
| Budget forecast indicator | 📅 calendar icon with projected end date | `GET /budgets/:campaignId/forecast` | Admin app — campaign detail |
| Blocklist add button | 🚫 block icon + input field | `POST /blocklists` | Admin app — blocklist management |
| Blocklist remove button | ✕ remove icon on blocklist row | `DELETE /blocklists/:id` | Admin app — blocklist row |
| Revenue summary card | 💰 metric card with trend arrow | `GET /analytics/revenue` | Admin app — dashboard |
| Impressions chart | 📊 bar chart with fill rate overlay | `GET /analytics/impressions` | Admin app — analytics |
| CTR chart | 📈 line chart showing click-through rate | `GET /analytics/clicks` | Admin app — analytics |
| Dashboard summary | Multi-metric card grid (revenue, impressions, CTR) | `GET /analytics/summary` | Admin app — dashboard home |
| Audience reach estimator | 👥 user count with slider inputs | `POST /audiences/:id/estimate` | Admin app — audience builder |
| Schedule calendar | 📅 weekly calendar grid with active-hour highlighting | `GET /schedules/:campaignId` | Admin app — campaign scheduling |

## Architecture

This project enforces seven complementary design principles:

1. **CLEAN Architecture** (Layer Separation)
   - `domain/` layer: Pure types, errors, constants, schemas (zero framework dependencies)
   - `app/` layer: Fastify instance builder, use-case services
   - `infra/` layer: External adapters (database, cache, third-party APIs)
   - `routes/` layer: HTTP route handlers (thin — delegates to app/domain)
   - **Benefit**: Domain logic is testable, reusable, and framework-independent

2. **SOLID Principles** (Code-Level Design)
   - Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
   - **Benefit**: Code is maintainable, testable, and resistant to side effects

3. **DRY Principle** (No Duplication)
   - Shared schemas, constants, and error types eliminate duplication
   - **Benefit**: Changes propagate consistently; less code to maintain

4. **Import Boundary Enforcement** (`eslint-plugin-boundaries`)
   - `domain/` → may only import from `domain/` (zero framework deps)
   - `app/` → may import `domain/` + `app/` (never `infra/` or `routes/`)
   - `infra/` → may import `domain/`, `app/`, and `infra/`
   - `routes/` → may import `domain/`, `app/`, and `routes/`
   - **Benefit**: CLEAN layer violations are caught at lint time, not at code review

5. **Path Aliases** (`@/domain`, `@/app`, `@/infra`, `@/routes`)
   - Configured in `tsconfig.json` (`paths`) and `vitest.config.ts` (`resolve.alias`)
   - Eliminates fragile `../../` relative imports across layers
   - **Benefit**: Imports are self-documenting and resilient to file moves

6. **Barrel Exports** (`index.ts` per directory)
   - Each layer exposes a single public API via its barrel file
   - Internal module structure can change without breaking consumers
   - **Benefit**: Explicit public APIs; refactoring internals doesn't cascade import changes

7. **Schema-First Validation** (Zod + OpenAPI)
   - All inputs validated with Zod schemas in the domain layer
   - Fastify route schemas auto-generate OpenAPI docs via `@fastify/swagger`
   - **Benefit**: Single source of truth for validation, serialization, and documentation

## Environment Variables

Copy `.env.example` to `.env` and configure:

| Variable | Description | Type | Default | Required |
| -------- | ----------- | ---- | ------- | -------- |
| `PORT` | HTTP server port | `number` | `3000` | No |
| `HOST` | Bind address | `string` | `0.0.0.0` | No |
| `NODE_ENV` | Runtime environment (`development`, `staging`, `production`) | `string` | `development` | No |
| `LOG_LEVEL` | Pino log level (`fatal`, `error`, `warn`, `info`, `debug`, `trace`) | `string` | `info` | No |
| `DATABASE_URL` | PostgreSQL connection string | `string` | — | **Yes** (production) |
| `API_KEY` | Service-to-service API key for internal callers | `string` | — | **Yes** (production) |
| `JWT_SECRET` | Secret used to sign and verify JWT access tokens | `string` | — | **Yes** (production) |
| `BILLING_API_URL` | Internal Billing API base URL for subscription-tier lookups | `string` | `http://localhost:3001` | No |
| `BILLING_API_KEY` | API key for authenticating with the Billing API | `string` | — | **Yes** (production) |
| `ADMOB_APP_ID` | AdMob application ID for mediation | `string` | — | No |
| `UNITY_ADS_GAME_ID` | Unity Ads game ID for mediation | `string` | — | No |
| `CDN_BASE_URL` | Base URL for creative asset CDN | `string` | — | No |
| `STORAGE_BUCKET` | S3-compatible bucket for creative uploads | `string` | — | **Yes** (production) |
| `STORAGE_REGION` | Cloud storage region | `string` | `us-east-1` | No |
| `STORAGE_ACCESS_KEY` | Cloud storage access key ID | `string` | — | **Yes** (production) |
| `STORAGE_SECRET_KEY` | Cloud storage secret access key | `string` | — | **Yes** (production) |
| `CORS_ORIGIN` | Allowed CORS origin(s), comma-separated | `string` | `*` | No |
| `RATE_LIMIT_MAX` | Max requests per rate-limit window | `number` | `100` | No |
| `RATE_LIMIT_WINDOW_MS` | Rate-limit window duration in milliseconds | `number` | `60000` | No |

## Authentication

All non-public endpoints require authentication. The API supports two authentication methods:

### JWT Bearer Tokens (Game Clients & Admin Apps)

Game clients and admin apps authenticate by sending a JWT in the `Authorization` header:

```
Authorization: Bearer <token>
```

- Tokens are issued by the auth service upon successful login
- Tokens contain the user's `sub` (user ID) and `roles` array
- Tokens expire after a configurable TTL (default: 1 hour)
- Refresh tokens are used to obtain new access tokens without re-authentication

### API Keys (Service-to-Service)

Internal services authenticate with a static API key in the `X-API-Key` header:

```
X-API-Key: <key>
```

- Used by admin apps and other portfolio APIs for server-to-server calls
- Keys are configured via the `API_KEY` environment variable
- API key requests bypass user-scoped authorization checks

### Public Endpoints

The following endpoints do not require authentication:

| Endpoint | Purpose |
| -------- | ------- |
| `GET /health` | Health check |
| `GET /ready` | Readiness probe |
| `GET /docs` | Swagger UI |
| `GET /docs/json` | OpenAPI spec |

## Error Handling

All error responses follow a consistent JSON structure:

```json
{
  "statusCode": 422,
  "error": "Unprocessable Entity",
  "message": "Campaign 'summer-promo' has exceeded its daily budget",
  "code": "BUDGET_EXCEEDED"
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `statusCode` | `number` | HTTP status code |
| `error` | `string` | HTTP status text |
| `message` | `string` | Human-readable error description |
| `code` | `string?` | Machine-readable domain error code (optional) |

### HTTP Status Codes

| Status | Meaning | Example |
| ------ | ------- | ------- |
| `400` | Bad Request | Malformed JSON body or invalid creative dimensions |
| `401` | Unauthorized | Missing or expired JWT / invalid API key |
| `403` | Forbidden | Valid auth but insufficient role for campaign management |
| `404` | Not Found | Campaign, creative, or placement not found |
| `409` | Conflict | Duplicate placement slug or campaign name |
| `413` | Payload Too Large | Creative asset upload exceeds size limit |
| `422` | Unprocessable Entity | Business rule violation (e.g., budget exceeded, schedule conflict) |
| `429` | Too Many Requests | Rate limit exceeded |
| `500` | Internal Server Error | Unexpected failure |

### Domain Error Codes

| Code | Description |
| ---- | ----------- |
| `CAMPAIGN_NOT_FOUND` | No campaign exists with the given ID |
| `CAMPAIGN_INACTIVE` | Campaign is paused or ended; cannot serve ads |
| `CAMPAIGN_BUDGET_EXCEEDED` | Campaign has spent its daily or total budget |
| `CREATIVE_NOT_FOUND` | No creative exists with the given ID |
| `CREATIVE_NOT_APPROVED` | Creative is pending review and cannot be served |
| `CREATIVE_FORMAT_INVALID` | Creative dimensions or format do not match the target placement |
| `PLACEMENT_NOT_FOUND` | No ad placement exists with the given ID |
| `PLACEMENT_BLOCKLISTED` | Placement is on the advertiser's or publisher's blocklist |
| `IMPRESSION_DUPLICATE` | This impression has already been recorded (idempotency check) |
| `REWARD_ALREADY_CLAIMED` | Rewarded ad reward has already been granted |
| `REWARD_NOT_EARNED` | User has not completed the required ad view for the reward |
| `AUDIENCE_NOT_FOUND` | Target audience segment does not exist |
| `SCHEDULE_CONFLICT` | Campaign schedule overlaps with an exclusive placement reservation |
| `CONSENT_REQUIRED` | User has not provided GDPR/CCPA ad-tracking consent |
| `CONSENT_WITHDRAWN` | User has withdrawn ad-tracking consent |
| `FRAUD_DETECTED` | Click or impression flagged as fraudulent |

## Rate Limiting

Rate limiting is enforced via `@fastify/rate-limit` to protect against abuse:

| Scope | Limit | Window | Notes |
| ----- | ----- | ------ | ----- |
| **Global default** | 100 requests | 60 seconds | Per IP address |
| **Ad serving** | 300 requests | 60 seconds | High-frequency ad requests from game clients |
| **Impression tracking** | 500 requests | 60 seconds | Fire-and-forget pixel/beacon calls |
| **Creative uploads** | 10 requests | 60 seconds | Admin-only |
| **Campaign management** | 60 requests | 60 seconds | CRUD operations |
| **Reporting endpoints** | 30 requests | 60 seconds | Heavy queries |
| **Health / readiness** | Unlimited | — | Excluded from rate limiting |

Rate-limited responses include standard headers:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 47
X-RateLimit-Reset: 1719000060
Retry-After: 12
```

When the limit is exceeded, the API returns `429 Too Many Requests` with the error body above.

## Data Models

Planned domain entities for the ad system. These will be implemented as Zod schemas in `src/domain/types.ts`:

### Campaign

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique campaign identifier |
| `advertiserId` | `string (uuid)` | Advertiser's user ID |
| `name` | `string` | Campaign name |
| `status` | `'draft' \| 'active' \| 'paused' \| 'ended' \| 'archived'` | Campaign state |
| `startDate` | `string (ISO 8601)` | Campaign start date |
| `endDate` | `string (ISO 8601)?` | Campaign end date (`null` = ongoing) |
| `dailyBudget` | `number` | Daily spend limit in cents |
| `totalBudget` | `number` | Lifetime spend limit in cents |
| `spentToday` | `number` | Amount spent today in cents |
| `spentTotal` | `number` | Lifetime spend in cents |
| `targetAudienceIds` | `string[]` | Audience segments to target |
| `createdAt` | `string (ISO 8601)` | Creation timestamp |
| `updatedAt` | `string (ISO 8601)` | Last update timestamp |

### Creative

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique creative identifier |
| `campaignId` | `string (uuid)` | Parent campaign |
| `format` | `'banner' \| 'interstitial' \| 'native' \| 'rewarded_video'` | Ad format |
| `name` | `string` | Creative name |
| `assetUrl` | `string` | URL to the creative asset |
| `clickThroughUrl` | `string` | Destination URL on click |
| `width` | `number` | Width in pixels |
| `height` | `number` | Height in pixels |
| `status` | `'pending' \| 'approved' \| 'rejected'` | Review status |
| `rejectionReason` | `string?` | Reason for rejection |
| `createdAt` | `string (ISO 8601)` | Creation timestamp |

### Placement

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique placement identifier |
| `slug` | `string` | URL-friendly placement name (e.g., `game-over-interstitial`) |
| `gameId` | `string` | Which game this placement belongs to |
| `format` | `'banner' \| 'interstitial' \| 'native' \| 'rewarded_video'` | Accepted ad format |
| `position` | `string` | Screen position (e.g., `bottom`, `fullscreen`, `in-feed`) |
| `active` | `boolean` | Whether the placement is live |

### Impression

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique impression identifier |
| `campaignId` | `string (uuid)` | Campaign that was served |
| `creativeId` | `string (uuid)` | Creative that was displayed |
| `placementId` | `string (uuid)` | Where the ad was shown |
| `userId` | `string (uuid)?` | Viewer's user ID (if authenticated) |
| `deviceId` | `string?` | Anonymous device fingerprint |
| `timestamp` | `string (ISO 8601)` | When the impression occurred |
| `viewDurationMs` | `number?` | How long the ad was visible |
| `revenue` | `number` | Revenue earned in cents |

### ClickEvent

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Unique click identifier |
| `impressionId` | `string (uuid)` | Associated impression |
| `timestamp` | `string (ISO 8601)` | When the click occurred |
| `clickThroughUrl` | `string` | Destination URL |
| `fraudScore` | `number` | Fraud risk score (0.0–1.0) |
| `flagged` | `boolean` | Whether the click is flagged as suspicious |

### FrequencyCap

| Field | Type | Description |
| ----- | ---- | ----------- |
| `campaignId` | `string (uuid)` | Campaign this cap applies to |
| `maxImpressions` | `number` | Maximum impressions per user |
| `windowHours` | `number` | Time window in hours |
| `scope` | `'user' \| 'device' \| 'session'` | Scope of the cap |

### ConsentRecord

| Field | Type | Description |
| ----- | ---- | ----------- |
| `userId` | `string (uuid)` | User's ID |
| `consentGiven` | `boolean` | Whether consent was granted |
| `consentType` | `'gdpr' \| 'ccpa' \| 'coppa'` | Regulation type |
| `grantedAt` | `string (ISO 8601)` | When consent was given |
| `withdrawnAt` | `string (ISO 8601)?` | When consent was withdrawn |
| `ipAddress` | `string` | IP at time of consent action |

### RewardedAdSession

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `string (uuid)` | Session identifier |
| `userId` | `string (uuid)` | User who watched the ad |
| `creativeId` | `string (uuid)` | Rewarded creative shown |
| `status` | `'started' \| 'completed' \| 'claimed' \| 'expired'` | Session state |
| `rewardType` | `string` | Type of reward (e.g., `coins`, `hints`, `extra_life`) |
| `rewardAmount` | `number` | Amount of the reward |
| `startedAt` | `string (ISO 8601)` | When the ad started playing |
| `completedAt` | `string (ISO 8601)?` | When the ad finished playing |
| `claimedAt` | `string (ISO 8601)?` | When the reward was claimed |

## Deployment

### Docker (Recommended)

```bash
# Build the production image
docker build -t ads-api .

# Run with environment variables
docker run -d \
  --name ads-api \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -e DATABASE_URL=postgresql://user:pass@db:5432/ads \
  -e JWT_SECRET=your-jwt-secret \
  -e BILLING_API_URL=http://billing-api:3000 \
  -e BILLING_API_KEY=internal-key \
  -e STORAGE_BUCKET=ad-creatives \
  -e STORAGE_ACCESS_KEY=AKIA... \
  -e STORAGE_SECRET_KEY=... \
  ads-api
```

### Health Checks

Configure your orchestrator (Docker Compose, Kubernetes, ECS) to use the built-in probes:

| Probe | Endpoint | Interval | Timeout | Failure Threshold |
| ----- | -------- | -------- | ------- | ----------------- |
| Liveness | `GET /health` | 30s | 5s | 3 |
| Readiness | `GET /ready` | 10s | 5s | 3 |

### Production Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Provide all **required** environment variables (see [Environment Variables](#environment-variables))
- [ ] Configure database connection pooling (recommended: 10–20 connections)
- [ ] Enable TLS termination at the load balancer / reverse proxy
- [ ] Set up database migrations before first deploy
- [ ] Configure cloud storage for creative assets
- [ ] Configure CDN for serving creative assets with low latency
- [ ] Configure log aggregation (Pino outputs structured JSON)
- [ ] Set up monitoring alerts on `/health` and `/ready`
- [ ] Enable CORS for specific origins (do not use `*` in production)
- [ ] Set up fraud-detection alerting thresholds
- [ ] Configure GDPR/CCPA consent flow before serving personalized ads

## Supported Clients

This API is consumed by the following applications:

| Client | Type | Description |
| ------ | ---- | ----------- |
| **[📺 Ad Network](https://github.com/scottdreinhart/ad-network)** | Admin App | Campaign management, creative approval, revenue reports |
| **All portfolio games** | Game Clients | Ad requests, impression tracking, rewarded ad flows |
| **[💳 Billing API](https://github.com/scottdreinhart/billing-api)** | API (internal) | Subscription-tier checks to determine ad-free eligibility |

## Versioning

The API uses **URL-prefix versioning**:

```
https://api.example.com/v1/campaigns
https://api.example.com/v1/placements
```

- All current endpoints are under `/v1/`
- Breaking changes will be introduced under `/v2/` with a deprecation notice on `/v1/`
- Non-breaking additions (new fields, new endpoints) are added to the current version
- Deprecated versions will be supported for a minimum of **6 months** after the successor is released
- The OpenAPI spec at `/docs/json` includes the version in its `info.version` field

## Remaining Work

### Core Functionality

- [ ] **Domain model implementation** — define entities, value objects, and business rules for ad placements, campaigns, creatives, impressions, click events, revenue records, and frequency caps
- [ ] **Database integration** — connect to PostgreSQL/SQLite via Drizzle ORM or Prisma
- [ ] **CRUD route scaffolding** — RESTful endpoints for all domain entities
- [ ] **Authentication** — JWT or API key verification middleware
- [ ] **Authorization** — role-based access control for admin vs. client operations

### Code Quality

```bash
# Individual tools
pnpm lint           # ESLint — check for issues
pnpm eslint:fix     # ESLint — auto-fix issues
pnpm prettier:fix   # Prettier — format all source files
pnpm format:check   # Prettier — check formatting without writing
pnpm typecheck      # TypeScript type check (tsc --noEmit)

# Chains
pnpm check          # lint + format:check + typecheck in one pass (quality gate)
pnpm fix            # eslint:fix + prettier:fix in one pass (auto-fix everything)
pnpm validate       # check + build — full pre-push validation
```
## Future Improvements

- [ ] **Real-time bidding (RTB)** — integrate with ad exchanges for programmatic demand alongside direct-sold campaigns
- [ ] **A/B creative testing** — automatically split traffic between creative variants and report performance differences
- [ ] **Smart frequency capping** — ML-based optimal frequency detection instead of static per-user limits
- [ ] **Rewarded ad support** — serve reward-gated video/interstitial ads that grant in-game currency or hints on completion
- [ ] **Ad mediation layer** — waterfall/hybrid mediation across AdMob, Unity Ads, and direct campaigns to maximize fill rate
- [ ] **Geo-targeting & device targeting** — serve different creatives based on user location, device type, or OS
- [ ] **Fraud detection** — flag suspicious click patterns (rapid-fire, bot signatures) and exclude them from billing
- [ ] **Advertiser self-serve portal** — API endpoints supporting a self-service campaign builder for third-party advertisers
- [ ] **Real-time analytics WebSocket** — stream live impression/click/revenue events to the Ad Network admin dashboard
- [ ] **GDPR/CCPA consent endpoints** — manage user ad-tracking consent status and serve consent-appropriate creatives

## Portfolio Services

Infrastructure services supporting the game portfolio:

| Service | Description |
| ------- | ----------- |
| **[💳 Game Billing](https://github.com/scottdreinhart/game-billing)** | Payment processing & subscription management (Admin App) |
| **[🎨 Theme Store](https://github.com/scottdreinhart/theme-store)** | DLC theme downloader & manager (Admin App) |
| **[📺 Ad Network](https://github.com/scottdreinhart/ad-network)** | Ad serving & revenue management (Admin App) |
| **[💳 Billing API](https://github.com/scottdreinhart/billing-api)** | Fastify API backend for billing |
| **[🎨 Themes API](https://github.com/scottdreinhart/themes-api)** | Fastify API backend for themes |
| **[🏆 Rankings API](https://github.com/scottdreinhart/rankings-api)** | Fastify API backend for rankings & leaderboards |

## Portfolio Games

All games in this portfolio share the same React + Vite + TypeScript + CLEAN architecture stack:

| Game | Description | Complexity |
| ---- | ----------- | ---------- |
| **[Tic-Tac-Toe](https://github.com/scottdreinhart/tictactoe)** | Classic 3×3 grid game with 4 AI difficulty levels and series mode | Baseline — the reference architecture |
| **[Shut the Box](https://github.com/scottdreinhart/shut-the-box)** | Roll dice, flip numbered tiles to match the total; lowest remaining sum wins | Similar — grid UI + dice logic |
| **[Mancala (Kalah)](https://github.com/scottdreinhart/mancala)** | Two-row pit-and-stones capture game; simple rules, satisfying chain moves | Slightly higher — seed-sowing animation |
| **[Connect Four](https://github.com/scottdreinhart/connect-four)** | Drop discs into a 7×6 grid; first to four in a row wins | Similar — larger grid, same win-check pattern |
| **[Simon Says](https://github.com/scottdreinhart/simon-says)** | Repeat a growing sequence of colors/sounds; memory challenge | Similar — leverages existing Web Audio API |
| **[Lights Out](https://github.com/scottdreinhart/lights-out)** | Toggle a 5×5 grid of lights; goal is to turn them all off | Similar — grid + toggle logic |
| **[Nim](https://github.com/scottdreinhart/nim)** | Players take turns removing objects from piles; last to take loses | Simpler — minimal UI, pure strategy |
| **[Hangman](https://github.com/scottdreinhart/hangman)** | Guess letters to reveal a hidden word before the stick figure completes | Similar — alphabet grid + SVG drawing |
| **[Memory / Concentration](https://github.com/scottdreinhart/memory-game)** | Flip cards to find matching pairs on a grid | Similar — grid + flip animation |
| **[2048](https://github.com/scottdreinhart/2048)** | Slide numbered tiles on a 4×4 grid; merge matching tiles to reach 2048 | Slightly higher — swipe input + merge logic |
| **[Reversi (Othello)](https://github.com/scottdreinhart/reversi)** | Place discs to flip opponent's pieces; most discs wins | Moderately higher — flip-chain logic + AI |
| **[Checkers](https://github.com/scottdreinhart/checkers)** | Classic diagonal-move capture board game | Higher — move validation + multi-jump |
| **[Battleship](https://github.com/scottdreinhart/battleship)** | Place ships on a grid, take turns guessing opponent locations | Moderately higher — two-board UI + ship placement |
| **[Snake](https://github.com/scottdreinhart/snake)** | Steer a growing snake to eat food without hitting walls or itself | Different — real-time game loop instead of turn-based |
| **[Monchola](https://github.com/scottdreinhart/monchola)** | Traditional dice/board race game with capture mechanics | Similar — dice roll + board path + capture rules |
| **[Rock Paper Scissors](https://github.com/scottdreinhart/rock-paper-scissors)** | Best-of-N rounds against the CPU with hand animations | Simpler — minimal state, animation-focused |
| **[Minesweeper](https://github.com/scottdreinhart/minesweeper)** | Reveal cells on a minefield grid without detonating hidden mines | Moderately higher — flood-fill reveal + flag logic |

## Contributing

This is proprietary software. Contributions are accepted by invitation only.

If you have been granted contributor access:

1. Create a feature branch from `main`
2. Make focused, single-purpose commits with clear messages
3. Run `pnpm validate` before pushing (lint + format + build gate)
4. Submit a pull request with a description of the change

See the [LICENSE](LICENSE) file for usage restrictions.

## License

Copyright © 2026 Scott Reinhart. All Rights Reserved.

This project is proprietary software. No permission is granted to use, copy, modify, or distribute this software without the prior written consent of the owner. See the [LICENSE](LICENSE) file for full terms.

---
[⬆ Back to top](#-ads-api)
