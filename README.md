# GoPhish Project - Ready-to-run Package

This package contains a ready-to-run GoPhish phishing simulation environment (Docker Compose) plus example templates, landing page, sample targets, and documentation.

**Important:** This is intended for authorized internal testing only. Obtain written approval from management, legal, and HR before running phishing simulations. Misuse may violate laws and policies.

## Contents
- `docker-compose.yml` - brings up GoPhish and an nginx reverse proxy.
- `nginx/conf.d/gophish.conf` - sample nginx config to proxy phishing landing pages.
- `nginx/certs/` - expected location for TLS certs (place your `fullchain.pem` and `privkey.pem` here).
- `templates/` - sample email & landing page HTML.
- `targets/targets_sample.csv` - sanitized example target list.
- `report_template.md` - campaign reporting template.
- `ethics_and_consent.md` - template for approvals & consent.

## Quick start (Linux host with Docker)
1. Place your TLS certs (fullchain.pem + privkey.pem) into `nginx/certs/`.
2. Edit `nginx/conf.d/gophish.conf` and change `server_name` to the domain you will use for landing pages (e.g., `phish.example.internal`).
3. Start services:
   ```bash
   docker compose up -d
   ```
4. GoPhish Admin UI: by default, GoPhish admin listens on port `3333` of the host. **Do not** expose this to the public internet — use a VPN/SSH tunnel.
   Access: `https://<host-ip>:3333` (or configure admin to listen on localhost only and use `ssh -L`).
5. Upload templates/landing pages and import `targets/targets_sample.csv` via the GoPhish UI.
6. Configure a Sending Profile (SMTP). For testing use Mailtrap or MailHog. For internal campaigns, coordinate with IT for a dedicated sending domain and SPF/DKIM only with authorization.

## Files to customize before launch
- `templates/email_verify_account.html` - edit subject/body as needed.
- `templates/landing_login.html` - customize look; do NOT capture real credentials without an IR plan.
- `targets/targets_sample.csv` - replace with authorized target CSV.

## Legal & Ethics checklist (must be completed before running)
1. Written approval from executive sponsor and HR.
2. Scope: approved user groups, excluded users, timeframe.
3. Communication plan for notifying impacted users.
4. Incident response plan for credential capture.
5. Data retention & deletion policy.

## Backups
Database file is in the `gophish_data` volume (container path `/opt/gophish`). Backup `gophish.db` regularly.

## Support
This package is a starting point. Refer to GoPhish official docs for advanced configuration: https://getgophish.com (not included here).
