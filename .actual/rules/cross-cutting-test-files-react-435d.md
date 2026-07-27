# Adopt Jest 'it' Block Syntax for React Native Test Suites: Test Files React

These rules are ALWAYS ACTIVE for React Native component test files using Jest framework, specifically test files in `__tests__` directories with `-test.tsx` or `-test.ts` naming conventions.

### Rules

- **R-JEST-RN-001** MUST: Test files in React Native projects MUST use the 'it' block syntax for defining individual test cases.

### Verify

```bash
# Check for 'it' block syntax in template test files
grep -r "it('" template/src/__tests__/ || grep -r 'it("' template/src/__tests__/

# Find test files following the naming convention
find . -path '*/__tests__/*-test.tsx' -o -path '*/__tests__/*-test.ts' | head -5

# Verify Jest and react-native are configured
grep -l 'react-native' package.json && grep -l 'jest' package.json
```

**Accept when:**
- At least one test file in `__tests__` directory uses 'it' block syntax with a string description and callback function
- Jest is configured as a devDependency with react-native preset or equivalent transformation setup
- Test files follow the `-test.tsx` or `-test.ts` naming convention and are discoverable by Jest's default test match patterns

<enforcement>
Claude Code MUST NOT skip or defer verification of this rule. All React Native test files must be inspected to confirm 'it' block syntax compliance before accepting pull requests.
</enforcement>