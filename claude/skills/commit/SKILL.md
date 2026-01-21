---
name: Commit
description: Create well-formatted commits following conventional commit standards
triggers:
  - /commit
  - commit changes
  - commit this
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash(git status*)
  - Bash(git diff*)
  - Bash(git add*)
  - Bash(git commit*)
  - Bash(git log*)
---

# Commit Skill

Create clean, well-documented commits following conventional commit standards.

## Process

### 1. Review Current Changes
```bash
git status
git diff
git diff --staged
```

Before committing:
- Understand the scope of changes
- Identify if changes should be split into multiple commits
- Ensure no unintended files are staged

### 2. Stage Changes
Stage related changes together. Split unrelated changes into separate commits.

```bash
# Stage specific files
git add src/feature.ts src/feature.test.ts

# Stage all changes in a directory
git add src/components/
```

### 3. Write Commit Message

Follow **Conventional Commits** format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

#### Commit Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Formatting (no code change) |
| `refactor` | Code restructuring |
| `perf` | Performance improvement |
| `test` | Adding/fixing tests |
| `chore` | Maintenance tasks |

#### Examples

**Feature:**
```
feat(auth): add password reset flow

- Add forgot password form
- Implement email service integration
- Add token validation endpoint

Closes #123
```

**Bug Fix:**
```
fix(cart): prevent duplicate items on rapid clicks

Added debounce to add-to-cart button to prevent
race condition causing duplicate entries.

Fixes #456
```

**Refactor:**
```
refactor(utils): consolidate date formatting functions

Merged 4 duplicate implementations into shared utility.
No behavior changes.
```

### 4. Commit

```bash
git commit -m "type(scope): subject"
```

For multiline messages:
```bash
git commit -m "$(cat <<'EOF'
feat(payments): add Stripe integration

- Configure Stripe client
- Add checkout session creation
- Implement webhook handlers

Closes #789
EOF
)"
```

## Pre-Commit Checklist

- [ ] Code builds without errors
- [ ] Tests pass
- [ ] No debug code (console.log, debugger)
- [ ] No secrets or API keys
- [ ] Commit message follows conventions

## Atomic Commits

Each commit should be:
- **Self-contained**: One logical change
- **Complete**: Doesn't break the build
- **Reversible**: Can be reverted independently
