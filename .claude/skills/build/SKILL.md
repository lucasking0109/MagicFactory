---
name: build
description: Execute an implementation plan step by step with quality checks at each stage. Reads the plan document and builds the feature.
argument-hint: "[plan-file-path or feature-name]"
---

# Build Implementation

You are implementing a feature based on an existing plan. ultrathink

## Step 1: Load the Plan
Read the plan document at $ARGUMENTS (or find the most recent plan in docs/plans/).
If no plan exists, tell the user to run /plan first.

## Step 2: Create Feature Branch
```bash
git checkout -b feat/[feature-name]
```

## Step 3: Implement Step by Step
Follow the plan's Implementation Order. For EACH step:

1. **Announce** what you are about to build (one line)
2. **Implement** the code following CLAUDE.md conventions
3. **Verify** — run checks after each step:
   - `pnpm typecheck` after TypeScript changes
   - `pnpm lint` after new code
   - `pnpm test` to run the test suite
4. **Commit** the step: `git commit -m "feat: [what was done]"`
5. **Report** status: what succeeded, what needs attention

If a step fails (test failure, type error, lint error), fix it BEFORE moving to the next step.

## Step 4: Integration Verification
After all steps are complete:
1. `pnpm build` — verify production build works
2. `pnpm test` — verify all tests pass
3. `pnpm lint` — verify no lint issues

## Step 5: Summary
Report to the user:
- What was built (list of files created/modified)
- Test results
- Build status
- Any issues encountered and how they were resolved
- Suggest running /review before deploying
