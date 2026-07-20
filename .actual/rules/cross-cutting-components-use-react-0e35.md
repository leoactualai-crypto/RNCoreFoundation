# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Components Use React

These rules are ALWAYS ACTIVE for all React Native mobile application development within the codebase, including screens, navigators, UI components, and style definitions.

### Rules

- **R-RNNAV-001** MUST: UI components MUST use react-native-paper for Material Design themed components including buttons, text inputs, cards, and surface elements.
- **R-RNNAV-002** MUST: All screen components MUST import navigation dependencies from @react-navigation scoped packages (@react-navigation/stack, @react-navigation/drawer, @react-navigation/material-bottom-tabs).
- **R-RNNAV-003** MUST: Navigation parameter lists MUST be defined as TypeScript types (e.g., TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList) in co-located types.ts files within navigator directories.
- **R-RNNAV-004** MUST: Screen components MUST use typed navigation props via StackScreenProps or equivalent types from @react-navigation.
- **R-RNNAV-005** MUST: Style definitions MUST use StyleSheet.create() with descriptive property names reflecting component structure (container, title, button, etc.).
- **R-RNNAV-006** MUST: Icons MUST be imported from react-native-vector-icons/MaterialCommunityIcons with consistent naming conventions across the application.
- **R-RNNAV-007** MUST: The react-native-paper Provider component MUST be used at the app root to enable theming.
- **R-RNNAV-008** SHOULD: Theme values SHOULD be accessed through the useTheme() hook from react-native-paper for consistent styling.
- **R-RNNAV-009** SHOULD: Navigator components SHOULD be organized in dedicated directories (e.g., navigators/TopicDrawerNavigator/) with co-located types.ts files for navigation contracts.

### Verify

```bash
# Verify @react-navigation imports across screen components
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper imports across components
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create() usage for style definitions
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation parameter list type definitions in navigator modules
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- No inline styles are detected in screen or component files; all styling uses StyleSheet.create().
- MaterialCommunityIcons imports are used consistently across the application for iconography.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native mobile application code. Violations must be caught during code review and CI checks before merge.
</enforcement>