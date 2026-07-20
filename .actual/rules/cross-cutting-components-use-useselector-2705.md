# Adopt React Hooks for State and Effect Management in UI Components: Components Use Useselector

These rules are ALWAYS ACTIVE for all functional React and React Native components, custom hooks, screen components, and navigation components that require state management, side effects, or context access.

### Rules

- **R-HOOKS-001** SHOULD: Components SHOULD use useSelector from react-redux for accessing global state rather than direct store access.

### Verify

```bash
# Count hook usage across the codebase
grep -r "useState\|useEffect\|useCallback\|useMemo" template/src --include="*.tsx" --include="*.ts" | wc -l

# Validate ESLint rules for hooks compliance
eslint template/src --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn'

# Verify no new class components are introduced
grep -r "class.*extends.*Component" template/src --include="*.tsx" --include="*.ts" || echo 'No class components found'
```

**Accept when:**
- All functional components use hooks (useState, useEffect, etc.) for state and effects with no violations of Rules of Hooks
- ESLint validation passes with react-hooks plugin rules enabled showing no errors for hooks usage
- Custom hooks follow naming conventions (use* prefix) and are properly extracted for reusable stateful logic
- No new class-based components are introduced except with documented exception approval
- Components using global state access it via useSelector from react-redux rather than direct store access

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint pre-commit hooks with react-hooks/rules-of-hooks and react-hooks/exhaustive-deps rules MUST be enabled. Code review MUST verify proper hook usage, dependency arrays, and custom hook abstractions. CI pipeline MUST run ESLint validation on all TypeScript/TSX files with hooks-specific rules. Violations block CI pipeline and prevent merge.
</enforcement>