# Define Public API Contracts for Authentication-Aware React Native Navigation: Authentication State Accessed

These rules are ALWAYS ACTIVE for React Native mobile applications using @react-navigation/native and @react-navigation/stack, components that require authentication state or perform navigation operations, root-level App components that initialize navigation and authentication context, and type definitions for navigation parameters and route configurations.

### Rules

- **R-AUTH-001** MUST: Authentication state MUST be accessed through a centralized useAuth() hook pattern to ensure consistent authentication context across navigation flows.

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
Clause Code MUST NOT skip or defer verification. TypeScript compiler checks during CI/CD pipeline execution are mandatory. Code review verification that new navigation routes are added to RootStackParamList is required. Automated linting rules that enforce useAuth() hook usage for authentication-dependent screens must be applied.
</enforcement>