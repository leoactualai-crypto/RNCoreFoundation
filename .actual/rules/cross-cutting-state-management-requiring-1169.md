# Standardize React Native with React Navigation and Material Design Components: State Management Requiring

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, authentication flows, and secure storage implementations within the configured scope.

### Rules

- **R-STATE-001** MUST: State management requiring Redux patterns MUST use '@reduxjs/toolkit' for slice definitions and store configuration.

### Verify

```bash
# Verify Redux Toolkit usage in state management
grep -r "@reduxjs/toolkit" template/src --include='*.tsx' --include='*.ts' | grep -E "(createSlice|configureStore)" | wc -l

# Verify React and React Native core imports
grep -r "from 'react'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify React Navigation patterns
grep -r "@react-navigation" template/src --include='*.tsx' --include='*.ts' | grep -E "(stack|drawer|native|material-bottom-tabs)" | wc -l

# Verify StyleSheet.create patterns
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native-paper usage
grep -r "react-native-paper" template/src --include='*.tsx' | wc -l

# Verify Redux Toolkit, axios, and secure storage in package.json
find template/src -name 'package.json' -exec grep -l '@reduxjs/toolkit\|axios\|react-native-secure-storage' {} \;
```

**Accept when:**
- Redux Toolkit slices use `createSlice()` for state management with consistent patterns across all state modules
- All Redux state management implementations import and use '@reduxjs/toolkit' for slice definitions and store configuration
- Redux store is configured using `configureStore()` from '@reduxjs/toolkit' with proper middleware setup
- Navigation implementations use @react-navigation libraries with consistent patterns across stack, drawer, and tab navigators
- StyleSheet.create() appears in all component files defining styles, with no inline style objects for complex styling
- Material Design components from react-native-paper are used for common UI elements (buttons, text inputs, cards) across screens
- axios is configured for HTTP clients with centralized base URL and interceptors
- react-native-secure-storage handles credential persistence for authentication flows
- All React Native component files import 'react' and 'react-native' as core dependencies
- TypeScript ParamList types are defined for all navigators to enforce type-safe navigation parameter passing

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be checked before approving code changes. Violations block pull request merges and require architecture review for exceptions.
</enforcement>