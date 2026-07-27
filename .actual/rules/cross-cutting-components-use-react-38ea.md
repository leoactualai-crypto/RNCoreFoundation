# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Components Use React

These rules are ALWAYS ACTIVE for all React Native UI components and screens within the template application, including screen components in `template/src/screens/`, custom hooks in `template/src/components/`, navigation components using `@react-navigation/stack`, and components integrating with react-redux global state.

### Rules

- **R-RN-HOOKS-001** MUST: UI components MUST use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for managing component state and side effects instead of class-based component patterns.
- **R-RN-HOOKS-002** MUST: UI components MUST use react-native-paper components for Material Design elements rather than implementing custom UI primitives.
- **R-RN-HOOKS-003** MUST: All component styles MUST be defined using StyleSheet.create() with structured style definitions organized at the bottom of component files.
- **R-RN-HOOKS-004** MUST: Custom hooks MUST follow the useXxx naming convention and be extracted to reusable modules in `template/src/components/` or `template/src/hooks/` when stateful logic is used in more than one component.
- **R-RN-HOOKS-005** MUST: When using useEffect, developers MUST always specify complete dependency arrays and enable ESLint exhaustive-deps rule to validate dependencies.
- **R-RN-HOOKS-006** SHOULD: Use TypeScript interfaces to define component props and state shapes, enabling type inference for useState and other hooks to catch type errors at compile time.
- **R-RN-HOOKS-007** SHOULD: For components accessing global state, use useSelector with selector functions that extract only the specific state slice needed to minimize re-renders when unrelated state changes.
- **R-RN-HOOKS-008** SHOULD: Integrate react-native-safe-area-context for proper safe area handling on iOS devices using SafeAreaView.

### Verify

```bash
# Count hook usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Count StyleSheet.create() definitions in screen components
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
- No inline style objects or styled-components are used for component styling

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native UI components. Violations must be resolved before code review approval.
</enforcement>