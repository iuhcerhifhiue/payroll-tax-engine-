# Agent notes

- The user-facing app is the Omnia site: `site/server.ts` (plain `node:http`, no npm deps, Node 22 runs `.ts` natively). It serves `site/index.html` (landing), `site/docs.html` (signup wizard + console), `site/reference.html`.
- Base44 dev env: `docker compose -f docker-compose.base44.yml up -d` → port 3000. `node --watch` restarts on changes in `site/`, `src/`, `data/`; HTML is read per request, but the browser needs a reload.
- Signup flow: landing form → `POST /api/signup` → docs.html opens on the verify step (handoff via `sessionStorage.omnia_pending`). With no `RESEND_API_KEY`, the 6-digit code is printed in `docker compose -f docker-compose.base44.yml logs site` (`CODE: ...`).
- Local account store: the `site-data` volume (`SITE_DB_DIR=/data`), deliberately outside the watched `site/` dir so writes don't restart the server. `docker compose -f docker-compose.base44.yml down -v` resets accounts.
- All secrets are optional; the site boots with none.
- Tests: `node --test ...` (see `npm test` in package.json) — run inside a `node:22` container.
- The `examples/*-server.ts` UIs (payroll, trades, calculator…) are separate demo servers, not wired into the Base44 compose.
