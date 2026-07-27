# Adopt Jest 'it' Block Syntax for React Native Test Suites: Template Test Files

These rules are ALWAYS ACTIVE for React Native component test files in template baseline test suites within `__tests__` directories, using TypeScript and TSX test modules with the Jest framework.

### Rules

- **R-JEST-001** SHOULD: Template test files SHOULD provide minimal example implementations that demonstrate syntax without imposing specific assertion patterns.

### Verify

```bash
# Check for 'it' block syntax in template test files
grep -r "it('" template/src/__tests__/ || grep -r 'it("' template/src/__tests__/

# Verify test file naming convention
find . -path '*/__tests__/*-test.tsx' -o -path '*/__tests__/*-test.ts' | head -5

# Confirm Jest and react-native are configured
grep -l 'react-native' package.json && grep -l 'jest' package.json
```

**Accept when:**
- At least one test file in `__tests__` directory uses 'it' block syntax with a string description and callback function
- Jest is configured as a devDependency with react-native preset or equivalent transformation setup
- Test files follow the `-test.tsx` or `-test.ts` naming convention and are discoverable by Jest's default test match patterns

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All three verification commands must pass before accepting the codebase as compliant.
</enforcement>