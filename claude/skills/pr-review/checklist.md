# PR Review Checklist

## Code Quality
- [ ] Variable/function names are descriptive and consistent
- [ ] No magic numbers or strings (use constants)
- [ ] Functions are focused (single responsibility)
- [ ] No unnecessary code duplication
- [ ] Comments explain "why" not "what"
- [ ] Dead code removed
- [ ] Imports are organized and minimal

## Security
- [ ] User input is validated and sanitized
- [ ] No SQL injection vulnerabilities
- [ ] No XSS vulnerabilities
- [ ] Authentication checks are in place
- [ ] Authorization is properly scoped
- [ ] Sensitive data is not logged
- [ ] Secrets are not hardcoded
- [ ] Dependencies are from trusted sources

## Error Handling
- [ ] Errors are caught and handled appropriately
- [ ] Error messages are user-friendly (no stack traces)
- [ ] Errors are logged for debugging
- [ ] Graceful degradation where appropriate

## Testing
- [ ] New code has unit tests
- [ ] Edge cases are tested
- [ ] Tests are meaningful (not just coverage padding)
- [ ] Test names describe the scenario

## Performance
- [ ] No unnecessary database queries
- [ ] Large lists are paginated
- [ ] Expensive operations are cached or optimized
- [ ] No memory leaks introduced

## Documentation
- [ ] Public APIs are documented
- [ ] Complex logic has comments
- [ ] README updated if needed
- [ ] Changelog updated (if applicable)
