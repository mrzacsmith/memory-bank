---
name: Refactor
description: Safe code refactoring with proper testing and incremental changes
triggers:
  - /refactor
  - refactor this
  - clean up code
  - restructure
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(npm run test*)
  - Bash(pnpm test*)
  - Bash(yarn test*)
  - Bash(npm run lint*)
  - Bash(git diff*)
  - Bash(git status*)
---

# Refactor Skill

Safely restructure code without changing behavior.

## Golden Rule

> Refactoring changes HOW code works, not WHAT it does.

All tests should pass before AND after refactoring.

## Process

### 1. Ensure Test Coverage

Before refactoring, verify tests exist:

```bash
# Run existing tests
npm run test

# Check coverage for the code you're refactoring
npm run test -- --coverage src/module-to-refactor/
```

If coverage is low, **add tests first** before refactoring.

### 2. Make Small, Incremental Changes

Each step should be:
- A single, focused change
- Independently committable
- Passing all tests

**Bad:** One massive commit that changes everything
**Good:** Series of small commits, each improving the code

### 3. Run Tests After Each Change

```bash
# Quick feedback loop
npm run test -- --watch src/module/
```

### 4. Commit Frequently

```bash
git commit -m "refactor(auth): extract token validation to separate function"
git commit -m "refactor(auth): rename validateToken to verifyJWT"
git commit -m "refactor(auth): move JWT logic to dedicated service"
```

## Common Refactoring Patterns

### Extract Function
**When:** Code block does one specific thing

```typescript
// Before
function processOrder(order) {
  // 20 lines calculating total
  // 15 lines validating inventory
  // 10 lines sending notifications
}

// After
function processOrder(order) {
  const total = calculateTotal(order);
  validateInventory(order.items);
  sendOrderNotifications(order);
}
```

### Extract Module/Class
**When:** Related functions should be grouped

```
// Before: utils.ts with 50 functions

// After:
// date-utils.ts - date formatting functions
// string-utils.ts - string manipulation
// validation-utils.ts - validators
```

### Rename for Clarity
**When:** Names don't describe purpose

```typescript
// Before
const d = new Date();
function proc(x) { ... }

// After
const createdAt = new Date();
function processPayment(transaction) { ... }
```

### Replace Conditionals with Polymorphism
**When:** Switch/if chains based on type

```typescript
// Before
function calculateArea(shape) {
  if (shape.type === 'circle') return Math.PI * shape.radius ** 2;
  if (shape.type === 'rectangle') return shape.width * shape.height;
}

// After
class Circle {
  calculateArea() { return Math.PI * this.radius ** 2; }
}
class Rectangle {
  calculateArea() { return this.width * this.height; }
}
```

### Remove Duplication (DRY)
**When:** Same code appears in multiple places

```typescript
// Before: Same validation in 3 files

// After: Shared validation utility
import { validateEmail } from '@/utils/validation';
```

### Simplify Conditionals
**When:** Complex boolean logic

```typescript
// Before
if (user && user.subscription && user.subscription.active && !user.banned) {
  // ...
}

// After
function canAccessPremiumContent(user) {
  if (!user) return false;
  if (user.banned) return false;
  return user.subscription?.active ?? false;
}

if (canAccessPremiumContent(user)) {
  // ...
}
```

## Refactoring Checklist

- [ ] Tests pass before starting
- [ ] Identified specific improvements to make
- [ ] Making one change at a time
- [ ] Running tests after each change
- [ ] Committing after each successful change
- [ ] Not adding new features (refactor only)
- [ ] Not fixing bugs (separate concern)

## Warning Signs to Stop

- Tests start failing unexpectedly
- Scope is growing beyond original plan
- You're tempted to "also fix this bug"
- Changes are getting hard to explain

When in doubt, commit what you have and reassess.

## Code Smells to Look For

| Smell | Refactoring |
|-------|-------------|
| Long function | Extract functions |
| Long parameter list | Introduce parameter object |
| Duplicate code | Extract shared function |
| Large class | Split into focused classes |
| Feature envy | Move method to appropriate class |
| Primitive obsession | Create domain types |
| Deep nesting | Extract functions, early returns |
