# Define Public API Contracts for Authentication-Aware React Native Navigation: Components Use Usestate

These rules are ALWAYS ACTIVE for React Native applications using @react-navigation/native and @react-navigation/stack with centralized authentication state management through useAuth() hooks.

### Rules

- **R-AUTH-NAV-001** MAY: Components MAY use useState() for local UI interaction state that does not affect authentication or navigation logic.

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
Clause MUST NOT skip or defer verification. TypeScript compiler checks during CI/CD pipeline execution are mandatory. Code review verification that new navigation routes are added to RootStackParamList is required. Automated linting rules that enforce useAuth() hook usage for authentication-dependent screens must pass.
</enforcement>