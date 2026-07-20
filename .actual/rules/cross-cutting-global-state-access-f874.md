# Standardize React Hooks for UI State Management in Public API Components: Global State Access

These rules are ALWAYS ACTIVE for all screen components, navigator components, and custom authentication hooks exported as public API contracts in React Native applications using react-redux and react-navigation.

### Rules

- **R-HOOKS-001** SHOULD: Global state access SHOULD use useSelector hook from react-redux for consistency across navigation and screen components.
- **R-HOOKS-002** MUST: Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data).
- **R-HOOKS-003** SHOULD: Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders.
- **R-HOOKS-004** MUST: Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests.
- **R-HOOKS-005** MUST: Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation.
- **R-HOOKS-006** MUST: Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties.
- **R-HOOKS-007** MUST: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage in all hook implementations.

### Verify

```bash
# Verify React hooks usage across screen and navigator components
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify StyleSheet.create usage consistency
grep -r "StyleSheet.create" template/src/screens/ template/src/components/

# Verify axios HTTP client usage with centralized configuration
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# Verify TypeScript compilation succeeds
npx tsc --noEmit --project tsconfig.json

# Verify ESLint hooks rules are enforced
npx eslint --plugin react-hooks template/src/screens/ template/src/components/ template/src/navigators/
```

**Accept when:**
- All screen components and navigators use React hooks for state management and side effects
- StyleSheet.create is used consistently across all components with styles
- Axios HTTP client is used for all external API calls with centralized configuration
- TypeScript compilation succeeds without errors for all public API contracts
- ESLint hooks rules pass without violations in CI pipeline
- All hook dependency arrays are correctly specified and verified by eslint-plugin-react-hooks

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for public API components and must be verified before code review approval.
</enforcement>