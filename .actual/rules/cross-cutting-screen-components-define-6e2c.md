# Adopt StyleSheet.create for Component Styling in React Native Screens: Screen Components Define

These rules are ALWAYS ACTIVE for all React Native screen components in the `/screens` directory and custom components that define visual presentation.

### Rules

- **R-STYLESHEET-001** MUST: All screen components MUST define their styles using StyleSheet.create() at the module level.
- **R-STYLESHEET-002** MUST: StyleSheet.create() calls MUST be defined at the bottom of each screen component file, after the component definition.
- **R-STYLESHEET-003** MUST: Style names MUST be descriptive and clearly indicate the component or element they apply to (e.g., 'container', 'title', 'button').
- **R-STYLESHEET-004** SHOULD: For shared styles across multiple components, extract common style definitions into a separate theme or styles module.
- **R-STYLESHEET-005** SHOULD: When dynamic styling is necessary, compute style values outside the render method and combine with StyleSheet-defined base styles using array syntax.
- **R-STYLESHEET-006** MAY: Inline style objects MAY be used only for dynamic styling that cannot be pre-defined (EXC-001) or when integrating third-party libraries that require inline style objects (EXC-002).

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
- All screen components in `/screens` directory contain at least one StyleSheet.create() declaration
- Inline style objects are used only for dynamic styling that cannot be pre-defined
- ESLint linting passes without violations of style definition rules
- StyleSheet.create() calls are positioned at the bottom of component files after component definition
- Style names follow descriptive naming conventions
- Exceptions are documented in component comments with technical justification

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for screen component styling in this React Native project.
</enforcement>