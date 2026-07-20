# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Navigator Components Separate

These rules are ALWAYS ACTIVE for all React Native mobile application development within the codebase, including screens, navigators, UI components, and style definitions.

### Rules

- **R-NAV-001** SHOULD: Navigator components SHOULD separate navigation structure from screen implementation, with navigators defined in dedicated navigator modules.

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
- All screen components import navigation dependencies from @react-navigation scoped packages (@react-navigation/stack, @react-navigation/drawer, @react-navigation/material-bottom-tabs) and define typed navigation parameter lists (TopicDrawerStackParamList, MaterialBottomTabParamList, RootStackParamList).
- UI components consistently use react-native-paper components for Material Design elements and react-native-vector-icons/MaterialCommunityIcons for iconography.
- Navigator modules contain co-located types.ts files defining navigation contracts with StackScreenProps or equivalent typed navigation props.
- StyleSheet.create() is used consistently across component files with descriptive property names reflecting component structure (container, title, button, etc.).
- Verification commands return counts consistent with codebase size (29+ files using @react-navigation and react-native-paper).

<enforcement>
Claude Code MUST NOT skip or defer verification. TypeScript compilation MUST enforce navigation parameter list types at build time. Code review MUST verify StyleSheet.create() usage and navigation contract definitions before merge.
</enforcement>