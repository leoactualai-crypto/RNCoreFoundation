# Define Public API Contracts for Authentication-Aware React Native Navigation: Navigation Type Contracts

These rules are ALWAYS ACTIVE for React Native applications using @react-navigation/native and @react-navigation/stack that integrate authentication state with navigation management.

### Rules

- **R-NAV-001** SHOULD: Navigation type contracts SHOULD be exported as public API contracts to enable type-safe navigation calls from any component in the application.

### Verify

```bash
# Verify RootStackParamList is defined and exported
grep -r "RootStackParamList" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify useAuth() hook is implemented and used
grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts"

# Verify TypeScript compilation succeeds with no navigation-related type errors
npx tsc --noEmit --project tsconfig.json 2>&1 | grep -i "navigation\|route" || echo "No navigation type errors"
```

**Accept when:**
- RootStackParamList type contract is defined and exported, and grep verification shows it is referenced in multiple files
- useAuth() hook is implemented and used in the App component or navigation configuration
- TypeScript compilation succeeds with no navigation-related type errors

<enforcement>
Verification via TypeScript compiler checks during CI/CD pipeline execution is mandatory. Code review must verify that new navigation routes are added to RootStackParamList. Violations block pull request merges until navigation type errors are resolved.
</enforcement>