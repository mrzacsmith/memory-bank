---
name: Create PR
description: Create well-documented pull requests with proper descriptions
triggers:
  - /create-pr
  - create pr
  - open pr
  - make pull request
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash(git status*)
  - Bash(git diff*)
  - Bash(git log*)
  - Bash(git push*)
  - Bash(git branch*)
  - Bash(gh pr create*)
  - Bash(gh pr view*)
---

# Create PR Skill

Create well-documented pull requests that are easy to review and merge.

## Process

### 1. Verify Branch State

```bash
# Check current branch
git branch --show-current

# Ensure all changes are committed
git status

# View commits that will be in the PR
git log main..HEAD --oneline
```

### 2. Push to Remote

```bash
# Push and set upstream
git push -u origin feature/my-feature
```

### 3. Review Changes

Before creating the PR, understand what you're submitting:

```bash
# See all changes vs main branch
git diff main...HEAD

# List changed files
git diff main...HEAD --name-only

# View commit history
git log main..HEAD
```

### 4. Create the Pull Request

```bash
gh pr create --title "feat: add user authentication" --body "$(cat <<'EOF'
## Summary

Brief description of what this PR does and why.

- Add login/logout functionality
- Implement JWT token handling
- Add protected route middleware

## Changes

- `src/auth/` - New authentication module
- `src/middleware/` - Auth middleware for protected routes
- `src/pages/login.tsx` - Login page component

## Testing

- [ ] Unit tests pass
- [ ] Manual testing of login flow
- [ ] Tested logout clears session

## Screenshots

(If UI changes, add screenshots here)

## Related Issues

Closes #123
EOF
)"
```

## PR Description Template

```markdown
## Summary

[One paragraph explaining the change and motivation]

## Changes

- [List key files/components changed]
- [Explain non-obvious implementation decisions]

## Testing

- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist

- [ ] Code follows project conventions
- [ ] Documentation updated (if needed)
- [ ] No console.log or debug code
- [ ] Migrations included (if DB changes)

## Screenshots

[For UI changes, before/after screenshots]

## Related Issues

Closes #[issue-number]
```

## Best Practices

### PR Size
- Keep PRs small and focused (< 400 lines ideally)
- Split large features into multiple PRs
- One logical change per PR

### Title Format
Follow conventional commit style:
- `feat: add user dashboard`
- `fix: resolve cart duplication bug`
- `refactor: extract payment logic to service`

### Description Quality
- Explain WHY, not just WHAT
- Link to related issues/discussions
- Include testing instructions
- Add screenshots for UI changes

### Before Requesting Review
- Self-review your own diff first
- Ensure CI passes
- Resolve any merge conflicts
- Remove WIP commits (squash if needed)

## Useful gh Commands

```bash
# Create PR with specific base branch
gh pr create --base develop

# Create draft PR
gh pr create --draft

# View PR status
gh pr view

# List open PRs
gh pr list

# Check PR checks status
gh pr checks
```
