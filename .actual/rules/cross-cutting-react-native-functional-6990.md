# Adopt React Hooks for State Management in React Native UI Components: React Native Functional

These rules are ALWAYS ACTIVE for all React Native functional components managing local UI state, navigation components, custom hooks, and components using react-native-paper UI elements within the codebase.

### Rules

- **R-RN-HOOKS-001** MUST: All React Native functional components managing local UI state MUST use the useState hook for state declarations.

### Verify

```bash
# Count hook usage across React Native components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src --include="*.tsx" --include="*.ts" | wc -l

# Verify no new class components have been introduced
grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint checks for hook rules compliance
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All new React Native functional components use hooks (useState, useEffect, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks is enforced in the CI pipeline. Code review blocks merge if new class components are introduced without documented exception. Automated PR comments flag missing dependencies in useEffect/useCallback for reviewer attention.
</enforcement>