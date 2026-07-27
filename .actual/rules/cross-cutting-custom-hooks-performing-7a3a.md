# Standardize React Hooks for UI State Management in React Native Screens: Custom Hooks Performing

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in the `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** MUST: Custom hooks performing side effects (HTTP requests, storage operations) MUST handle error states and loading states explicitly.

### Verify

```bash
# Verify React hooks usage across screen components
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
- All custom hooks performing async operations (HTTP, storage) explicitly return loading and error states alongside data

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations block CI pipeline. Code review must verify hook usage patterns and dependency arrays before merge.
</enforcement>