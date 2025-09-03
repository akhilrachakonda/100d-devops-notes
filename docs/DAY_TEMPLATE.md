# Day XX — <short title>

## Question
> Paste the exact lab prompt (or a concise paraphrase).

## Environment
- Hosts/users (redact secrets with <PLACEHOLDER>).

## My Answer (steps & commands)
```bash
# commands you ran + brief why
# Day XX — <short title>

## Question
> Paste the exact lab prompt (or a concise paraphrase).

## Environment
- Hosts/users (redact secrets with <PLACEHOLDER>).

## My Answer (steps & commands)
```bash
# commands you ran + brief why
---

## 5) (Optional) Add Day 01 if it’s missing on `main`
If the file already exists, this will **do nothing**. If it’s missing, this creates a minimal Day 01 entry.
```bash
[ -f days/day01-user-noninteractive.md ] || mkdir -p days
[ -f days/day01-user-noninteractive.md ] || cat > days/day01-user-noninteractive.md <<'MD'
# Day 01 — User with non-interactive shell (App Server 2)

## Question
Create a user **kareem** with a **non-interactive shell** on **App Server 2**.

## Environment
- Jump host: `thor@jumphost`
- App server: `stapp02` (user: `steve`)

## My Answer (steps & commands)
```bash
# From jump host
ssh steve@stapp02
sudo useradd -s /sbin/nologin kareem || sudo useradd -s /bin/false kareem
grep '^kareem:' /etc/passwd
---

## 6) Stage and commit your Phase-1 baseline
This uses your Conventional Commit style.
```bash
git add README.md PROGRESS.md LICENSE docs/DAY_TEMPLATE.md days/day01-user-noninteractive.md 2>/dev/null || true
git commit -m "docs(repo): baseline README, progress, template, license"
git push
