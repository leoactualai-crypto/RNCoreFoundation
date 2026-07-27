# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Screen Components Define

These rules are ALWAYS ACTIVE for all React Native screen components, custom hooks, navigation components, and authentication UI flows within the template application.

### Rules

- **R-HOOKS-001** MUST: All screen components MUST define styles using StyleSheet.create() with typed style objects rather than inline style objects or external CSS.
- **R-HOOKS-002** MUST: All screen components MUST use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for state management rather than class-based components.
- **R-HOOKS-003** MUST: Custom hooks MUST follow the useXxx naming convention and be extracted to reusable modules in template/src/components/ or template/src/hooks/.
- **R-HOOKS-004** MUST: All useEffect calls MUST specify complete dependency arrays and comply with ESLint exhaustive-deps rule.
- **R-HOOKS-005** SHOULD: StyleSheet.create() definitions SHOULD be organized at the bottom of component files with descriptive names reflecting semantic purpose.
- **R-HOOKS-006** SHOULD: Components accessing global state SHOULD use useSelector with selector functions that extract only the specific state slice needed.
- **R-HOOKS-007** SHOULD: TypeScript interfaces SHOULD be used to define component props and state shapes for type inference with hooks.

### Verify

```bash
# Count hooks usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Verify StyleSheet.create() usage in screen components
grep -r "StyleSheet.create" template/src/screens/ --include="*.tsx" | wc -l

# Verify no class-based components exist
grep -r "class.*extends.*Component" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint hooks rules verification
npx eslint template/src/ --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn' --format json
```

**Accept when:**
- All screen components in template/src/screens/ use React hooks (useState, useEffect, etc.) with zero class-based components detected
- All screen components define styles using StyleSheet.create() with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow useXxx naming convention and are extracted to reusable modules in template/src/components/ or template/src/hooks/
- TypeScript compilation succeeds with no type errors for hooks and style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules MUST be verified before accepting component implementations. ESLint hooks rules MUST pass in CI pipeline before merge.
</enforcement>