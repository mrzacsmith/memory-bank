# Claude Code Memory File (CLAUDE.md)

This is the primary memory file for Claude Code. It serves as persistent project context that survives between sessions.

## What is CLAUDE.md?

CLAUDE.md is automatically loaded by Claude Code at the start of every conversation. It provides:
- Project-specific instructions and conventions
- Architecture decisions and patterns
- Commands to run (build, test, lint)
- Things Claude should always/never do

## File Locations

| File | Location | Scope | Committed to Git? |
|------|----------|-------|-------------------|
| Project CLAUDE.md | `./CLAUDE.md` or `./.claude/CLAUDE.md` | This project only | Yes |
| User CLAUDE.md | `~/.claude/CLAUDE.md` | All projects | N/A |

## Structure of a Good CLAUDE.md

```markdown
# Project Name

Brief 1-2 sentence description of what this project does.

## Tech Stack
- Framework: Next.js 15 (App Router)
- Language: TypeScript 5.x
- Styling: Tailwind CSS 4.x
- Database: PostgreSQL with Prisma ORM
- Testing: Vitest + Playwright

## Project Structure
/src
  /app          - Next.js app router pages
  /components   - Reusable UI components
  /lib          - Utility functions and helpers
  /services     - Business logic and API calls

## Commands
- `npm run dev` - Start development server
- `npm run build` - Production build
- `npm run test` - Run unit tests
- `npm run lint` - Run ESLint

## Coding Conventions
- Use functional components with hooks
- Prefer named exports over default exports
- Use absolute imports (@/components, @/lib)
- Write tests for all new features

## Architecture Decisions
- Server components by default, client only when needed
- Centralized error handling in /lib/errors
- API routes use standardized response format

## DO NOT
- Never commit .env files
- Never use `any` type without justification
- Never skip error handling
- Never push directly to main branch
```

## Memory Layers (Load Order)

Claude Code loads instructions in this order (later overrides earlier):

1. **Global** (`~/.claude/CLAUDE.md`) - Personal preferences across all projects
2. **Project** (`./CLAUDE.md`) - Team conventions for this project
3. **Local** (`.claude/settings.local.json`) - Your personal settings for this project
4. **Conversation** - Current session context

## Best Practices

### Keep It Focused
- Only include information Claude needs regularly
- Move detailed specs to separate files and reference with `@filename`
- Update when architecture or conventions change

### Be Explicit
- "Prefer X over Y" is better than "use good patterns"
- Include specific file paths when relevant
- List actual commands, not just "run tests"

### Security
- Never put secrets or API keys in CLAUDE.md
- Reference environment variables by name only
- Don't include sensitive business logic

## Example: Full CLAUDE.md

```markdown
# E-Commerce Platform

A Next.js-based e-commerce platform with Stripe integration.

## Tech Stack
- Next.js 15.1 (App Router)
- TypeScript 5.7
- Tailwind CSS 4.0
- Prisma + PostgreSQL
- Stripe for payments
- Clerk for authentication

## Commands
- `pnpm dev` - Development server (port 3000)
- `pnpm build` - Production build
- `pnpm test` - Run Vitest tests
- `pnpm db:push` - Push schema to database
- `pnpm db:studio` - Open Prisma Studio

## Key Patterns
- All API routes return `{ success: boolean, data?: T, error?: string }`
- Use server actions for mutations, not API routes
- Components in /components follow atomic design (atoms, molecules, organisms)
- All prices stored in cents, formatted on display

## Testing Requirements
- Unit tests for all utility functions
- Integration tests for API routes
- E2E tests for checkout flow

## DO NOT
- Use client components for data fetching
- Store prices as floats
- Skip Stripe webhook signature verification
- Commit without running `pnpm lint`
```

## Updating CLAUDE.md

Update your CLAUDE.md when:
1. Adding new technologies or frameworks
2. Changing architectural patterns
3. Adding new coding conventions
4. After major refactors

You can ask Claude: "Update CLAUDE.md with our new authentication approach" and it will help maintain the file.
