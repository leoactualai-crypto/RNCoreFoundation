# Adopt 'it' Function as Standard Test Case Declaration in React Native Projects: Test Case Descriptions

These rules are ALWAYS ACTIVE for all React Native component test files, unit tests for React Native modules and utilities, integration tests within React Native applications, and test files using Jest as the test runner.

### Rules

- **R-TEST-001** SHOULD: Test case descriptions SHOULD clearly describe the expected behavior being validated.

### Verify

```bash
# Verify 'it' function usage in test files
grep -r "^[[:space:]]*it('" template/src/__tests__/ || echo 'No it() declarations found'

# Verify test file naming convention
find . -path '*/__tests__/*-test.tsx' -type f | head -5

# Verify ESLint jest/consistent-test-it rule configuration
npx eslint --print-config template/src/__tests__/App-test.tsx | grep -A 5 'jest/consistent-test-it' || echo 'Linting rule not configured'
```

**Accept when:**
- All test files in `__tests__` directories use 'it' function for test case declarations
- ESLint configuration includes jest/consistent-test-it rule enforcing 'it' syntax
- Test files follow the naming pattern `<ComponentName>-test.tsx` and are located in `__tests__` directories
- Test case descriptions clearly articulate expected behavior in human-readable language

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations related to test declaration syntax MUST cause CI pipeline failure. Code review feedback MUST request changes to non-compliant test declarations. Exceptions require technical lead approval and documentation in test file comments.
</enforcement>