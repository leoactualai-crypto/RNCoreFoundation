# Define Public API Contracts for Authentication-Aware React Native Navigation: Root App Component

These rules are ALWAYS ACTIVE for React Native applications using @react-navigation/native and @react-navigation/stack, root-level App components that initialize navigation and authentication context, and components that require authentication state or perform navigation operations.

### Rules

- **R-RAPP-001** MUST: The root App component MUST coordinate authentication state with navigation configuration to enforce authentication-aware routing.

### Verify

```bash
# Verify RootStackParamList is defined and exported
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
Clause R-RAPP-001 verification is mandatory. TypeScript compiler checks during CI/CD pipeline execution, code review verification that new navigation routes are added to RootStackParamList, and automated linting rules that enforce useAuth() hook usage for authentication-dependent screens MUST all pass before code is accepted.
</enforcement>