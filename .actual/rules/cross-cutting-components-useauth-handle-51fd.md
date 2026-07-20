# Adopt useAuth Hook as Standard Authentication Provider Pattern in React Native Applications: Components Useauth Handle

These rules are ALWAYS ACTIVE for all React Native functional components requiring authentication state, screen components within React Navigation stack navigators, custom hooks that depend on authentication context, and components rendering conditional UI based on authentication status.

### Rules

- **R-USEAUTH-001** SHOULD: Components using useAuth() SHOULD handle both authenticated and unauthenticated states gracefully with appropriate UI feedback.

### Verify

```bash
# Check for useAuth() hook usage in component files
grep -r "useAuth()" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify useAuth hook is imported
grep -r "import.*useAuth" template/src --include="*.tsx" --include="*.ts"

# Check for authentication context creation
grep -r "createContext.*Auth" template/src --include="*.tsx" --include="*.ts"
```

**Accept when:**
- useAuth() hook is imported and called in at least one component file
- Authentication context provider is present in the application root or App component
- No direct authentication state management (useState for auth tokens) exists outside the authentication provider

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands must execute successfully and return results indicating useAuth() pattern adoption before accepting code changes.
</enforcement>