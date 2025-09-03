# Day 07 — Passwordless SSH from jump host (thor → all app servers)

## Question
Configure **passwordless SSH** from the jump host user **thor** to all app servers:
- `stapp01` (user: `tony`)
- `stapp02` (user: `steve`)
- `stapp03` (user: `banner`)

## Environment
- Jump host: `thor@jumphost`
- Targets: `tony@stapp01`, `steve@stapp02`, `banner@stapp03`
- Assumptions: OpenSSH available; home directories are standard
- Redactions: replace any passwords shown with `<PASSWORD>`

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Ensure thor has an SSH keypair (`~/.ssh/id_rsa` + `.pub`) with correct permissions.
- Install the **public key** on each target user’s `authorized_keys` using `ssh-copy-id`.
- Verify by forcing **publickey auth** (explicitly disabling password auth for the test).

**Commands (copy-pasteable):**
```bash
# On the jump host as thor

# 1) Create an SSH keypair if you don't already have one (no passphrase for lab)
[ -f ~/.ssh/id_rsa ] || ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa -C "thor@jumphost"

# 2) Ensure correct local permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub

# 3) Copy the public key to each app server (enter the password once per host)
ssh-copy-id -i ~/.ssh/id_rsa.pub tony@stapp01
ssh-copy-id -i ~/.ssh/id_rsa.pub steve@stapp02
ssh-copy-id -i ~/.ssh/id_rsa.pub banner@stapp03

# --- Fallback if ssh-copy-id is unavailable ---
# for u in "tony@stapp01" "steve@stapp02" "banner@stapp03"; do
#   cat ~/.ssh/id_rsa.pub | ssh "$u" 'mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys'
# done
# ------------------------------------------------

# 4) (Optional) Add hosts to known_hosts to avoid interactive prompts later
ssh -o StrictHostKeyChecking=accept-new tony@stapp01 'exit' || true
ssh -o StrictHostKeyChecking=accept-new steve@stapp02 'exit' || true
ssh -o StrictHostKeyChecking=accept-new banner@stapp03 'exit' || true
