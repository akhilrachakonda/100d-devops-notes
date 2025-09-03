# Day 05 — SELinux packages & disabled (App Server 1)

## Question
Install the required **SELinux packages** and **permanently disable SELinux** on **App Server 1 (stapp01)** so it remains disabled after reboot.

## Environment
- Jump host: `thor@jumphost`
- Target: `stapp01` (sudo user: `tony`)
- OS family: RHEL/CentOS-like
- Files: `/etc/selinux/config`
- Redactions: replace secrets with `<PLACEHOLDER>`

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Ensure SELinux tooling/policies are installed (some systems need them even to manage/inspect SELinux).
- Set `SELINUX=disabled` in `/etc/selinux/config` for **permanent** disable.
- Optionally switch runtime to **Permissive** until the next reboot (full “Disabled” requires reboot).
- Verify configuration and current mode.

**Commands (copy-pasteable):**
```bash
# 1) SSH to App Server 1
ssh tony@stapp01

# 2) Install SELinux-related packages (tools + policies)
#    Note: some might already be present; this is safe to run.
sudo yum install -y \
  selinux-policy selinux-policy-targeted policycoreutils libselinux-utils diffutils \
  rpm-plugin-selinux || true

# 3) Check current runtime mode (Enforcing/Permissive/Disabled)
getenforce || true

# 4) Set permanent mode to 'disabled' in /etc/selinux/config
#    - If line exists, replace it; if missing, append it once.
if sudo grep -q '^SELINUX=' /etc/selinux/config; then
  sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
else
  echo 'SELINUX=disabled' | sudo tee -a /etc/selinux/config
fi

# 5) (Optional) Soften enforcement immediately until reboot
#    - 'setenforce 0' switches to Permissive at runtime (ignored if already Disabled)
sudo setenforce 0 2>/dev/null || echo "Runtime already Disabled; will remain so after reboot via config."

# 6) Show the effective config line
grep '^SELINUX=' /etc/selinux/config

# 7) Show current runtime status (won't become fully 'Disabled' until reboot)
getenforce || true
# If available:
sestatus 2>/dev/null || true
