# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Screens Use React

These rules are ALWAYS ACTIVE for all React Native mobile application screens, navigators, and UI components within the codebase.

### Rules

- **R-RNNAV-001** MAY: Screens MAY use @react-navigation/native hooks (useNavigation, useFocusEffect) for navigation interactions within functional components.

### Verify

```bash
# Verify @react-navigation imports across screen components
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper usage for Material Design
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create() usage for style definitions
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation type definitions in navigator modules
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages (@react-navigation/stack, @react-navigation/drawer, @react-navigation/material-bottom-tabs).
- Navigation parameter lists (TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList) are defined with TypeScript types in co-located types.ts files.
- UI components consistently use react-native-paper components for Material Design elements and theming.
- StyleSheet.create() is used for all style definitions across screen and component files with descriptive property names.
- MaterialCommunityIcons from react-native-vector-icons/MaterialCommunityIcons are used consistently for iconography.
- Verification commands return counts consistent with codebase size (29+ files using @react-navigation and react-native-paper).

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation MUST enforce navigation parameter list types at build time. CI checks MUST verify @react-navigation and react-native-paper imports. Code review MUST validate StyleSheet.create() usage and navigation contract definitions before merge.
</enforcement>