# Project Conventions

## Tech Stack
- Next.js 15 (App Router), TypeScript strict mode, Tailwind CSS v4, shadcn/ui
- pnpm as package manager (NEVER use npm or yarn)
- Biome for linting and formatting (NEVER use ESLint or Prettier separately)
- Vitest for unit tests, Playwright for E2E tests

## Commands
- `pnpm dev` — Start dev server
- `pnpm build` — Production build
- `pnpm test` — Run Vitest
- `pnpm test:e2e` — Run Playwright E2E tests
- `pnpm lint` — Run Biome check
- `pnpm lint:fix` — Run Biome check with auto-fix
- `pnpm format` — Run Biome format
- `pnpm typecheck` — Run tsc --noEmit

## Code Style
- TypeScript strict mode always. No `any` types. Use `unknown` when type is uncertain.
- Use named exports, not default exports (except for Next.js pages/layouts).
- File naming: kebab-case for files, PascalCase for components, camelCase for utilities.
- Prefer `function` declarations for components, arrow functions for callbacks.
- Use `const` by default, `let` only when reassignment is needed, NEVER `var`.

## Project Structure
```
src/
  app/           # Next.js App Router pages and layouts
  components/
    ui/          # shadcn/ui components (do not modify directly)
    shared/      # Reusable project components
    features/    # Feature-specific components
  lib/           # Utility functions, API clients, helpers
  hooks/         # Custom React hooks
  types/         # TypeScript type definitions
  styles/        # Global styles
  __tests__/     # Test files mirroring src/ structure
```

## Architecture Rules
- Server Components by default. Only use "use client" when truly needed.
- Keep components small (<100 lines). Extract logic into custom hooks.
- API routes go in `src/app/api/`. Use Route Handlers.
- All data fetching happens in Server Components or Route Handlers.
- Every route segment should have error.tsx and loading.tsx.

## Quality Standards
- YOU MUST run `pnpm typecheck` after making TypeScript changes.
- YOU MUST run `pnpm lint` after writing new code.
- YOU MUST write at least one test for every new function or component.
- YOU MUST run `pnpm build` before any deployment.
- All images must use next/image. All links must use next/link.
- All forms must have proper aria labels and error states.

## Git Conventions
- Branch naming: `feat/description`, `fix/description`, `refactor/description`
- Commit messages: conventional commits (feat:, fix:, refactor:, test:, docs:, chore:)
- Always create a feature branch. NEVER commit directly to main.
- NEVER add "Co-Authored-By" lines mentioning Claude, Anthropic, or any AI in commit messages.
- NEVER add any AI-related comments, watermarks, or attribution in code or commits.
- Commit author should always appear as the user only.

## Deployment
- Target: Vercel
- Always test with `pnpm build` locally before deploying
- Environment variables go in `.env.local` (NEVER commit .env files)

## Security
- NEVER hardcode API keys, secrets, or credentials
- NEVER commit .env, .env.local, or any file containing secrets
- Validate all user inputs on the server side
- Use parameterized queries for any database operations
