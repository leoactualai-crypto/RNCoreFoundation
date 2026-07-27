# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Stylesheet Create Calls

These rules are ALWAYS ACTIVE for all React Native screen components, navigator components, reusable UI components, and the root application component that render React Native View, Text, or other native components.

### Rules

- **R-STYLESHEET-001** MUST: StyleSheet.create() calls MUST be placed at the module level (outside component functions) to ensure style objects are created once and reused across renders.

### Verify

```bash
# Count StyleSheet.create() usage across React Native files
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" | wc -l

# Count inline style objects (should be minimal or zero)
grep -r "style={{" template/src --include="*.tsx" | grep -v "style=\[" | wc -l

# Run ESLint rule to detect inline styles
eslint template/src --rule 'react-native/no-inline-styles: error' --ext .tsx,.ts
```

**Accept when:**
- All React Native component files contain at least one StyleSheet.create() call for component-specific styles
- Inline style objects (style={{...}}) are only used in combination with StyleSheet-defined styles or for documented dynamic styling exceptions
- ESLint rule 'react-native/no-inline-styles' passes with zero violations across the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verification commands must pass before accepting changes to React Native component styling patterns.
</enforcement>