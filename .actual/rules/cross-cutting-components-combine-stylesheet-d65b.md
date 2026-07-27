# Adopt StyleSheet.create for Component Styling in React Native Screens: Components Combine Stylesheet

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory and custom components that define visual presentation.

### Rules

- **R-STYLESHEET-001** MAY: Components MAY combine StyleSheet-defined styles with react-native-paper component theming.

### Verify

```bash
# Count StyleSheet.create declarations in screen components
grep -r 'StyleSheet.create' template/src/screens/ | wc -l

# Count inline style objects in screen components
grep -r 'style={{' template/src/screens/ --include='*.tsx' | wc -l

# Find screen components without StyleSheet.create
find template/src/screens -name '*.tsx' -exec grep -L 'StyleSheet.create' {} \;
```

**Accept when:**
- All screen components in `/screens` directory contain at least one `StyleSheet.create()` declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- Linting passes without violations of style definition rules
- Exceptions for dynamic styling are documented in component comments with technical justification

<enforcement>
Claude Code MUST NOT skip or defer verification. All screen components must be checked for StyleSheet.create compliance before accepting changes.
</enforcement>