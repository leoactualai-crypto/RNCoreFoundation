# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Stylesheet Definitions Colocated

These rules are ALWAYS ACTIVE for all React Native screen components in `template/src/screens/`, custom hooks in `template/src/components/` and `template/src/hooks/`, navigation components using `@react-navigation/stack`, components integrating with `react-redux` global state, and authentication/authorization UI flows.

### Rules

- **R-HOOKS-001** SHOULD: StyleSheet definitions SHOULD be colocated with their component definitions at the bottom of the file, using descriptive style names that reflect semantic purpose.
- **R-HOOKS-002** MUST: All screen components MUST use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for state management instead of class-based components.
- **R-HOOKS-003** MUST: Custom hooks MUST follow the `useXxx` naming convention and be extracted to reusable modules in `template/src/components/` or `template/src/hooks/`.
- **R-HOOKS-004** SHOULD: When using useEffect, SHOULD always specify complete dependency arrays and enable ESLint exhaustive-deps rule validation.
- **R-HOOKS-005** SHOULD: For components accessing global state, SHOULD use useSelector with selector functions that extract only the specific state slice needed to minimize re-renders.
- **R-HOOKS-006** MUST: TypeScript interfaces MUST be used to define component props and state shapes, enabling type inference for useState and other hooks.

### Verify

```bash
# Count React hooks usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Verify StyleSheet.create() usage in screen components
grep -r "StyleSheet.create" template/src/screens/ --include="*.tsx" | wc -l

# Verify no class-based components exist
grep -r "class.*extends.*Component" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint with hooks rules enforcement
npx eslint template/src/ --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn' --format json
```

**Accept when:**
- All screen components in `template/src/screens/` use React hooks (useState, useEffect, etc.) with zero class-based components detected
- All screen components define styles using StyleSheet.create() with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow useXxx naming convention and are extracted to reusable modules in `template/src/components/` or `template/src/hooks/`
- TypeScript compilation succeeds with no type errors for hooks and style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules MUST be verified before accepting component implementations. ESLint hooks rules MUST pass in CI pipeline before merge.
</enforcement>