# Adopt React Hooks for State Management in React Native UI Components: Components Not Use

These rules are ALWAYS ACTIVE for all React Native UI component development within the codebase, including screen components, navigation components, custom hooks, and components using react-native-paper UI elements.

### Rules

- **R-HOOKS-001** MUST_NOT: Components MUST NOT use class-based lifecycle methods (componentDidMount, componentDidUpdate) in new code; use functional components with hooks instead.

### Verify

```bash
# Count hook usage across the codebase
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src --include="*.tsx" --include="*.ts" | wc -l

# Count class-based components
grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint checks for hook rules
npx eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'
```

**Accept when:**
- All new React Native functional components use hooks (useState, useEffect, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.
- No class-based lifecycle methods appear in new component code.

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks is enforced in the CI pipeline. Code review blocks merge if new class components are introduced without documented exception. Violations result in CI build failure.
</enforcement>