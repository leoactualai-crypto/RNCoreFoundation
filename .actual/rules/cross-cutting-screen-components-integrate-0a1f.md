# Standardize React Hooks for UI State Management in React Native Screens: Screen Components Integrate

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory, custom hooks in `/components` or `/hooks` directories, navigation components using `@react-navigation/stack`, and components requiring global state access via `react-redux`.

### Rules

- **R-HOOKS-001** MAY: Screen components MAY integrate specialized libraries (react-native-gifted-chat, react-native-safe-area-context) when standard components are insufficient.
- **R-HOOKS-002** MUST: All screen components use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for state management instead of class components.
- **R-HOOKS-003** MUST: Custom hooks coordinating multiple concerns (HTTP, storage, Redux state) follow the pattern established in useAuth.ts, returning a consistent object structure with loading state, error state, and data/methods.
- **R-HOOKS-004** SHOULD: Each custom hook be limited to a single responsibility and documented with JSDoc or TypeScript interfaces describing purpose, parameters, and return values.
- **R-HOOKS-005** MUST: All hook calls respect the Rules of Hooks: no conditional calls, no calls inside loops, and dependency arrays must be exhaustive.
- **R-HOOKS-006** SHOULD: Performance optimizations using useMemo and useCallback be applied only when profiling data demonstrates measurable benefit, not as premature optimization.

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
- No conditional hook calls or missing dependencies are detected in code review

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks MUST be enforced in CI pipeline. Code review MUST block merge if hooks are used conditionally or with incorrect dependencies. Performance regression tests MUST fail if screen render times exceed established budgets.
</enforcement>