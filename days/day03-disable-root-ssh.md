# Day 03 — Disable direct root SSH (all app servers)

## Question
Disable **direct root SSH login** on all three app servers (**stapp01**, **stapp02**, **stapp03**). Keep normal user access working.

## Environment
- Jump host: `thor@jumphost`
- Targets:
  - `stapp01` (sudo user: `tony`)
  - `stapp02` (sudo user: `steve`)
  - `stapp03` (sudo user: `banner`)
- OS: RHEL/CentOS-like
- File edited: `/etc/ssh/sshd_config`
- Redactions: passwords replaced with `<PASSWORD>` where applicable

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Backup `sshd_config`.
- Ensure `PermitRootLogin no` is **set once** (replace if present, append if missing).
- Validate config with `sshd -t` **before** reload (avoid lockout).
- Reload `sshd` to apply without dropping current session.
- Verify: root login should fail; normal users still OK.

**Commands (copy-pasteable):**

> Do the block below **on each server**, adjusting the username/host:
> - stapp01 → `tony@stapp01`
> - stapp02 → `steve@stapp02`
> - stapp03 → `banner@stapp03`

```bash
# 0) SSH to the target as its sudo user
ssh tony@stapp01

# 1) Backup the server-side SSH daemon config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak-$(date +%Y%m%d)

# 2) If PermitRootLogin exists (commented or not), replace it; else, append once
if sudo grep -qE '^\s*#?\s*PermitRootLogin\b' /etc/ssh/sshd_config; then
  sudo sed -i 's/^\s*#\?\s*PermitRootLogin\s\+.*/PermitRootLogin no/' /etc/ssh/sshd_config
else
  echo 'PermitRootLogin no' | sudo tee -a /etc/ssh/sshd_config
fi

# 3) Validate syntax (no output means OK; non-zero exit means error)
sudo sshd -t -f /etc/ssh/sshd_config

# 4) Apply safely (reload keeps existing sessions)
sudo systemctl reload sshd

# 5) Show the effective directive
sudo grep -n '^\s*PermitRootLogin' /etc/ssh/sshd_config
