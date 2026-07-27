# Adopt React Hooks and StyleSheet Pattern for React Native UI State Management: Components Requiring Global

These rules are ALWAYS ACTIVE for all React Native UI components and screens within the template application, including Chat, Login, Registration, and navigation components in `template/src/screens/`, custom hooks in `template/src/components/`, and components integrating with react-redux global state.

### Rules

- **R-HOOKS-001** MUST: Components requiring global state access MUST use useSelector hook from react-redux rather than connect() higher-order component pattern.
- **R-HOOKS-002** MUST: All screen components MUST define styles using StyleSheet.create() with at least one style definition per component.
- **R-HOOKS-003** MUST: React hooks (useState, useEffect, useCallback, useMemo, useSelector) MUST be called at the top level of components, never conditionally or in loops.
- **R-HOOKS-004** MUST: useEffect hooks MUST specify complete dependency arrays; the exhaustive-deps ESLint rule MUST pass.
- **R-HOOKS-005** SHOULD: Extract custom hooks for any stateful logic used in more than one component, following the useXxx naming convention.
- **R-HOOKS-006** SHOULD: Use TypeScript interfaces to define component props and state shapes for type inference with hooks.
- **R-HOOKS-007** SHOULD: Organize StyleSheet.create() definitions at the bottom of component files with descriptive names reflecting semantic purpose.
- **R-HOOKS-008** SHOULD: For components accessing global state, use useSelector with selector functions that extract only the specific state slice needed.

### Verify

```bash
# Count hook usage across screens and components
grep -r "useState\|useEffect\|useCallback\|useMemo\|useSelector" template/src/screens/ template/src/components/ --include="*.tsx" --include="*.ts" | wc -l

# Count StyleSheet.create() usage in screen components
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

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint with eslint-plugin-react-hooks enforcing rules-of-hooks and exhaustive-deps rules MUST pass in CI pipeline. Code review MUST verify hooks usage patterns and StyleSheet.create() for new components. TypeScript compilation MUST enforce type safety for hooks and style definitions. CI build MUST fail if ESLint hooks rules report errors, blocking merge until violations are resolved.
</enforcement>