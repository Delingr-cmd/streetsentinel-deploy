# StreetSentinel — Deploy

Public install bundle for [StreetSentinel](https://github.com/Delingr-cmd/cctv-platform),
a self-hosted CCTV monitoring platform. This repo holds only the **non-secret**
deploy files (`install.sh`, `docker-compose.yml`, `.env.example`); the
application source stays private. The container images are public on GHCR.

## Install (one line)

On a clean **Linux (amd64)** host with `sudo`:

```bash
curl -fsSL https://raw.githubusercontent.com/Delingr-cmd/streetsentinel-deploy/main/install.sh | sudo bash
```

This will:

1. Install Docker Engine + the Compose plugin if missing.
2. Create `/opt/streetsentinel` and fetch `docker-compose.yml` + `.env.example`.
3. Generate all bootstrap secrets into `.env` (only if absent — safe to re-run).
4. Pull the public images and start the stack.
5. Wait for health, then print: `Open http://<server-ip>:8000 to finish setup.`

Open that URL and complete the **web setup wizard** (first admin → public URL →
Telegram → network → Cloudflare tunnel → AI). No file editing required.

## Images (public, GHCR)

| Service | Image |
|---------|-------|
| app | `ghcr.io/delingr-cmd/streetsentinel-app:stable` |
| cloudflared sidecar | `ghcr.io/delingr-cmd/streetsentinel-cloudflared:stable` |
| whatsapp bridge | `ghcr.io/delingr-cmd/streetsentinel-bridge:stable` |
| postgres | `postgres:18-bookworm` |

## Update / rollback

```bash
cd /opt/streetsentinel
docker compose pull && docker compose up -d   # update (migrate re-runs)
```

Pin a specific version by editing the `:stable` tags in
`/opt/streetsentinel/docker-compose.yml` to `:vX.Y.Z`. Full guides:
[UPDATING.md](https://github.com/Delingr-cmd/cctv-platform/blob/main/deploy/docker/UPDATING.md)
· [RELEASING.md](https://github.com/Delingr-cmd/cctv-platform/blob/main/deploy/docker/RELEASING.md).

## Branded URL (optional)

To serve the one-liner from `https://get.streetsentinel.app`, point a Cloudflare
Pages project (or any static host / redirect) at this repo, then run the
installer with `DEPLOY_BASE_URL=https://get.streetsentinel.app`.

> These files are kept in sync with the private app repo
> (`Delingr-cmd/cctv-platform`). Edit them there, not here.
