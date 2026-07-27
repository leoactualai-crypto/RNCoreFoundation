# Standardize React Hooks for UI State Management in React Native Screens: Components Accessing Global

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in the `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and any components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** MUST: Components accessing global state MUST use `useSelector` from `react-redux` rather than direct store access.

### Verify

```bash
# Verify all screen components use React hooks
grep -r "useState\|useCallback\|useMemo\|useEffect\|useSelector" template/src/screens/ --include="*.tsx" --include="*.ts"

# Verify no class components exist in screens directory
grep -r "class.*extends.*Component" template/src/screens/ --include="*.tsx" --include="*.ts" | wc -l

# Verify ESLint rules of hooks compliance
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All screen components in `/screens` directory use React hooks (useState, useEffect, useSelector, etc.) with no class components present
- ESLint reports zero violations of `react-hooks/rules-of-hooks` and `react-hooks/exhaustive-deps` rules
- Custom hooks like `useAuth` are present in dedicated hooks directory and follow consistent return value patterns (loading, error, data/methods)
- All global state access uses `useSelector` from `react-redux` with no direct store access patterns detected

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks MUST be enforced in CI pipeline. Code review MUST block merge if hooks are used conditionally or with incorrect dependencies. Violations result in CI build failure.
</enforcement>