# Define Public API Contracts for Authentication-Aware React Native Navigation: React Native Applications

These rules are ALWAYS ACTIVE for all React Native applications using @react-navigation/native and @react-navigation/stack that require authentication state management and type-safe routing.

### Rules

- **R-RNNAV-001** MUST: All React Native applications using @react-navigation/stack MUST define a RootStackParamList type contract that explicitly declares all navigation routes and their parameter types.

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
Clause R-RNNAV-001 verification is mandatory. TypeScript compiler checks during CI/CD pipeline execution MUST pass. Code review MUST verify that new navigation routes are added to RootStackParamList. Violations block pull request merges until navigation type errors are resolved.
</enforcement>