# Standardize React Hooks for UI State Management in Public API Components: Components Exposing Public

These rules are ALWAYS ACTIVE for screen components, navigators, and custom hooks that expose public API contracts in React Native applications, including LoginScreen, ChatScreen, AuthStackNavigator, and authentication hooks that manage external API integration.

### Rules

- **R-HOOKS-001** SHOULD: Components exposing public contracts SHOULD use TypeScript interfaces to define navigation parameter types (RootStackParamList).
- **R-HOOKS-002** SHOULD: Use useState for local component state (form inputs, UI toggles); use useSelector for global state access (authentication status, user data).
- **R-HOOKS-003** SHOULD: Wrap expensive computations in useMemo and callback functions in useCallback when passing to child components to prevent unnecessary re-renders.
- **R-HOOKS-004** SHOULD: Configure axios interceptors in centralized config to inject authentication tokens from react-native-secure-storage for all API requests.
- **R-HOOKS-005** SHOULD: Define TypeScript interfaces for all navigation parameter lists and export them as part of public API contracts for type-safe navigation.
- **R-HOOKS-006** SHOULD: Use StyleSheet.create for all component styles to enable performance optimizations and provide type checking for style properties.
- **R-HOOKS-007** MUST: Enable eslint-plugin-react-hooks with exhaustive-deps rule to enforce correct dependency array usage in all hook implementations.
- **R-HOOKS-008** MUST: Implement comprehensive error handling in custom authentication hooks (useAuth) covering success, failure, and network error scenarios.
- **R-HOOKS-009** MUST: Use react-native-secure-storage for token persistence and implement token refresh logic in authentication configuration.

### Verify

```bash
# Check for consistent React hooks usage across public API components
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify StyleSheet.create usage across all components
grep -r "StyleSheet.create" template/src/screens/ template/src/components/

# Verify axios HTTP client usage for external API calls
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# Verify TypeScript compilation succeeds for public API contracts
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
- Navigation parameter types are defined as TypeScript interfaces and exported as public API contracts
- Custom authentication hooks implement comprehensive error handling
- Token persistence uses react-native-secure-storage with token refresh logic

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting changes to public API components. TypeScript compilation and ESLint hooks rules are mandatory checks in the CI pipeline.
</enforcement>