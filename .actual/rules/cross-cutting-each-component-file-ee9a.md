# Adopt StyleSheet.create() as Standard React Native Styling Pattern: Each Component File

These rules are ALWAYS ACTIVE for all React Native component files (screens, navigators, and reusable UI components) that render native View, Text, or other React Native components.

### Rules

- **R-STYLESHEET-001** SHOULD: Each component file SHOULD define its own StyleSheet.create() block rather than importing styles from external style modules, maintaining component-level style encapsulation.
- **R-STYLESHEET-002** MUST: StyleSheet.create() calls MUST be placed at the bottom of each component file, after the component definition but before the export statement, for consistent file structure.
- **R-STYLESHEET-003** SHOULD: Style objects SHOULD use TypeScript or Flow type annotations to enable IDE autocomplete and catch type errors: `const styles: StyleSheet.NamedStyles<any> = StyleSheet.create({...})`
- **R-STYLESHEET-004** SHOULD: For dynamic styling needs, SHOULD create style arrays combining static StyleSheet styles with computed inline styles: `style={[styles.base, dynamicCondition && styles.variant]}`
- **R-STYLESHEET-005** MAY: Inline style objects MAY be used only in combination with StyleSheet-defined styles or for documented dynamic styling exceptions (EXC-001, EXC-002).
- **R-STYLESHEET-006** SHOULD: Large StyleSheet.create() blocks SHOULD be limited to approximately 15-20 style keys per component to maintain readability and ease maintenance.

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
- StyleSheet.create() calls are positioned at the bottom of component files before export statements
- Exception cases (EXC-001, EXC-002) are documented in component file comments with clear rationale

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native component files in scope. Violations detected by ESLint or grep verification MUST be resolved or explicitly documented as approved exceptions before code review approval.
</enforcement>