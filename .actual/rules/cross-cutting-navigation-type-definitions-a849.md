# Standardize React Native with React Navigation and Material Design Components: Navigation Type Definitions

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, and authentication flows across the codebase.

### Rules

- **R-NAV-001** MAY: Navigation type definitions MAY define ParamList types to enforce type-safe navigation parameters across screens.

### Verify

```bash
# Verify React core dependencies are present
grep -r "from 'react'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify @react-navigation libraries are used consistently
grep -r "@react-navigation" template/src --include='*.tsx' --include='*.ts' | grep -E "(stack|drawer|native|material-bottom-tabs)" | wc -l

# Verify StyleSheet.create() patterns are used
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native-paper usage
grep -r "react-native-paper" template/src --include='*.tsx' | wc -l

# Verify state management and storage libraries
find template/src -name 'package.json' -exec grep -l '@reduxjs/toolkit\|axios\|react-native-secure-storage' {} \;
```

**Accept when:**
- All React Native component files import 'react' and 'react-native' as core dependencies
- Navigation implementations use @react-navigation libraries with consistent patterns across stack, drawer, and tab navigators
- StyleSheet.create() appears in all component files defining styles, with no inline style objects for complex styling
- Material Design components from react-native-paper are used for common UI elements (buttons, text inputs, cards) across screens
- Redux Toolkit slices use createSlice() for state management, axios is configured for HTTP clients, and react-native-secure-storage handles credential persistence
- ParamList types are defined for all navigators (RootStackParamList, TopicDrawerStackParamList, MaterialBottomTabParamList) to enforce type-safe navigation parameter passing

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST execute successfully before accepting navigation type definitions and related implementations.
</enforcement>