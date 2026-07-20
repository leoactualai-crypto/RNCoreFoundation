# Define Public API Contracts for Authentication-Aware React Native Navigation: Stylesheet Definitions Colocated

These rules are ALWAYS ACTIVE for React Native mobile applications using @react-navigation/native and @react-navigation/stack, components that require authentication state or perform navigation operations, and root-level App components that initialize navigation and authentication context.

### Rules

- **R-NAVAUTH-001** SHOULD: StyleSheet definitions SHOULD be colocated with components and use consistent design tokens (Colors.lighter, Colors.white, etc.) for maintainable styling.
- **R-NAVAUTH-002** MUST: Define RootStackParamList in a dedicated types file (e.g., navigation.types.ts) and export it as a public contract for use across the application.
- **R-NAVAUTH-003** MUST: Implement the useAuth() hook in a context provider (e.g., AuthContext.tsx) and wrap the App component with the provider to ensure authentication state is available throughout the navigation tree.
- **R-NAVAUTH-004** MUST: Use TypeScript's NavigationProp and RouteProp types from @react-navigation/native with RootStackParamList to type-check navigation props in screen components.
- **R-NAVAUTH-005** SHOULD: Document the navigation structure and authentication flow in the project README, including examples of how to add new routes and integrate authentication checks.

### Verify

```bash
# Verify RootStackParamList is defined and referenced
grep -r "RootStackParamList" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify useAuth() hook is implemented and used
grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts"

# Verify TypeScript compilation succeeds with no navigation type errors
npx tsc --noEmit --project tsconfig.json 2>&1 | grep -i "navigation\|route" || echo "No navigation type errors"
```

**Accept when:**
- RootStackParamList type contract is defined and exported, and grep verification shows it is referenced in multiple files
- useAuth() hook is implemented and used in the App component or navigation configuration
- TypeScript compilation succeeds with no navigation-related type errors

<enforcement>
Clause MUST NOT skip or defer verification. TypeScript compilation failures block pull request merges until navigation type errors are resolved. Code review verification is required that new navigation routes are added to RootStackParamList. Automated linting rules MUST enforce useAuth() hook usage for authentication-dependent screens.
</enforcement>