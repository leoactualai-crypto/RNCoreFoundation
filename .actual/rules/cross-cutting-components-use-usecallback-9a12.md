# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Components Use Usecallback

These rules are ALWAYS ACTIVE for all React Native screen components in `template/src/screens/`, custom hooks in `template/src/components/` and `template/src/hooks/`, navigation components using `@react-navigation/stack`, components integrating with `react-redux` global state, and authentication/authorization UI flows.

### Rules

- **R-HOOKS-001** SHOULD: Components SHOULD use `useCallback` to memoize event handlers and `useMemo` to memoize computed values when passed as props to child components to prevent unnecessary re-renders.

### Verify

```bash
# Verify hooks usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Verify StyleSheet.create usage
grep -r "StyleSheet.create" template/src/screens/ --include="*.tsx" | wc -l

# Verify no class-based components exist
grep -r "class.*extends.*Component" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Verify ESLint hooks rules pass
npx eslint template/src/ --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn' --format json
```

**Accept when:**
- All screen components in `template/src/screens/` use React hooks (useState, useEffect, useCallback, useMemo, useSelector) with zero class-based components detected
- All screen components define styles using `StyleSheet.create()` with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow `useXxx` naming convention and are extracted to reusable modules in `template/src/components/` or `template/src/hooks/`
- `useCallback` is applied to event handlers passed as props to child components
- `useMemo` is applied to computed values passed as props to child components

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST execute successfully before accepting changes to React Native UI components.
</enforcement>