# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Components Combine Stylesheet

These rules are ALWAYS ACTIVE for all React Native screen components, navigator components, reusable UI components, and the root application component (App.tsx) that render React Native View, Text, or other native components.

### Rules

- **R-STYLESHEET-001** MUST: Define component styles using `StyleSheet.create()` at the module level, placed after the component definition but before the export statement.
- **R-STYLESHEET-002** MAY: Components MAY combine StyleSheet-defined styles with dynamic inline styles when runtime style computation is required (e.g., conditional styling, theme-based colors).
- **R-STYLESHEET-003** SHOULD: Use style arrays combining static StyleSheet styles with computed inline styles for dynamic styling needs: `style={[styles.base, dynamicCondition && styles.variant]}`.
- **R-STYLESHEET-004** SHOULD: Use TypeScript or Flow type annotations for style objects to enable IDE autocomplete: `const styles: StyleSheet.NamedStyles<any> = StyleSheet.create({...})`.
- **R-STYLESHEET-005** MUST NOT: Create inline style objects (style={{...}}) without combining them with StyleSheet-defined styles, except for documented dynamic styling exceptions (EXC-001, EXC-002).

### Verify

```bash
# Count StyleSheet.create() usage in React Native component files
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" | wc -l

# Count inline style objects not combined with StyleSheet styles
grep -r "style={{" template/src --include="*.tsx" | grep -v "style=\[" | wc -l

# Run ESLint rule to detect inline styles
eslint template/src --rule 'react-native/no-inline-styles: error' --ext .tsx,.ts
```

**Accept when:**
- All React Native component files contain at least one `StyleSheet.create()` call for component-specific styles
- Inline style objects (`style={{...}}`) are only used in combination with StyleSheet-defined styles or for documented dynamic styling exceptions
- ESLint rule `react-native/no-inline-styles` passes with zero violations across the codebase
- All exceptions are documented in component file comments referencing EXC-001 or EXC-002

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must pass ESLint checks and automated grep-based verification before merge. Violations are automatically flagged for tech lead review.
</enforcement>