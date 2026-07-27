# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Custom Hooks Extracted

These rules are ALWAYS ACTIVE for all React Native UI components and screens within the template application, including Chat, Login, Registration, and navigation components in `template/src/screens/`, custom hooks in `template/src/components/` and `template/src/hooks/`, and components integrating with react-navigation, react-redux, and react-native-paper.

### Rules

- **R-HOOKS-001** SHOULD: Custom hooks SHOULD be extracted for reusable stateful logic that spans multiple components, following the useXxx naming convention (e.g., useAuth).
- **R-HOOKS-002** SHOULD: All screen components SHOULD use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for state management instead of class-based components.
- **R-HOOKS-003** SHOULD: All screen components SHOULD define styles using StyleSheet.create() with structured style definitions organized at the bottom of component files.
- **R-HOOKS-004** SHOULD: When using useEffect, SHOULD always specify complete dependency arrays and enable ESLint exhaustive-deps rule validation.
- **R-HOOKS-005** SHOULD: Components accessing global state SHOULD use useSelector with selector functions that extract only the specific state slice needed to minimize re-renders.
- **R-HOOKS-006** SHOULD: TypeScript interfaces SHOULD be used to define component props and state shapes, enabling type inference for useState and other hooks.

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
- All screen components in `template/src/screens/` use React hooks (useState, useEffect, etc.) with zero class-based components detected
- All screen components define styles using StyleSheet.create() with at least one style definition per component
- ESLint hooks rules (rules-of-hooks, exhaustive-deps) pass with zero errors and fewer than 5 warnings across the codebase
- Custom hooks follow useXxx naming convention and are extracted to reusable modules in `template/src/components/` or `template/src/hooks/`
- TypeScript compilation succeeds with no type errors for hooks and style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST be executed and pass before accepting changes. ESLint hooks rules MUST be enforced in CI pipeline. Code review MUST verify hooks usage patterns and StyleSheet.create() for new components. TypeScript compilation errors for incorrect hook usage MUST be resolved before deployment.
</enforcement>