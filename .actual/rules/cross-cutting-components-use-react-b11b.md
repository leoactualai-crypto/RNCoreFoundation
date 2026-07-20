# Standardize React Native with React Navigation and Material Design Components: Components Use React

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, and authentication flows across the codebase.

### Rules

- **R-RN-001** SHOULD: UI components SHOULD use 'react-native-paper' for Material Design components and theming to maintain consistent visual design language.

### Verify

```bash
# Verify React imports across component files
grep -r "from 'react'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify @react-navigation usage with consistent patterns
grep -r "@react-navigation" template/src --include='*.tsx' --include='*.ts' | grep -E "(stack|drawer|native|material-bottom-tabs)" | wc -l

# Verify StyleSheet.create() patterns in all component files
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native-paper usage for Material Design components
grep -r "react-native-paper" template/src --include='*.tsx' | wc -l

# Verify Redux Toolkit, axios, and secure storage in package.json
find template/src -name 'package.json' -exec grep -l '@reduxjs/toolkit\|axios\|react-native-secure-storage' {} \;
```

**Accept when:**
- All React Native component files import 'react' and 'react-native' as core dependencies
- Navigation implementations use @react-navigation libraries with consistent patterns across stack, drawer, and tab navigators
- StyleSheet.create() appears in all component files defining styles, with no inline style objects for complex styling
- Material Design components from react-native-paper are used for common UI elements (buttons, text inputs, cards) across screens
- Redux Toolkit slices use createSlice() for state management, axios is configured for HTTP clients, and react-native-secure-storage handles credential persistence
- TypeScript ParamList types are defined for all navigators (RootStackParamList, TopicDrawerStackParamList, MaterialBottomTabParamList)
- Theme configuration uses react-native-paper's Provider component for consistent Material Design theming

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native component development and must be verified before code acceptance.
</enforcement>