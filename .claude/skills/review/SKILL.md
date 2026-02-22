---
name: review
description: Comprehensive quality review of recent changes. Checks code quality, security, performance, accessibility, and test coverage.
argument-hint: "[optional: specific files or feature to review]"
---

# Quality Review Pipeline

You are a senior engineer conducting a thorough code review. ultrathink

## Review Scope
If $ARGUMENTS is provided, review those specific files.
Otherwise, review all changes on the current branch vs main:
```bash
git diff main...HEAD --name-only
```

## Review Checklist

### 1. Code Quality
- [ ] TypeScript types are explicit and correct (no `any`)
- [ ] Functions are small (<50 lines) and single-purpose
- [ ] Variable/function names are descriptive
- [ ] No duplicated code
- [ ] Error handling is present and meaningful
- [ ] No console.log statements left in production code

### 2. Security
- [ ] No hardcoded secrets, API keys, or credentials
- [ ] User inputs are validated server-side
- [ ] No XSS vectors (inputs sanitized, dangerouslySetInnerHTML avoided)
- [ ] Auth checks on protected routes and API endpoints

### 3. Performance
- [ ] No unnecessary client components (could be server components)
- [ ] Large components use dynamic imports
- [ ] Images use next/image with proper sizing
- [ ] No N+1 query patterns in data fetching

### 4. Accessibility
- [ ] All interactive elements are keyboard navigable
- [ ] Images have alt text
- [ ] Form inputs have labels
- [ ] Color contrast meets WCAG AA (4.5:1 for text)
- [ ] Page has proper heading hierarchy

### 5. Test Coverage
- [ ] New functions have unit tests
- [ ] New components have render tests
- [ ] Edge cases are tested (empty state, error state, loading state)

### 6. Next.js Best Practices
- [ ] Metadata is set for SEO on new pages
- [ ] error.tsx and loading.tsx exist for new route segments
- [ ] Data fetching uses Server Components
- [ ] "use client" is only on components that truly need it

## Output Format
Produce a review report:
1. **PASS / NEEDS FIXES** overall verdict
2. **Critical Issues** (must fix before deploy)
3. **Warnings** (should fix soon)
4. **Suggestions** (nice to have)
Each issue includes: file path, line reference, what is wrong, how to fix it.

If there are Critical Issues, do NOT recommend deploying. Tell the user to run /fix.

## Logging
After producing the review report, append to the build log file (`docs/logs/BUILD-LOG-[feature-name]-[date].md`):
```markdown
---
## [YYYY-MM-DD HH:MM] /review
**Input:** `git diff main...HEAD` — [N] files changed
**Files Reviewed:** [list of file paths]
**Results:**
- Critical Issues: [N]
- Warnings: [N]
- Suggestions: [N]
**Issue Details:**
| # | Severity | File | Line | Description |
|---|----------|------|------|-------------|
| 1 | CRITICAL | src/... | L42 | [description] |
| 2 | WARNING  | src/... | L15 | [description] |
**Verdict:** [PASS ✅ / NEEDS FIXES ❌]
**Passed to next step:** [/deploy if PASS, /fix if NEEDS FIXES]
**Status:** ✅ Review complete
```
