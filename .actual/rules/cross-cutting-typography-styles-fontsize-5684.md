# Adopt StyleSheet.create for Component Styling in React Native Screens: Typography Styles Fontsize

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory and custom components that define visual presentation, layout containers, and typography styling.

### Rules

- **R-STYLESHEET-001** SHOULD: Typography styles (fontSize, fontWeight, color) SHOULD be centralized in StyleSheet definitions rather than defined inline or computed dynamically on every render.

### Verify

```bash
# Count StyleSheet.create declarations in screen components
grep -r 'StyleSheet.create' template/src/screens/ | wc -l

# Count inline style objects in screen components
grep -r 'style={{' template/src/screens/ --include='*.tsx' | wc -l

# Find screen components missing StyleSheet.create
find template/src/screens -name '*.tsx' -exec grep -L 'StyleSheet.create' {} \;
```

**Accept when:**
- All screen components in `/screens` directory contain at least one `StyleSheet.create()` declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- Typography styles (fontSize, fontWeight, color) are defined within StyleSheet.create blocks
- Linting passes without violations of style definition rules
- Exceptions for dynamic styling are documented in component comments with technical justification

<enforcement>
Claude Code MUST NOT skip or defer verification. All screen components MUST be checked for StyleSheet.create usage and typography style centralization before accepting changes.
</enforcement>