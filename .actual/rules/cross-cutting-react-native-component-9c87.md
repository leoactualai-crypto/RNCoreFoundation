# Adopt StyleSheet.create() for Component Styling in React Native: React Native Component

These rules are ALWAYS ACTIVE for all React Native component files that require styling, including navigation components, screen components, and reusable UI components.

### Rules

- **R-RN-STYLE-001** MUST: All React Native component styles MUST be defined using StyleSheet.create() rather than plain JavaScript objects.

### Verify

```bash
# Count StyleSheet.create() usage
grep -r "StyleSheet.create" template/src --include="*.tsx" --include="*.ts" --include="*.jsx" --include="*.js" | wc -l

# Count inline style objects without StyleSheet
grep -r "style={{" template/src --include="*.tsx" --include="*.jsx" | grep -v "StyleSheet" | wc -l

# Find React Native component files that don't use StyleSheet.create()
find template/src -type f \( -name "*.tsx" -o -name "*.jsx" \) -exec grep -l "from 'react-native'" {} \; | xargs grep -L "StyleSheet.create" | wc -l
```

**Accept when:**
- StyleSheet.create() usage count is greater than or equal to the number of component files that import react-native
- Inline style object usage (style={{ }}) without StyleSheet.create() is less than 5% of total style definitions
- All screen components, navigator components, and reusable UI components use StyleSheet.create() for style definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. ESLint violations block PR merge until resolved. Code review feedback requests refactoring of inline styles to StyleSheet.create(). CI pipeline warnings flag files with high inline style usage ratios. Exceptions require developer documentation and code reviewer approval based on technical justification.
</enforcement>