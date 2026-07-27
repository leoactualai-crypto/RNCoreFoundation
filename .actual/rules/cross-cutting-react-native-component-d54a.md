# Adopt StyleSheet.create() as Standard React Native Styling Pattern: React Native Component

These rules are ALWAYS ACTIVE for all React Native component files including screens, navigators, and reusable UI components that render native View, Text, or other React Native components.

### Rules

- **R-RN-STYLE-001** MUST: All React Native component files MUST define styles using StyleSheet.create() rather than plain JavaScript objects or inline style definitions.

### Verify

```bash
# Count StyleSheet.create() usage across component files
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" | wc -l

# Count inline style objects (should be minimal/zero)
grep -r "style={{" template/src --include="*.tsx" | grep -v "style=\[" | wc -l

# Run ESLint rule for inline styles
eslint template/src --rule 'react-native/no-inline-styles: error' --ext .tsx,.ts
```

**Accept when:**
- All React Native component files contain at least one StyleSheet.create() call for component-specific styles
- Inline style objects (style={{...}}) are only used in combination with StyleSheet-defined styles or for documented dynamic styling exceptions
- ESLint rule 'react-native/no-inline-styles' passes with zero violations across the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. All React Native component files must be checked for StyleSheet.create() compliance before accepting changes.
</enforcement>