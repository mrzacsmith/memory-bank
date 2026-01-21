---
name: Bug Fix
description: Systematic approach to debugging and fixing issues
triggers:
  - /bug-fix
  - fix bug
  - debug this
  - find the bug
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(npm run test*)
  - Bash(pnpm test*)
  - Bash(yarn test*)
  - Bash(git log*)
  - Bash(git blame*)
---

# Bug Fix Skill

A systematic approach to identifying, debugging, and fixing issues.

## Process

### 1. Reproduce the Bug
Before fixing, confirm you can reproduce it:
- What are the exact steps to trigger the bug?
- What is the expected behavior?
- What is the actual behavior?
- Is it consistent or intermittent?

### 2. Gather Information

```bash
# Check recent changes that might have caused it
git log --oneline -20

# Find who last modified relevant code
git blame src/problematic-file.ts

# Search for related code
grep -r "functionName" src/
```

### 3. Isolate the Problem

Narrow down the scope:
- Which file(s) contain the bug?
- Which function or component?
- What conditions trigger it?

**Debugging techniques:**
- Add logging at key points
- Use breakpoints in debugger
- Write a failing test that reproduces the bug
- Binary search through git history (`git bisect`)

### 4. Write a Failing Test

Before fixing, write a test that fails due to the bug:

```typescript
describe('checkout', () => {
  it('should not duplicate items when clicked rapidly', () => {
    // This test should FAIL before the fix
    const cart = new Cart();

    // Simulate rapid clicks
    cart.addItem('product-1');
    cart.addItem('product-1');

    expect(cart.items).toHaveLength(1);
    expect(cart.items[0].quantity).toBe(2);
  });
});
```

### 5. Implement the Fix

- Fix the root cause, not just the symptom
- Keep the fix minimal and focused
- Don't refactor unrelated code

### 6. Verify the Fix

```bash
# Run the specific test
npm run test -- --grep "should not duplicate"

# Run related tests
npm run test -- src/cart/

# Run full test suite
npm run test
```

### 7. Document and Commit

```bash
git add .
git commit -m "$(cat <<'EOF'
fix(cart): prevent duplicate items on rapid clicks

The add-to-cart handler wasn't debounced, allowing multiple
clicks to create duplicate cart entries before the UI updated.

Added 300ms debounce to the click handler.

Fixes #456
EOF
)"
```

## Common Bug Categories

### Race Conditions
- Symptoms: Intermittent failures, timing-dependent
- Fix: Add locks, debounce, or proper async handling

### Null/Undefined Errors
- Symptoms: "Cannot read property of undefined"
- Fix: Add null checks, optional chaining, default values

### Off-by-One Errors
- Symptoms: Array index out of bounds, wrong loop iterations
- Fix: Check boundary conditions, use `.length - 1` carefully

### State Management
- Symptoms: Stale data, unexpected values
- Fix: Ensure proper state updates, check mutation vs immutability

### Memory Leaks
- Symptoms: Growing memory usage, slowdowns over time
- Fix: Clean up subscriptions, event listeners, timers

## Git Bisect (Finding When Bug Was Introduced)

```bash
# Start bisect
git bisect start

# Mark current as bad
git bisect bad

# Mark known good commit
git bisect good abc123

# Git will checkout commits for you to test
# Mark each as good or bad
git bisect good  # or git bisect bad

# When found, git shows the first bad commit
# End bisect
git bisect reset
```
