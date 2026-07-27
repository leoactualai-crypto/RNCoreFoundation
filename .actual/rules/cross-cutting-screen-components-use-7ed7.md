# Standardize React Hooks for UI State Management in React Native Screens: Screen Components Use

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** MUST: Screen components MUST use React hooks (useState, useCallback, useMemo, useEffect) for managing local component state and side effects.

### Verify

```bash
# Verify React hooks are used in screen components
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

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks is enforced in CI pipeline. Code review must verify hook usage patterns and dependency arrays. CI build fails if violations are detected.
</enforcement>