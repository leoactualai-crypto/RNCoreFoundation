# Adopt Jest 'it' Block Syntax for React Native Test Suites: Test Descriptions Passed

These rules are ALWAYS ACTIVE for React Native component test files, template baseline test suites in `__tests__` directories, and TypeScript/TSX test modules using Jest framework.

### Rules

- **R-JEST-001** SHOULD: Test descriptions passed to 'it' blocks SHOULD use lowercase strings describing the expected behavior.

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
- Test descriptions use lowercase strings that describe expected behavior

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. Violations should be flagged as CI build warnings for non-compliant test file naming patterns and linter errors for non-standard 'it' block syntax.
</enforcement>