# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Style Object Keys

These rules are ALWAYS ACTIVE for all React Native screen components, navigator components, reusable UI components, and the root application component that render React Native View, Text, or other native components.

### Rules

- **R-STYLESHEET-001** SHOULD: Style object keys SHOULD use camelCase naming convention (e.g., 'container', 'userInfoSection', 'drawerContent') for consistency with JavaScript conventions.

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
- Style object keys in all StyleSheet.create() blocks follow camelCase naming convention

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native UI components within scope.
</enforcement>