# Adopt StyleSheet.create() for Component Styling in React Native: Style Object Keys

These rules are ALWAYS ACTIVE for all React Native functional and class components, navigation components (drawer, stack, tab navigators), screen components, reusable UI components, and layout containers and wrappers.

### Rules

- **R-STYLE-001** SHOULD: Style object keys SHOULD use descriptive names that reflect the component or layout purpose (e.g., container, title, button, text).

### Verify

```bash
# Count StyleSheet.create() usage in component files
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" --include="*.jsx" --include="*.js" | wc -l

# Count inline style objects without StyleSheet.create()
grep -r "style={{" template/src --include="*.tsx" --include="*.jsx" | grep -v "StyleSheet" | wc -l

# Count component files importing react-native but not using StyleSheet.create()
find template/src -type f \( -name "*.tsx" -o -name "*.jsx" \) -exec grep -l "from 'react-native'" {} \; | xargs grep -L "StyleSheet.create" | wc -l
```

**Accept when:**
- StyleSheet.create() usage count is greater than or equal to the number of component files that import react-native
- Inline style object usage (style={{ }}) without StyleSheet.create() is less than 5% of total style definitions
- All screen components, navigator components, and reusable UI components use StyleSheet.create() for style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations block PR merge until resolved. Code review feedback requests refactoring of inline styles to StyleSheet.create(). CI pipeline warnings flag files with high inline style usage ratios. Exceptions require developer documentation and code reviewer approval based on technical justification.
</enforcement>