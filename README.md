# 100 Days of DevOps — Notes & Hands-On

A daily log of my **KodeKloud** DevOps labs and hands-on tasks.  
Each entry is short, reproducible, and uses a consistent 5-section template.

---

## 📁 Repo Structure

- `days/` — each day’s Q&A write-up  
- `docs/DAY_TEMPLATE.md` — template I use for every day
- `PROGRESS.md` — running checklist of days completed
- `LICENSE` — MIT  
- `.editorconfig`, `.gitattributes`, `.gitignore` — formatting & hygiene

---

## 🔖 Daily Logs

- Day 01 — User with non-interactive shell → [`days/day01-user-noninteractive.md`](days/day01-user-noninteractive.md)  
- Day 02 — Temporary user with expiry → [`days/day02-temp-user-expiry.md`](days/day02-temp-user-expiry.md)  
- Day 03 — Disable direct root SSH → [`days/day03-disable-root-ssh.md`](days/day03-disable-root-ssh.md)  
- Day 04 — Script executable for all → [`days/day04-script-exec-perms.md`](days/day04-script-exec-perms.md)  
- Day 05 — SELinux packages & disabled → [`days/day05-selinux-disable.md`](days/day05-selinux-disable.md)  
- Day 06 — Cron every 5 min → [`days/day06-cron-every-5-min.md`](days/day06-cron-every-5-min.md)  
- Day 07 — Passwordless SSH → [`days/day07-passwordless-ssh.md`](days/day07-passwordless-ssh.md)  
- Day 08 — Ansible 4.7.0 via pip3 → [`days/day08-ansible-4-7-0-pip3.md`](days/day08-ansible-4-7-0-pip3.md)
- Day 09 — MariaDB Service Fix → [`days/day09-mariadb-service-fix.md`](days/day09-mariadb-service-fix.md)  

> I’ll keep adding new days with the same format.

---

## ✍️ How I Document Each Day (5 sections)

1. **Question** — the exact lab prompt (or a tight paraphrase).  
2. **Environment** — where I ran it (hosts/users; secrets redacted as `<PLACEHOLDER>`).  
3. **My Answer (steps & commands)** — what I did and why (copy-pasteable commands).  
4. **Verification** — commands/output that prove it worked.  
5. **Learnings** — 1–3 bullets (gotchas, patterns, tips).

See the template: [`docs/DAY_TEMPLATE.md`](docs/DAY_TEMPLATE.md)

---

## 🚀 Daily Workflow (fast & repeatable)

1. Create `days/dayNN-short-title.md` (NN = 2 digits).  
2. Paste the 5 section headings from the template.  
3. Fill in Question → Environment → My Answer → Verification → Learnings.  
4. Commit with **Conventional Commits**:  
   - Example: `feat(days): add day02 temp user with expiry`

---

## 🧭 Standards (Phase 0)

- **Naming:** `dayNN-short-title.md` (lowercase, hyphen-separated).  
- **Commits:** `feat(days): …`, `docs(readme): …`, `chore(repo): …`.  
- **Redaction:** replace secrets with `<PASSWORD>`, `<TOKEN>`, `<IP-ADDR>`, etc.

More details: [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)

---

## ✅ Progress

Track progress here: [`PROGRESS.md`](PROGRESS.md)

---

## 👤 About

I’m **Akhil Goud Rachakonda**, M.Sc. CS @ BTH (focus: ML/DL + DevOps/MLOps).  
This repo documents my DevOps practice with clear, reproducible notes.

---

## 📄 License

This project is licensed under the **MIT License** — see [`LICENSE`](LICENSE).
