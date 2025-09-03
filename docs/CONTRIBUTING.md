# Repo Standards

## File naming
- One day = one file under `days/`
- Format: `dayNN-short-title.md` (NN is two digits)

## Required sections (in order)
1) Question
2) Environment
3) My Answer (steps & commands)
4) Verification
5) Learnings

## Redaction
- Replace sensitive values with placeholders: <PASSWORD>, <TOKEN>, <IP-ADDR>, etc.

## Commit messages (Conventional Commits)
- Daily content: `feat(days): add dayNN short-title`
- Docs: `docs(readme): ...` or `docs(template): ...`
- Chores: `chore(repo): ...`

## Branching
- Default: commit daily entries to `main`.
- Use short-lived branches only for larger changes (README overhaul, scripts).
