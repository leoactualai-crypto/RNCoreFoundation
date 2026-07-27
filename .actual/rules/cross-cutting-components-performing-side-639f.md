# Adopt React Hooks for State Management in React Native UI Components: Components Performing Side

These rules are ALWAYS ACTIVE for all React Native UI component development within the codebase, including screen components, navigation components, custom hooks, and components using react-native-paper UI elements.

### Rules

- **R-HOOKS-001** MUST: Components performing side effects (API calls, subscriptions, storage operations) MUST use the useEffect hook with appropriate dependency arrays.

### Verify

```bash
# Count hook usage across the codebase
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify no new class components are introduced
grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint checks for hook rules
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All new React Native functional components use hooks (useState, useEffect, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.
- All useEffect hooks include appropriate dependency arrays to prevent stale closures and infinite render loops.

<enforcement>
Clause Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks is enforced in the CI pipeline. Code review blocks merge if new class components are introduced without documented exception. Automated PR comments flag missing dependencies in useEffect/useCallback for reviewer attention.
</enforcement>