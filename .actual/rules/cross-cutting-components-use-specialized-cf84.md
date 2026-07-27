# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Components Use Specialized

These rules are ALWAYS ACTIVE for all React Native screen components, custom hooks, navigation components, and authentication UI flows within the template application.

### Rules

- **R-HOOKS-001** MUST: Use React hooks (useState, useEffect, useCallback, useMemo, useSelector) for managing component state and side effects in all screen components.
- **R-HOOKS-002** MUST: Define component styles using StyleSheet.create() with structured style definitions organized at the bottom of component files.
- **R-HOOKS-003** MUST: Extract custom hooks following the useXxx naming convention for any stateful logic used in more than one component.
- **R-HOOKS-004** MUST: Specify complete dependency arrays for all useEffect calls and enable ESLint exhaustive-deps rule validation.
- **R-HOOKS-005** MUST: Use useSelector with selector functions that extract only the specific state slice needed to minimize re-renders.
- **R-HOOKS-006** SHOULD: Use TypeScript interfaces to define component props and state shapes for type inference and compile-time validation.
- **R-HOOKS-007** SHOULD: Integrate react-native-safe-area-context for proper safe area handling on iOS devices.
- **R-HOOKS-008** MAY: Components MAY use specialized third-party interaction libraries (e.g., react-native-gifted-chat) for domain-specific UI patterns when they provide significant functionality beyond basic components.

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
- TypeScript compilation succeeds with no type errors for hooks and style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint hooks rules MUST pass in CI pipeline before merge. Code review MUST verify hooks usage patterns and StyleSheet.create() for all new components. TypeScript compilation errors for incorrect hook usage MUST be resolved before deployment.
</enforcement>