# Day 08 — Install Ansible 4.7.0 via pip3 (jump host, global availability)

## Question
Install **Ansible 4.7.0** on the **jump host** using **pip3**, and make sure the `ansible` binary is **available to all users** on the system.

## Environment
- Host: jump host (`thor`)
- OS family: RHEL/CentOS-like (Python 3.9+)
- Package source: `pip3` (system-wide)
- Paths involved: `/usr/local/bin`, `/usr/bin`, `/etc/profile.d/`
- Redactions: replace secrets with `<PLACEHOLDER>` if shown

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Ensure `pip3` is reachable by `sudo` (some systems install it to `/usr/local/bin` only).
- Install exact version **ansible==4.7.0** system-wide via `pip3`.
- Make the CLI globally reachable for every user:
  - **Preferred:** add `/usr/local/bin` to global PATH via `/etc/profile.d/`.
  - **Fallback:** symlink `ansible*` into `/usr/bin`.
- Verify version and that a non-root user can run it.

**Commands (copy-pasteable):**
```bash
# 0) Confirm where pip3 lives (expect /usr/local/bin/pip3 on many hosts)
ls -l /usr/local/bin/pip3 /usr/bin/pip3 2>/dev/null || true

# 1) Expose /usr/local/bin/pip3 to sudo if /usr/bin/pip3 is missing
[ -x /usr/local/bin/pip3 ] && sudo ln -sf /usr/local/bin/pip3 /usr/bin/pip3 || true

# 2) Install exact Ansible version (system-wide)
sudo pip3 install --no-cache-dir --upgrade --force-reinstall ansible==4.7.0

# 3) Preferred: make /usr/local/bin available to all users via global PATH
echo 'export PATH=/usr/local/bin:$PATH' | sudo tee /etc/profile.d/localbin.sh >/dev/null
sudo chmod 644 /etc/profile.d/localbin.sh
# apply to current shell (new logins will inherit automatically)
source /etc/profile.d/localbin.sh

# 4) Fallback (if needed): ensure ansible binaries are visible under /usr/bin
[ -x /usr/local/bin/ansible ] && sudo ln -sf /usr/local/bin/ansible /usr/bin/ansible || true
[ -x /usr/local/bin/ansible-playbook ] && sudo ln -sf /usr/local/bin/ansible-playbook /usr/bin/ansible-playbook || true
[ -x /usr/local/bin/ansible-inventory ] && sudo ln -sf /usr/local/bin/ansible-inventory /usr/bin/ansible-inventory || true
