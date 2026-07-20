# Adopt 'it' Function as Standard Test Case Declaration in React Native Projects: Test Cases React

These rules are ALWAYS ACTIVE for all React Native component test files, unit tests for React Native modules and utilities, integration tests within React Native applications, and test files using Jest as the test runner.

### Rules

- **R-REACT-TEST-001** MUST: Test cases in React Native projects MUST be declared using the 'it' function with a descriptive string label and callback function.

### Verify

```bash
# Check for it() declarations in test files
grep -r "^[[:space:]]*it('" template/src/__tests__/ || echo 'No it() declarations found'

# Verify test file naming convention
find . -path '*/__tests__/*-test.tsx' -type f | head -5

# Check ESLint configuration for jest/consistent-test-it rule
npx eslint --print-config template/src/__tests__/App-test.tsx | grep -A 5 'jest/consistent-test-it' || echo 'Linting rule not configured'
```

**Accept when:**
- All test files in `__tests__` directories use 'it' function for test case declarations
- ESLint configuration includes jest/consistent-test-it rule enforcing 'it' syntax
- Test files follow the naming pattern `<ComponentName>-test.tsx` and are located in `__tests__` directories
- No test files use alternative declaration patterns like 'test' or 'describe' for individual test cases

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations related to test declaration syntax MUST cause CI pipeline failure. Code review MUST verify test file structure compliance. Violations require actionable error messages and may be escalated to technical lead for exception approval.
</enforcement>