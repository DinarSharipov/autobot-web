# IMPLEMENTATION_PLAN.md

## Goal
Build a standalone React + TypeScript Web Admin for Autobot that uses the common NestJS backend and is deployable independently to a future dedicated Web server.

## Planned implementation

### 1. Bootstrap
- Initialize React + TypeScript + Vite.
- Add routing.
- Add environment configuration.
- Add API client.
- Add linting/formatting/testing baseline.
- Add Dockerfile for production build/runtime.

### 2. Backend API integration
- Configure public backend API URL through environment variables.
- Use HTTPS in production.
- Support credentialed requests for authenticated endpoints.
- Centralize API error mapping.
- Do not call PostgreSQL, Redis, or Telegram Bot API directly.

### 3. Telegram authentication
Implement browser authentication flow backed by `autobot-backend`.

Requirements:
- Telegram identity login
- backend validation
- secure cookie-based session
- authenticated session bootstrap endpoint
- logout
- no tokens in localStorage

### 4. Application shell
Implement:
- authenticated layout
- navigation
- route guards
- loading/error boundaries
- responsive desktop/mobile behavior

### 5. Product areas
Implement:
- Dashboard
- Publications/posts
- Calendar
- Topics
- Schedules
- Channels
- Moderation
- Publication history
- Subscription/usage

### 6. Post workflows
Support through backend API:
- immediate one-time posts
- scheduled one-time posts
- recurring posts
- AUTO mode
- MODERATION mode
- revision requests
- approve/reject actions
- image generation where entitlement allows it

### 7. Cross-client consistency
- Backend state is authoritative.
- Refresh/invalidate server state after mutations.
- Changes made in Telegram bot must appear in Web Admin.
- Changes made in Web Admin must appear in Telegram bot.
- Do not introduce frontend-only domain state that can diverge from backend.

### 8. Deployment
The dedicated Web server has not been provisioned yet.

For now:
- build/test CI may be implemented
- production deployment remains disabled/pending
- do not use credentials of the existing bot/backend server
- do not assume access to `autobot-shared`
- keep production API URL configurable

When the Web server is created:
- configure repository-specific Actions deployment secrets
- expose Web Admin over HTTPS
- configure DNS/TLS
- configure backend CORS allowlist for the Web origin
- enable deployment job

### 9. CI/CD
Before Web server exists:
- lint
- typecheck
- test
- build

After Web server provisioning add:
- `SERVER_HOST`
- `SERVER_PORT`
- `SERVER_USER`
- `SERVER_SSH_KEY`
- `SERVER_KNOWN_HOSTS`
- deployment workflow

Do not reuse deployment credentials from `autobot` or `autobot-backend`.
