# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: React Native Mobile

These rules are ALWAYS ACTIVE for all React Native mobile application development within the codebase, including all screens, navigators, UI components, and styling implementations.

### Rules

- **R-RN-001** MUST: All React Native mobile screens MUST use @react-navigation for navigation implementation, including stack, drawer, and tab navigation patterns.
- **R-RN-002** MUST: All navigation parameter lists MUST be defined as TypeScript types (e.g., TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList) in co-located types.ts files within navigator directories.
- **R-RN-003** MUST: All screen components MUST import navigation dependencies from @react-navigation scoped packages (@react-navigation/stack, @react-navigation/drawer, @react-navigation/material-bottom-tabs).
- **R-RN-004** MUST: All UI components requiring Material Design theming MUST use react-native-paper components and the useTheme() hook for consistent styling.
- **R-RN-005** MUST: All style definitions MUST use StyleSheet.create() with descriptive property names reflecting component structure (container, title, button, etc.).
- **R-RN-006** MUST: All icon usage MUST import from react-native-vector-icons/MaterialCommunityIcons with consistent icon naming conventions.
- **R-RN-007** SHOULD: Organize navigator components in dedicated directories (e.g., navigators/TopicDrawerNavigator/) with co-located types.ts files for navigation contracts.
- **R-RN-008** SHOULD: Define screen components by establishing TypeScript navigation param list types first, then implementing screen components with typed navigation props using StackScreenProps or similar types.

### Verify

```bash
# Verify @react-navigation imports across screen components
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper usage for Material Design
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create() usage for style definitions
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation type definitions in co-located files
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- All screens follow navigation contract patterns with TypeScript-typed navigation props.
- No inline styles are present; all styling uses StyleSheet.create().
- MaterialCommunityIcons are imported consistently from react-native-vector-icons/MaterialCommunityIcons.

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST execute successfully before accepting changes to React Native mobile screens, navigators, or UI components. TypeScript compilation MUST enforce navigation parameter list types at build time. Code review MUST verify StyleSheet.create() usage and navigation contract definitions.
</enforcement>