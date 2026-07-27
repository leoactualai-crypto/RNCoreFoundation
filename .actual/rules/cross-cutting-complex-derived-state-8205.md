# Standardize React Hooks for UI State Management in React Native Screens: Complex Derived State

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in the `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** SHOULD: Complex derived state or memoized callbacks SHOULD use `useMemo` and `useCallback` to optimize re-render performance.

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
- All screen components in `/screens` directory use React hooks (`useState`, `useEffect`, etc.) with no class components present
- ESLint reports zero violations of `react-hooks/rules-of-hooks` and `react-hooks/exhaustive-deps` rules
- Custom hooks like `useAuth` are present in dedicated hooks directory and follow consistent return value patterns (loading, error, data/methods)
- `useMemo` and `useCallback` are applied to complex derived state and memoized callbacks where profiling demonstrates measurable performance benefit

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks MUST be enforced in CI pipeline. Code review MUST verify hook usage patterns and dependency arrays before merge. Performance regression tests MUST pass for screen render times.
</enforcement>