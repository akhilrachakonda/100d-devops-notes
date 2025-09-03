# Day 02 — Temporary user with expiry (App Server 3)

## Question
Create a user **mariyam** on **App Server 3 (stapp03)** with **expiry = 2024-04-15**. Ensure the username is lowercase and confirm the expiry date.

## Environment
- Jump host: `thor@jumphost`
- Target: `stapp03` (sudo user: `banner`)
- OS family: RHEL/CentOS-like (uses `useradd`, `chage`)
- Redactions: replace secrets with `<PASSWORD>` if shown

## My Answer (steps & commands)

**Plan / Rationale (short):**
- Create the user with an account expiration date using `useradd -e`.
- Verify the expiration details with `chage -l`.
- Keep username lowercase to follow standard policy.

**Commands (copy-pasteable):**
```bash
# 1) SSH to App Server 3 from the jump host
ssh banner@stapp03

# 2) Create user with an expiry date (YYYY-MM-DD format)
sudo useradd -e 2024-04-15 mariyam

# 3) Verify the expiry and password policies
sudo chage -l mariyam
