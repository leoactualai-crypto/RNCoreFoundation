# Standardize React Hooks for UI State Management in Public API Components: External Calls Performed

These rules are ALWAYS ACTIVE for all screen components, navigators, and custom hooks that manage external API integration in React Native applications, particularly those exported as public API contracts (LoginScreen, ChatScreen, AuthStackNavigator) and authentication hooks (useAuth).

### Rules

- **R-EXT-001** MUST: External API calls MUST be performed using axios HTTP client with explicit endpoint configuration from centralized config.
- **R-EXT-002** MUST: Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data).
- **R-EXT-003** MUST: Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders.
- **R-EXT-004** MUST: Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests.
- **R-EXT-005** MUST: Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation.
- **R-EXT-006** MUST: Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties.
- **R-EXT-007** SHOULD: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage.
- **R-EXT-008** SHOULD: Implement comprehensive error handling in authentication hooks; add unit tests covering authentication success, failure, and network error scenarios.

### Verify

```bash
# Verify React hooks usage across screen and component files
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify StyleSheet.create usage consistency
grep -r "StyleSheet.create" template/src/screens/ template/src/components/

# Verify axios HTTP client usage for external API calls
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# Verify TypeScript compilation succeeds
npx tsc --noEmit --project tsconfig.json
```

**Accept when:**
- All screen components and navigators use React hooks for state management and side effects
- StyleSheet.create is used consistently across all components with styles
- Axios HTTP client is used for all external API calls with centralized configuration
- TypeScript compilation succeeds without errors for all public API contracts
- ESLint hooks rules pass without violations in CI pipeline
- Hook dependency arrays are correctly specified and verified by eslint-plugin-react-hooks

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and CI pipeline validation.
</enforcement>