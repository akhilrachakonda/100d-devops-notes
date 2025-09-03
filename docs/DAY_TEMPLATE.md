# Day XX — <short title>

## Question
> Paste the lab prompt.

## Environment
- Hosts/users (redact secrets).

## My Answer (steps & commands)
```bash
# commands…
### 2) Add the eight day logs you finished
```bash
cat > days/day01-user-noninteractive.md <<'MD'
# Day 01 — User with non-interactive shell (App Server 2)
## Question
Create a user **kareem** with a **non-interactive shell** on **App Server 2**.
## My Answer
```bash
ssh steve@stapp02
sudo useradd -s /sbin/nologin kareem || sudo useradd -s /bin/false kareem
grep '^kareem:' /etc/passwd
