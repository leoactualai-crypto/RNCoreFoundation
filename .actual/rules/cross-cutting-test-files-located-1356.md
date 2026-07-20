# Adopt 'it' Function as Standard Test Case Declaration in React Native Projects: Test Files Located

These rules are ALWAYS ACTIVE for all React Native component test files, unit tests for React Native modules and utilities, and integration tests within React Native applications using Jest as the test runner.

### Rules

- **R-TEST-001** SHOULD: Test files SHOULD be co-located with the components they test, using the `__tests__` directory convention.
- **R-TEST-002** SHOULD: Test files SHOULD follow the naming pattern `<ComponentName>-test.tsx` when located in `__tests__` directories.
- **R-TEST-003** SHOULD: Test case declarations SHOULD use the 'it' function for consistency with Jest and React Native ecosystem conventions.
- **R-TEST-004** SHOULD: Test descriptions SHOULD be clear and descriptive to improve test output readability and debugging efficiency.

### Verify

```bash
# Check for 'it()' declarations in test files
grep -r "^[[:space:]]*it('" template/src/__tests__/ || echo 'No it() declarations found'

# Verify test file naming convention
find . -path '*/__tests__/*-test.tsx' -type f | head -5

# Check ESLint configuration for jest/consistent-test-it rule
npx eslint --print-config template/src/__tests__/App-test.tsx | grep -A 5 'jest/consistent-test-it' || echo 'Linting rule not configured'
```

**Accept when:**
- All test files in `__tests__` directories use 'it' function for test case declarations
- ESLint configuration includes `jest/consistent-test-it` rule enforcing 'it' syntax
- Test files follow the naming pattern `<ComponentName>-test.tsx` and are located in `__tests__` directories
- CI pipeline passes ESLint validation for test declaration syntax

<enforcement>
Claude Code MUST NOT skip or defer verification of test file structure and 'it' function usage. Violations detected by ESLint or naming convention checks MUST be flagged and require remediation before acceptance.
</enforcement>