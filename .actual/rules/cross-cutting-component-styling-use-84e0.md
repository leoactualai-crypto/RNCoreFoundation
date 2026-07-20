# Standardize React Native with React Navigation and Material Design Components: Component Styling Use

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, and UI component libraries within the project.

### Rules

- **R-STYLING-001** MUST: Component styling MUST use StyleSheet.create() from 'react-native' for style definitions to ensure optimization and type safety.

### Verify

```bash
# Verify StyleSheet.create() usage across all component files
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native imports are present
grep -r "from 'react-native'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify no inline style objects in complex styling scenarios
grep -r "style={{" template/src --include='*.tsx' --include='*.ts' | grep -v "simple\|temporary" | wc -l

# Verify react-native-paper Material Design components are used
grep -r "react-native-paper" template/src --include='*.tsx' | wc -l
```

**Accept when:**
- All React Native component files import 'react-native' as a core dependency
- StyleSheet.create() appears in all component files defining styles, with no inline style objects for complex styling
- Material Design components from react-native-paper are used for common UI elements (buttons, text inputs, cards) across screens
- Navigation implementations use @react-navigation libraries with consistent patterns across stack, drawer, and tab navigators
- Redux Toolkit slices use createSlice() for state management, axios is configured for HTTP clients, and react-native-secure-storage handles credential persistence

<enforcement>
Claude Code MUST NOT skip or defer verification. All style definitions must be validated against StyleSheet.create() patterns before accepting component implementations.
</enforcement>