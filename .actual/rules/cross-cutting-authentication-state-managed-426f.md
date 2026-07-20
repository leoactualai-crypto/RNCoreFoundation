# Standardize React Hooks for UI State Management in Public API Components: Authentication State Managed

These rules are ALWAYS ACTIVE for all screen components, navigators, and custom hooks that manage authentication state and UI interactions in React Native applications exposing public API contracts.

### Rules

- **R-AUTH-001** MUST: Authentication state MUST be managed through custom hooks (useAuth) that encapsulate authentication logic and external API integration.
- **R-AUTH-002** MUST: Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data).
- **R-AUTH-003** MUST: Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests.
- **R-AUTH-004** MUST: Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation.
- **R-AUTH-005** MUST: Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties.
- **R-AUTH-006** SHOULD: Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders.
- **R-AUTH-007** SHOULD: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage.

### Verify

```bash
# Verify React hooks usage across screen and component files
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify StyleSheet.create usage
grep -r "StyleSheet.create" template/src/screens/ template/src/components/

# Verify axios HTTP client usage
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# Verify TypeScript compilation
npx tsc --noEmit --project tsconfig.json
```

**Accept when:**
- All screen components and navigators use React hooks for state management and side effects
- StyleSheet.create is used consistently across all components with styles
- Axios HTTP client is used for all external API calls with centralized configuration
- TypeScript compilation succeeds without errors for all public API contracts
- ESLint with eslint-plugin-react-hooks passes without violations in CI pipeline
- Authentication tokens are managed through react-native-secure-storage in the useAuth hook
- All navigation parameter lists have TypeScript interfaces defined and exported

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication state management in public API components. Violations must be caught during code review and CI pipeline checks before merge.
</enforcement>