# Adopt StyleSheet.create for Component Styling in React Native Screens: Style Objects Created

These rules are ALWAYS ACTIVE for all React Native screen components and custom components in the `/screens` directory that define visual presentation and layout.

### Rules

- **R-STYLE-001** MUST: Style objects created with StyleSheet.create MUST be defined as constants outside the component render function.

### Verify

```bash
# Count StyleSheet.create declarations in screen components
grep -r 'StyleSheet.create' template/src/screens/ | wc -l

# Count inline style objects that should be refactored
grep -r 'style={{' template/src/screens/ --include='*.tsx' | wc -l

# Find screen components missing StyleSheet.create
find template/src/screens -name '*.tsx' -exec grep -L 'StyleSheet.create' {} \;
```

**Accept when:**
- All screen components in `/screens` directory contain at least one `StyleSheet.create()` declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- No violations of style definition rules are detected by linting
- Exceptions are documented in component comments with technical justification

<enforcement>
Claude Code MUST NOT skip or defer verification. All screen components must be checked for StyleSheet.create compliance before accepting changes.
</enforcement>