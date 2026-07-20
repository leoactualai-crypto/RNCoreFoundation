# Adopt useAuth Hook as Standard Authentication Provider Pattern in React Native Applications: Authentication Provider Components

These rules are ALWAYS ACTIVE for all React Native functional components requiring authentication state, screen components within React Navigation stack navigators, custom hooks that depend on authentication context, and components rendering conditional UI based on authentication status.

### Rules

- **R-AUTH-001** SHOULD: Authentication provider components SHOULD be placed at the root level of the React Native application, above navigation containers.

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
Claude Code MUST NOT skip or defer verification. Authentication provider pattern compliance is mandatory for all React Native applications in scope.
</enforcement>