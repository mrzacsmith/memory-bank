---
name: TDD Workflow
description: Test-Driven Development workflow - write tests first, then implement
triggers:
  - tdd
  - test first
  - write test first
  - test driven
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(npm run test*)
  - Bash(pnpm test*)
  - Bash(yarn test*)
  - Bash(npx jest*)
  - Bash(npx vitest*)
---

# TDD Workflow Skill

Implement features using strict Test-Driven Development methodology.

## The Red-Green-Refactor Cycle

### 1. RED - Write a Failing Test
```
1. Understand the requirement
2. Write a test that describes the expected behavior
3. Run the test - confirm it FAILS
4. Commit: "test: add failing test for [feature]"
```

### 2. GREEN - Make It Pass
```
1. Write the MINIMUM code to make the test pass
2. No extra features, no "while I'm here" changes
3. Run the test - confirm it PASSES
4. Commit: "feat: implement [feature]"
```

### 3. REFACTOR - Improve the Code
```
1. Clean up the implementation
2. Remove duplication
3. Improve naming
4. Run tests - confirm they still PASS
5. Commit: "refactor: clean up [feature]"
```

## Test Quality Rules

### Good Tests
- Test behavior, not implementation
- One assertion per test (when practical)
- Descriptive test names: "should [expected behavior] when [condition]"
- Independent - no shared state between tests
- Fast - mock external dependencies

### Bad Tests
- Testing implementation details
- Multiple unrelated assertions
- Flaky tests that sometimes fail
- Tests that require specific order

## Test Structure (Arrange-Act-Assert)

```typescript
describe('calculateTotal', () => {
  it('should apply discount when coupon is valid', () => {
    // Arrange
    const cart = { items: [{ price: 100 }] };
    const coupon = { discount: 0.1 };

    // Act
    const total = calculateTotal(cart, coupon);

    // Assert
    expect(total).toBe(90);
  });
});
```

## When to Mock

**DO Mock:**
- External APIs
- Database calls
- File system operations
- Time-dependent functions
- Third-party services

**DON'T Mock:**
- The code you're testing
- Simple utility functions
- Internal collaborators (usually)

## TDD Commands

Before implementing any feature:
1. Ask: "What test would prove this works?"
2. Write that test
3. Watch it fail
4. Implement just enough
5. Watch it pass
6. Refactor if needed

## Integration with CI

Ensure all tests pass before:
- Committing code
- Creating PRs
- Merging to main
