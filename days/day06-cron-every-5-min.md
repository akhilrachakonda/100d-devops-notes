# Day 06 — Cron every 5 minutes (all app servers)

## Question
Install **cronie**, start/enable the **crond** service, and create a **root cron** that runs every 5 minutes:
Apply this on **all app servers**.

## Environment
- Jump host: `thor@jumphost`
- Targets:
  - `stapp01` (sudo user: `tony`)
  - `stapp02` (sudo user: `steve`)
  - `stapp03` (sudo user: `banner`)
- OS family: RHEL/CentOS-like (uses `yum`, `cronie`, `systemctl`)
- Redactions: passwords replaced with `<PASSWORD>` where applicable

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Ensure cron tooling is present (`cronie` provides `crond`).
- Enable and start `crond` so cron jobs run and persist across reboots.
- Install a **root** cron entry that runs every 5 minutes.
- Verify both the service status and that `/tmp/cron_text` is created/updated.

**Commands (copy-pasteable):**

> Run the block for **each** server (change the SSH user/host accordingly).

```bash
# 0) SSH to the target server (example: stapp01)
ssh tony@stapp01

# 1) Install cronie (safe if already installed)
sudo yum install -y cronie

# 2) Enable and start the cron daemon
sudo systemctl enable --now crond

# 3) LAB-SIMPLE method: replace root's crontab with exactly one line (overwrites existing root cron!)
echo "*/5 * * * * echo hello > /tmp/cron_text" | sudo crontab -

# --- Optional safer method (append only if missing; keep existing entries) ---
# tmp=$(mktemp)
# sudo crontab -l 2>/dev/null > "$tmp" || true
# grep -q '/tmp/cron_text' "$tmp" || echo '*/5 * * * * echo hello > /tmp/cron_text' >> "$tmp"
# sudo crontab "$tmp"
# rm -f "$tmp"
# ---------------------------------------------------------------------------

# 4) Quick checks
sudo systemctl is-enabled crond
sudo systemctl is-active crond
sudo crontab -l
