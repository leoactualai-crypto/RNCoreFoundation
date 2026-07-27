# Adopt Jest 'it' Block Syntax for React Native Test Suites: Test Files Placed

These rules are ALWAYS ACTIVE for React Native component test files and template baseline test suites using Jest framework with TypeScript/TSX modules.

### Rules

- **R-JEST-001** MUST: Test files MUST be placed in `__tests__` directories with the `-test.tsx` or `-test.ts` suffix pattern.
- **R-JEST-002** MUST: Test files MUST use Jest 'it' block syntax with a string description and callback function.
- **R-JEST-003** MUST: Jest MUST be configured with `preset: 'react-native'` in package.json or jest.config.js for proper TypeScript and TSX transformation.
- **R-JEST-004** SHOULD: Test file directory structure SHOULD mirror source directory hierarchy (e.g., `src/components/Button.tsx` → `src/__tests__/Button-test.tsx`) for discoverability.
- **R-JEST-005** SHOULD: Each 'it' block SHOULD include at least one meaningful assertion; avoid empty test bodies except in template examples explicitly marked as placeholders.
- **R-JEST-006** MAY: Use @testing-library/react-native for component rendering and queries to provide better accessibility-focused selectors.

### Verify

```bash
# Check for 'it' block syntax in __tests__ directories
grep -r "it('" __tests__/ || grep -r 'it("' __tests__/

# Find test files following -test.tsx or -test.ts naming convention
find . -path '*/__tests__/*-test.tsx' -o -path '*/__tests__/*-test.ts' | head -5

# Verify Jest and react-native are configured
grep -l 'react-native' package.json && grep -l 'jest' package.json

# Verify Jest configuration includes react-native preset
grep -E '"preset".*react-native|preset.*react-native' jest.config.js package.json
```

**Accept when:**
- At least one test file in `__tests__` directory uses 'it' block syntax with a string description and callback function
- Jest is configured as a devDependency with react-native preset or equivalent transformation setup
- Test files follow the `-test.tsx` or `-test.ts` naming convention and are discoverable by Jest's default test match patterns
- Test files are located in `__tests__` directories, not co-located with source files
- Jest can successfully discover and execute tests in the configured `__tests__` directories

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native test suites. Violations MUST be flagged as CI build warnings, linter errors, or pull request comments from automated tooling.
</enforcement>