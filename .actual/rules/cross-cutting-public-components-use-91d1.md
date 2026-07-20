# Standardize React Hooks for UI State Management in Public API Components: Public Components Use

These rules are ALWAYS ACTIVE for all screen components, navigator components, and custom authentication hooks that form the public API surface of React Native applications.

### Rules

- **R-HOOKS-001** MUST: Public API components MUST use React hooks (useState, useEffect, useMemo, useCallback) for local state management and side effects.
- **R-HOOKS-002** MUST: Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data).
- **R-HOOKS-003** MUST: Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders.
- **R-HOOKS-004** MUST: Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests.
- **R-HOOKS-005** MUST: Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation.
- **R-HOOKS-006** MUST: Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties.
- **R-HOOKS-007** SHOULD: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage.
- **R-HOOKS-008** SHOULD: Implement comprehensive error handling in custom authentication hooks; add unit tests covering authentication success, failure, and network error scenarios.

### Verify

```bash
# Check for consistent React hooks usage in public API components
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify StyleSheet.create usage across components
grep -r "StyleSheet.create" template/src/screens/ template/src/components/

# Verify axios HTTP client usage for external API calls
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# TypeScript compilation check for type safety
npx tsc --noEmit --project tsconfig.json

# ESLint verification for hooks rules
npx eslint --plugin react-hooks template/src/screens/ template/src/components/ template/src/navigators/
```

**Accept when:**
- All screen components and navigators use React hooks for state management and side effects
- StyleSheet.create is used consistently across all components with styles
- Axios HTTP client is used for all external API calls with centralized configuration
- TypeScript compilation succeeds without errors for all public API contracts
- ESLint hooks rules pass without violations in CI pipeline
- Navigation parameter interfaces are properly typed and exported as public API contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for public API components. Violations must be caught during code review and CI pipeline checks before merge.
</enforcement>