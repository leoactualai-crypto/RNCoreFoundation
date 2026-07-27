# Adopt React Hooks for State Management in React Native UI Components: Expensive Computations Derived

These rules are ALWAYS ACTIVE for all React Native UI components, navigation components, custom hooks, and components using react-native-paper UI elements within the configured scope.

### Rules

- **R-HOOKS-001** SHOULD: Expensive computations or derived state SHOULD be memoized using the useMemo hook to optimize rendering performance.

### Verify

```bash
# Count hook usage across the codebase
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify no new class components have been introduced
grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint checks for hook rules
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All new React Native functional components use hooks (useState, useEffect, useMemo, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.
- Expensive computations and derived state are wrapped with useMemo to prevent unnecessary recalculations on re-render.

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks is enforced in the CI pipeline. Code review must verify hooks usage for new components. TypeScript type checking ensures correct hook usage patterns. CI build fails if eslint-plugin-react-hooks rules are violated.
</enforcement>