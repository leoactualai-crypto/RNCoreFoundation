# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Components Access Styles

These rules are ALWAYS ACTIVE for all React Native screen components, navigator components, reusable UI components, and the root application component (App.tsx) that render React Native View, Text, or other native components.

### Rules

- **R-STYLESHEET-001** SHOULD: Components SHOULD access styles via the created stylesheet object (e.g., styles.container) rather than destructuring or copying style objects.

### Verify

```bash
# Count StyleSheet.create() usage across component files
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" | wc -l

# Count inline style objects (excluding style arrays)
grep -r "style={{" template/src --include="*.tsx" | grep -v "style=\[" | wc -l

# Run ESLint rule for inline styles
eslint template/src --rule 'react-native/no-inline-styles: error' --ext .tsx,.ts
```

**Accept when:**
- All React Native component files contain at least one StyleSheet.create() call for component-specific styles
- Inline style objects (style={{...}}) are only used in combination with StyleSheet-defined styles or for documented dynamic styling exceptions
- ESLint rule 'react-native/no-inline-styles' passes with zero violations across the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification commands must pass before accepting this rule as satisfied.
</enforcement>