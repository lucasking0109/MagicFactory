---
name: fix
description: Debug and fix issues in the project. Handles build errors, test failures, runtime bugs, and review findings.
argument-hint: "[error description or 'review' to fix review findings]"
---

# Debug and Fix

## Step 1: Identify the Problem
If $ARGUMENTS is "review", read the most recent review output.
Otherwise, analyze the error description: $ARGUMENTS

If no argument provided:
1. Run `pnpm typecheck` and capture errors
2. Run `pnpm lint` and capture errors
3. Run `pnpm test` and capture failures
4. Run `pnpm build` and capture errors
Report all issues found.

## Step 2: Diagnose
For each issue:
1. Read the relevant file(s)
2. Understand the root cause (not just the symptom)
3. Check if the issue exists in other similar files (pattern problem)

## Step 3: Fix
For each issue:
1. Implement the fix
2. Run the relevant check to verify the fix works
3. If the fix introduces new issues, address them immediately
4. Commit the fix: `git commit -m "fix: [description]"`

## Step 4: Verify All Clear
After all fixes:
1. `pnpm typecheck` — must pass
2. `pnpm lint` — must pass
3. `pnpm test` — must pass
4. `pnpm build` — must succeed

Report results. If all pass, suggest running /deploy.

## Logging
After each fix and after final verification, append to the build log file (`docs/logs/BUILD-LOG-[feature-name]-[date].md`):

### Per-Fix Entry
```markdown
---
## [YYYY-MM-DD HH:MM] /fix — Issue [N]: [short description]
**Input:** [Error description or review issue reference]
**Root Cause:** [What was actually wrong and why]
**Fix Applied:**
- File: `[file-path]` Line [N]
- Change: [What was changed]
**Verification:**
- typecheck: [✅/❌]
- lint: [✅/❌]
- test: [✅/❌]
**Git:** `fix: [description]` ([commit hash])
**Status:** ✅ Fixed
```

### Final Verification Log
```markdown
---
## [YYYY-MM-DD HH:MM] /fix — Final Verification
**Total Issues Fixed:** [N]
**Final Checks:**
- typecheck: [✅/❌]
- lint: [✅/❌]
- test: [✅/❌]
- build: [✅/❌]
**Passed to next step:** Ready for /deploy
**Status:** [✅ all clear / ❌ issues remain]
```
