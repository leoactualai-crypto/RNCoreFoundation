# Standardize React Native with React Navigation and Material Design Components: React Native Components

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, authentication flows, and HTTP client configurations.

### Rules

- **R-RN-001** MUST: All React Native components MUST use 'react' and 'react-native' as core framework dependencies for component definition and platform APIs.
- **R-RN-002** MUST: All navigation implementations MUST use @react-navigation libraries with consistent patterns across stack, drawer, and tab navigators.
- **R-RN-003** MUST: All component styling MUST use StyleSheet.create() patterns with no inline style objects for complex styling.
- **R-RN-004** MUST: Material Design components from react-native-paper MUST be used for common UI elements (buttons, text inputs, cards) across screens.
- **R-RN-005** MUST: State management MUST use Redux Toolkit with createSlice() patterns for normalized state management.
- **R-RN-006** MUST: HTTP clients MUST be configured using axios with centralized base URL and interceptors for standardized API communication.
- **R-RN-007** MUST: Credential persistence MUST use react-native-secure-storage for secure storage implementations.
- **R-RN-008** MUST: Navigation implementations MUST define TypeScript ParamList types for all navigators to enforce type-safe navigation parameter passing.
- **R-RN-009** SHOULD: Theme configuration SHOULD be centralized using react-native-paper's Provider component to ensure consistent Material Design theming.
- **R-RN-010** SHOULD: Common layout patterns (containers, flex layouts, spacing) SHOULD be established in a shared styles directory to reduce duplication.

### Verify

```bash
# Verify react and react-native imports
grep -r "from 'react'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify @react-navigation usage across navigator types
grep -r "@react-navigation" template/src --include='*.tsx' --include='*.ts' | grep -E "(stack|drawer|native|material-bottom-tabs)" | wc -l

# Verify StyleSheet.create() patterns
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify react-native-paper usage
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
- TypeScript ParamList types are defined for all navigators to enforce type-safe navigation parameter passing
- Theme configuration is centralized using react-native-paper's Provider component
- Common layout patterns are established in a shared styles directory

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-RN-001 through R-RN-008 are mandatory (MUST level) and MUST be verified before accepting code. Rules R-RN-009 and R-RN-010 are strongly recommended (SHOULD level) and violations should be flagged for review. Exceptions EXC-001 and EXC-002 require architecture review board approval with documented technical justification.
</enforcement>