# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Navigation Parameter Types

These rules are ALWAYS ACTIVE for all React Native mobile application development within the codebase, including all screen components, navigators, and UI implementations using @react-navigation and react-native-paper.

### Rules

- **R-NAV-001** MUST: Navigation parameter types MUST be defined using TypeScript interfaces that extend navigation param list types (e.g., RootStackParamList, MaterialBottomTabParamList).
- **R-NAV-002** MUST: All screen components MUST import navigation dependencies from @react-navigation scoped packages (@react-navigation/stack, @react-navigation/drawer, @react-navigation/material-bottom-tabs).
- **R-NAV-003** MUST: UI components MUST use react-native-paper components for Material Design elements and theming.
- **R-NAV-004** MUST: Style definitions MUST use StyleSheet.create() for layout, typography, spacing, and visual presentation.
- **R-NAV-005** MUST: Navigator modules MUST contain co-located types.ts files defining navigation contracts and parameter lists.
- **R-NAV-006** SHOULD: Use react-native-vector-icons/MaterialCommunityIcons for consistent iconography across the application.
- **R-NAV-007** SHOULD: Access theme values through useTheme() hook from react-native-paper for consistent styling.
- **R-NAV-008** MAY: Custom native navigation components may be used for platform-specific features not supported by @react-navigation (EXC-001).
- **R-NAV-009** MAY: Alternative UI component libraries may be used for specialized components not available in react-native-paper (EXC-002).

### Verify

```bash
# Verify @react-navigation imports across codebase
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper imports
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create usage
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation types.ts files exist in navigators
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- TypeScript compilation succeeds with no navigation parameter type errors.
- No inline styles are detected in screen or navigator components.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native mobile application code. Violations must be caught during code review and CI checks before merge.
</enforcement>