# Adopt 'it' Function as Standard Test Case Declaration in React Native Projects: Test Files Placed

These rules are ALWAYS ACTIVE for all React Native component test files, unit tests for React Native modules and utilities, and integration tests within React Native applications using Jest as the test runner.

### Rules

- **R-TEST-001** MUST: Test files MUST be placed in `__tests__` directories and follow the naming pattern `<ComponentName>-test.tsx`.
- **R-TEST-002** MUST: All test case declarations MUST use the `it` function with string descriptors and callback functions.
- **R-TEST-003** MUST: ESLint configuration MUST include `jest/consistent-test-it` rule set to enforce `it` syntax.

### Verify

```bash
# Check for it() declarations in test files
grep -r "^[[:space:]]*it('" template/src/__tests__/ || echo 'No it() declarations found'

# Verify test files follow naming convention and location
find . -path '*/__tests__/*-test.tsx' -type f | head -5

# Verify ESLint jest/consistent-test-it rule is configured
npx eslint --print-config template/src/__tests__/App-test.tsx | grep -A 5 'jest/consistent-test-it' || echo 'Linting rule not configured'
```

**Accept when:**
- All test files in `__tests__` directories use `it` function for test case declarations
- ESLint configuration includes `jest/consistent-test-it` rule enforcing `it` syntax
- Test files follow the naming pattern `<ComponentName>-test.tsx` and are located in `__tests__` directories
- No test files using alternative declaration patterns (`test`, `describe` without `it`) are found in scope

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands MUST pass before accepting changes to test files in scope.
</enforcement>