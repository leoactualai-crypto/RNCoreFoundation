# Standardize React Hooks for UI State Management in Public API Components: Component Styling Use

These rules are ALWAYS ACTIVE for all screen components, navigators, and custom hooks exported as public API contracts in React Native applications, including LoginScreen, ChatScreen, AuthStackNavigator, and useAuth hook implementations.

### Rules

- **R-STYLING-001** MUST: Component styling MUST use StyleSheet.create for performance optimization and type safety.

### Verify

```bash
# Verify StyleSheet.create usage across all components
grep -r "StyleSheet.create" template/src/screens/ template/src/components/ template/src/navigators/

# Verify React hooks usage patterns
grep -r "useState\|useEffect\|useMemo\|useCallback\|useSelector" template/src/screens/ template/src/components/ template/src/navigators/

# Verify axios HTTP client usage
grep -r "axios.post\|axios.get" template/src/ | grep -v node_modules

# Verify TypeScript compilation
npx tsc --noEmit --project tsconfig.json
```

**Accept when:**
- All screen components and navigators use StyleSheet.create for all style definitions
- React hooks (useState, useEffect, useMemo, useCallback, useSelector) are used consistently for state management and side effects
- Axios HTTP client is configured centrally and used for all external API calls
- TypeScript compilation succeeds without errors for all public API contracts
- ESLint with eslint-plugin-react-hooks passes without violations

<enforcement>
Claude Code MUST NOT skip or defer verification. All style definitions in public API components MUST use StyleSheet.create. Violations block CI pipeline and code review merge.
</enforcement>