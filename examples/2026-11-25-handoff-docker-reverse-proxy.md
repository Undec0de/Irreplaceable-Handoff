# Session Handoff — Caddy reverse proxy migration on VPS

## Session scope & starting point
Migrating three self-hosted Docker services (Nextcloud, Immich, and Grafana) on the Ubuntu VPS from manual Nginx configs to Caddy with automatic HTTPS via Cloudflare DNS-01 challenge. Constraint: zero downtime on Nextcloud during daytime hours and no plaintext API tokens committed to Git.

## Completed work & decisions
- [verified] Chose `caddy:2-alpine` built with the `caddy-dns/cloudflare` module instead of Nginx Proxy Manager — smaller footprint and declarative config in `/srv/proxy/Caddyfile`.
- [verified] Created `/srv/proxy/Dockerfile` and `/srv/proxy/docker-compose.yml` on branch `feat/caddy-migration` — pins Caddy and xcaddy versions for reproducible rebuilds.
- [verified] Configured `.env` loading via `$CLOUDFLARE_API_TOKEN` (stored only in `/srv/proxy/.env`, excluded by `.gitignore`).
- [verified] Tested Immich and Grafana routing on staging subdomains (`immich-test.example.com`, `grafana-test.example.com`) with valid TLS certificates.
- [unconfirmed] Drafted the commented-out Nextcloud CalDAV/CardDAV redirect rules in `/srv/proxy/Caddyfile` — not yet tested against live Nextcloud clients.

## Dead ends & constraints discovered
- Standard `caddy:2-alpine` image without custom build fails on `dns cloudflare` directive — do not revert to the stock image; keep the custom `Dockerfile`.
- Binding both Nginx and Caddy to host ports 80/443 simultaneously causes `address already in use` — staging tests must run on ports `8080`/`8443` until the final cutover window.
- Immich upload limit failed at 100 MB behind default proxy settings — added `request_body { max_size 10GB }` in the Immich block.

## Priority references
- Plan / spec file: `/srv/proxy/MIGRATION-PLAN.md` — cutover checklist and rollback steps; read first.
- `/srv/proxy/Caddyfile` — active reverse proxy routes for all three services (Nextcloud block still commented out).
- `/srv/proxy/docker-compose.yml` — service definitions, shared `proxy_net` network, and staging port mappings.

## Active state
- Background processes / jobs / routines: none
- Servers / ports / branches / connectors: branch `feat/caddy-migration` in `/srv/proxy`; staging container `caddy-staging` running on `127.0.0.1:8080` and `127.0.0.1:8443` (stop with `docker compose -f /srv/proxy/docker-compose.yml down`)
- Open drafts / pending approvals / external actions: waiting for user confirmation of the 15-minute maintenance window to stop Nginx and switch ports 80/443 to Caddy

## Verification & acceptance checks
- `docker exec caddy-staging caddy validate --config /etc/caddy/Caddyfile` — exits 0 with `Valid configuration`.
- `curl -I https://immich-test.example.com:8443` — returns `HTTP/2 200` with valid Let's Encrypt certificate.
- `git -C /srv/proxy status --short` — confirms `.env` is untracked/ignored and only `Dockerfile`, `docker-compose.yml`, and `Caddyfile` are staged.

## Deferred & open questions
- Deferred: Automated fail2ban log parsing for Caddy JSON access logs — scheduled after cutover is stable for 48 hours.
- Open: Confirm whether `cloud.example.com` (Nextcloud) can be cut over tonight after 22:00 CET — needed before uncommenting the Nextcloud block and stopping `nginx.service`.

## Next actions
- Start here: Confirm the maintenance window with the user and uncomment the Nextcloud block in `/srv/proxy/Caddyfile`.
- Then (ordered): 1) Run `caddy validate` on the updated config; 2) Stop `nginx.service` and rebind Caddy to ports 80/443 per `/srv/proxy/MIGRATION-PLAN.md`; 3) Verify Nextcloud web login and CalDAV/CardDAV sync from a live client.
