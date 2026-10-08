# AGENTS.md

## Project
Autobot Web is the standalone browser administration client for Autobot.

It allows users to manage Telegram channels, topics, AI-generated posts, schedules, moderation, publication history, and subscription usage through a full web interface.

Related repositories:
- `DinarSharipov/autobot-backend` — central NestJS API and business logic.
- `DinarSharipov/autobot` — grammY Telegram bot client.

## Communication
- All communication with the project owner must be in Russian unless the owner explicitly asks for another language.

## Technology stack
- React
- TypeScript
- Vite
- Docker
- HTTP/REST API client

## Architectural role
This repository contains only the Web Admin frontend.

The Web Admin is an independent client of the same backend used by the Telegram bot.

Expected communication:
```
Browser -> Autobot Web -> public HTTPS -> autobot-backend
Telegram -> grammY bot -> internal HTTP -> autobot-backend
```

The frontend MUST NOT:
- connect directly to PostgreSQL
- connect directly to Redis/BullMQ
- contain authoritative subscription/domain rules
- publish directly through Telegram Bot API
- depend on Telegram Mini App APIs

The NestJS backend is the source of truth.

## Deployment
The Web Admin will be deployed to a separate server from the bot/backend.

That Web server does not exist yet.

Therefore:
- do not use the current bot/backend server as the Web deployment target
- do not add current bot/backend server credentials to this repository
- Web deployment secrets will be configured only after the future Web server is provisioned
- Web Admin communicates with backend over a public HTTPS API, not through Docker DNS or `autobot-shared`

## Authentication
Authentication is Telegram-based and validated by the backend.

Rules:
- use backend authentication endpoints
- use secure HttpOnly cookie-based session handling
- send credentials with API requests where required
- never store authentication bearer tokens in localStorage
- bot and Web Admin must resolve to the same application user
- email/password auth is out of scope for MVP

## Main product areas
- Dashboard
- Publications/posts
- Calendar
- Topics
- Schedules
- Channels
- Moderation
- Publication history
- Subscription/usage

## Core product behavior
Supported post modes:
- immediate one-time
- scheduled one-time
- scheduled recurring

Publishing modes:
- AUTO
- MODERATION

Moderation/revision actions must update backend state so the Telegram bot immediately sees the same state.

## Engineering rules
- Keep business rules in backend.
- Use typed API/domain DTOs where practical.
- Keep server-state fetching separate from local UI state.
- Handle loading, empty, error, and permission/limit states explicitly.
- Do not duplicate backend entitlement checks as authoritative logic.
- Use environment configuration for backend public API URL.
- Do not hardcode production hostnames until infrastructure is provisioned.
