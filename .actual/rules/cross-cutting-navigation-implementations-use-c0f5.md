# Standardize React Native with React Navigation and Material Design Components: Navigation Implementations Use

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, and navigation implementations across the codebase.

### Rules

- **R-NAV-001** MUST: Navigation implementations MUST use `@react-navigation/native` as the base navigation library, with `@react-navigation/stack`, `@react-navigation/drawer`, or `@react-navigation/material-bottom-tabs` for specific navigation patterns.

### Verify

```bash
# Verify react and react-native core dependencies
grep -r "from 'react'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify @react-navigation libraries are used consistently
grep -r "@react-navigation" template/src --include='*.tsx' --include='*.ts' | grep -E "(stack|drawer|native|material-bottom-tabs)" | wc -l

# Verify StyleSheet.create() patterns for styling
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native-paper Material Design components
grep -r "react-native-paper" template/src --include='*.tsx' | wc -l

# Verify Redux Toolkit, axios, and secure storage integration
find template/src -name 'package.json' -exec grep -l '@reduxjs/toolkit\|axios\|react-native-secure-storage' {} \;
```

**Accept when:**
- All React Native component files import `react` and `react-native` as core dependencies
- Navigation implementations use `@react-navigation` libraries with consistent patterns across stack, drawer, and tab navigators
- `StyleSheet.create()` appears in all component files defining styles, with no inline style objects for complex styling
- Material Design components from `react-native-paper` are used for common UI elements (buttons, text inputs, cards) across screens
- Redux Toolkit slices use `createSlice()` for state management, axios is configured for HTTP clients, and `react-native-secure-storage` handles credential persistence
- TypeScript ParamList types are defined for all navigators (RootStackParamList, TopicDrawerStackParamList, MaterialBottomTabParamList)
- Theme configuration uses `react-native-paper`'s Provider component for consistent Material Design theming

<enforcement>
Claude Code MUST NOT skip or defer verification. All navigation implementations MUST be checked against R-NAV-001 requirements before approval. TypeScript compilation errors for navigation type mismatches MUST prevent builds. ESLint violations for inline styles or missing StyleSheet.create() MUST block pull request merges.
</enforcement>