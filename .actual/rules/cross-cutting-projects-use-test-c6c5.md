# Adopt Jest 'it' Block Syntax for React Native Test Suites: Projects Use Test

These rules are ALWAYS ACTIVE for React Native component test files, template baseline test suites in `__tests__` directories, and TypeScript/TSX test modules using the Jest framework.

### Rules

- **R-JEST-001** MUST: Use 'it' block syntax for test definitions in React Native test suites, following BDD-style conventions with string descriptions and callback functions.
- **R-JEST-002** MAY: Projects MAY use 'test' as an alias for 'it' where team conventions prefer explicit test nomenclature.
- **R-JEST-003** MUST: Structure test files to mirror source directory hierarchy (e.g., `src/components/Button.tsx` → `src/__tests__/Button-test.tsx`) for discoverability.
- **R-JEST-004** MUST: Follow the `-test.tsx` or `-test.ts` naming convention for test files to ensure Jest's default test match patterns discover them.
- **R-JEST-005** SHOULD: Include at least one meaningful assertion per 'it' block; avoid empty test bodies except in template examples explicitly marked as placeholders.
- **R-JEST-006** SHOULD: Configure Jest in `package.json` or `jest.config.js` with `preset: 'react-native'` to ensure proper TypeScript and TSX transformation.
- **R-JEST-007** SHOULD: Use `@testing-library/react-native` for component rendering and queries to provide better accessibility-focused selectors.

### Verify

```bash
# Check for 'it' block syntax in template test files
grep -r "it('" template/src/__tests__/ || grep -r 'it("' template/src/__tests__/

# Find test files following the naming convention
find . -path '*/__tests__/*-test.tsx' -o -path '*/__tests__/*-test.ts' | head -5

# Verify Jest and React Native are configured
grep -l 'react-native' package.json && grep -l 'jest' package.json
```

**Accept when:**
- At least one test file in `__tests__` directory uses 'it' block syntax with a string description and callback function
- Jest is configured as a devDependency with react-native preset or equivalent transformation setup
- Test files follow the `-test.tsx` or `-test.ts` naming convention and are discoverable by Jest's default test match patterns
- Jest can discover and run tests in `__tests__` directories via CI pipeline test execution

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All test files in scope MUST be validated against R-JEST-001 through R-JEST-007 before accepting changes to React Native test suites.
</enforcement>