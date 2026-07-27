# Standardize React Hooks for UI State Management in React Native Screens: Screen Components Define

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** SHOULD: Screen components SHOULD define styles using `StyleSheet.create` with descriptive property names (container, title, button, textinput).
- **R-HOOKS-002** SHOULD: Screen components SHOULD use React hooks (useState, useCallback, useMemo, useEffect, useSelector) for managing component state and side effects.
- **R-HOOKS-003** SHOULD: Custom hooks performing async operations (HTTP, storage) SHOULD follow the pattern established in `useAuth.ts`: return loading state, error state, and data/methods in a consistent object structure.
- **R-HOOKS-004** MUST: Hooks MUST NOT be called conditionally or inside loops; they MUST be called at the top level of functional components.
- **R-HOOKS-005** SHOULD: Dependency arrays in `useEffect` and other hooks SHOULD be exhaustive and include all external values referenced within the hook.

### Verify

```bash
# Verify React hooks usage in screen components
grep -r "useState\|useCallback\|useMemo\|useEffect\|useSelector" template/src/screens/ --include="*.tsx" --include="*.ts"

# Verify no class components exist in screens directory
grep -r "class.*extends.*Component" template/src/screens/ --include="*.tsx" --include="*.ts" | wc -l

# Verify ESLint rules of hooks compliance
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All screen components in `/screens` directory use React hooks (useState, useEffect, etc.) with no class components present
- ESLint reports zero violations of `react-hooks/rules-of-hooks` and `react-hooks/exhaustive-deps` rules
- Custom hooks like `useAuth` are present in dedicated hooks directory and follow consistent return value patterns (loading, error, data/methods)
- StyleSheet.create is used in screen components with descriptive property names

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks MUST be enforced in CI pipeline. Code review MUST verify hook usage patterns and dependency arrays before merge. Violations block CI build and code review approval.
</enforcement>