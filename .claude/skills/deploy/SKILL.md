---
name: deploy
description: Deploy the current project to Vercel. Runs pre-deploy checks, pushes to GitHub, and triggers deployment.
argument-hint: "[optional: preview|production]"
---

# Deploy to Vercel

## Step 1: Pre-Deploy Checks
Run ALL of these. If ANY fail, stop and report the issue:
1. `pnpm typecheck` — must pass
2. `pnpm lint` — must pass
3. `pnpm test` — must pass
4. `pnpm build` — must succeed

If any check fails, report the failure and suggest running /fix.

## Step 2: Git Status
Check `git status`:
- If there are uncommitted changes, commit them first
- If on main, warn the user and suggest creating a PR instead
- If on a feature branch, proceed

## Step 3: Push to GitHub
```bash
git push -u origin $(git branch --show-current)
```

If no GitHub remote exists:
1. Ask the user for the repository name
2. `gh repo create [name] --private --source=. --push`

## Step 4: Deploy
Based on the argument (default: preview):

**Preview deployment:**
```bash
vercel
```

**Production deployment:**
```bash
vercel --prod
```

## Step 5: Report
Provide:
- Deployment URL
- Any warnings from the build
- If on feature branch, suggest creating a PR:
  ```bash
  gh pr create --title "feat: [description]" --body "[summary]"
  ```

## Logging
After deployment completes (or fails), append to the build log file (`docs/logs/BUILD-LOG-[feature-name]-[date].md`):
```markdown
---
## [YYYY-MM-DD HH:MM] /deploy
**Input:** Branch `[branch-name]`, commit `[hash]`
**Pre-Deploy Checks:**
- typecheck: [✅/❌]
- lint: [✅/❌]
- test: [✅/❌]
- build: [✅/❌]
**Deployment:**
- Type: [preview / production]
- Platform: Vercel
- URL: [deployment URL]
- Build warnings: [list or "None"]
**Git:**
- Pushed to: `origin/[branch]`
- PR: [PR URL if created, or "N/A"]
**Status:** [✅ deployed / ❌ failed: reason]

---
## 🏁 Pipeline Complete
**Total Duration:** /plan → /build → /review → /fix → /deploy
**Final URL:** [deployment URL]
```
