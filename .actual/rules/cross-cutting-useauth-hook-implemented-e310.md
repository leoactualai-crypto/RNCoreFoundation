# Adopt useAuth Hook as Standard Authentication Provider Pattern in React Native Applications: Useauth Hook Implemented

These rules are ALWAYS ACTIVE for all React Native functional components requiring authentication state, screen components within React Navigation stack navigators, custom hooks that depend on authentication context, and components rendering conditional UI based on authentication status.

### Rules

- **R-AUTH-001** MUST: The useAuth() hook MUST be implemented as a React Context consumer to ensure consistent state across the component tree.

### Verify

```bash
# Check for useAuth() hook usage in component files
grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify useAuth() imports are present
grep -r "import.*useAuth" template/src --include="*.tsx" --include="*.ts"

# Check for authentication context creation
grep -r "createContext.*Auth" template/src --include="*.tsx" --include="*.ts"
```

**Accept when:**
- useAuth() hook is imported and called in at least one component file
- Authentication context provider is present in the application root or App component
- No direct authentication state management (useState for auth tokens) exists outside the authentication provider

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification commands must pass before accepting authentication state management patterns in React Native components.
</enforcement>