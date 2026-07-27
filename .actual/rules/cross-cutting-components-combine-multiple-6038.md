# Adopt React Hooks for State Management in React Native UI Components: Components Combine Multiple

These rules are ALWAYS ACTIVE for all React Native UI component development within the codebase, including screen components (Chat, Login, Registration), navigation components, custom hooks, and components using react-native-paper UI elements.

### Rules

- **R-HOOKS-001** MAY: Components MAY combine multiple hooks (useState, useEffect, useCallback) within a single component to manage complex interaction patterns.

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
- All new React Native functional components use hooks (useState, useEffect, etc.) for state management and lifecycle, with no new class components introduced.
- ESLint checks for react-hooks/rules-of-hooks and react-hooks/exhaustive-deps pass in CI without errors.
- Custom hooks follow naming convention (use* prefix) and are documented with clear input/output contracts and usage examples.
- Components combining multiple hooks maintain clear separation of concerns and follow the Rules of Hooks (top-level only, never in loops/conditions).

<enforcement>
Verification via ESLint with eslint-plugin-react-hooks is mandatory in the CI pipeline. Code review must verify hooks usage for all new components. TypeScript type checking must ensure correct hook usage patterns. CI build fails if eslint-plugin-react-hooks rules are violated. Code review blocks merge if new class components are introduced without documented exception.
</enforcement>