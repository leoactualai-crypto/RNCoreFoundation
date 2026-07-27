# Adopt StyleSheet.create for Component Styling in React Native Screens: Style Property Names

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory and custom components that define visual presentation.

### Rules

- **R-STYLE-001** SHOULD: Style property names SHOULD use lowercase naming convention (e.g., 'safeareaview', 'textinput') for consistency across all StyleSheet.create definitions.

### Verify

```bash
# Count StyleSheet.create usage in screen components
grep -r 'StyleSheet.create' template/src/screens/ | wc -l

# Count inline style objects that should be refactored
grep -r 'style={{' template/src/screens/ --include='*.tsx' | wc -l

# Find screen components missing StyleSheet.create
find template/src/screens -name '*.tsx' -exec grep -L 'StyleSheet.create' {} \;
```

**Accept when:**
- All screen components in `/screens` directory contain at least one `StyleSheet.create()` declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- Style property names follow lowercase naming convention consistently
- Linting passes without violations of style definition rules

<enforcement>
Claude Code MUST NOT skip or defer verification. All screen components must be checked for StyleSheet.create adoption and lowercase property naming conventions before accepting changes.
</enforcement>