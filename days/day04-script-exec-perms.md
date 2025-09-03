# Day 04 — Script executable by all (App Server 2)

## Question
Grant **executable permissions for all users** to the script `/tmp/xfusioncorp.sh` on **App Server 2**.

## Environment
- Jump host: `thor@jumphost`
- Target: `stapp02` (sudo user: `steve`)
- OS family: RHEL/CentOS-like
- Redactions: replace secrets with `<PLACEHOLDER>`

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Ensure the script exists at the stated path.
- Set permissions to **755** so: owner `rwx`, group `rx`, others `rx`.
- Verify with `ls -l` to confirm `-rwxr-xr-x`.

**Commands (copy-pasteable):**
```bash
# 1) SSH to App Server 2
ssh steve@stapp02

# 2) Confirm the script exists
ls -l /tmp/xfusioncorp.sh

# 3) Make it executable for all users (owner keeps write)
sudo chmod 755 /tmp/xfusioncorp.sh

# 4) Verify permissions
ls -l /tmp/xfusioncorp.sh
