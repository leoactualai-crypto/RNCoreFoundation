# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: React Native Screen

These rules are ALWAYS ACTIVE for all React Native screen components, custom hooks, navigation components, and authentication UI flows within the template application.

### Rules

- **R-RN-HOOKS-001** MUST: All React Native screen components MUST use React hooks (useState, useEffect, useCallback, useMemo) for local state management and side effects rather than class-based component lifecycle methods.
- **R-RN-HOOKS-002** MUST: All screen components MUST define styles using StyleSheet.create() with structured style definitions organized at the bottom of component files.
- **R-RN-HOOKS-003** MUST: Custom hooks MUST follow the useXxx naming convention and be extracted to reusable modules in template/src/components/ or template/src/hooks/.
- **R-RN-HOOKS-004** MUST: When using useEffect, MUST specify complete dependency arrays and enable ESLint exhaustive-deps rule validation.
- **R-RN-HOOKS-005** SHOULD: Use TypeScript interfaces to define component props and state shapes for type inference with hooks.
- **R-RN-HOOKS-006** SHOULD: For components accessing global state, use useSelector with selector functions that extract only the specific state slice needed.
- **R-RN-HOOKS-007** SHOULD: Prefer useCallback and useMemo to stabilize dependencies in useEffect arrays.
- **R-RN-HOOKS-008** MAY: Inline styles or styled-components may be used only for highly dynamic styles that change on every render based on props, with explicit justification.

### Verify

```bash
# Count React hooks usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Count StyleSheet.create() definitions in screen components
grep -r "StyleSheet.create" template/src/screens/ --include="*.tsx" | wc -l

# Verify no class-based components exist
grep -r "class.*extends.*Component" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Run ESLint hooks rules validation
npx eslint template/src/ --ext .ts,.tsx --rule 'react-hooks/rules-of-hooks: error' --rule 'react-hooks/exhaustive-deps: warn' --format json
```

**Accept when:**
- All screen components in template/src/screens/ use React hooks (useState, useEffect, etc.) with zero class-based components detected
- All screen components define styles using StyleSheet.create() with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow useXxx naming convention and are extracted to reusable modules in template/src/components/ or template/src/hooks/

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint hooks rules MUST pass in CI pipeline before merge. TypeScript compilation MUST succeed with no type errors for hooks and style definitions. Code review MUST verify hooks usage patterns and StyleSheet.create() for all new components.
</enforcement>