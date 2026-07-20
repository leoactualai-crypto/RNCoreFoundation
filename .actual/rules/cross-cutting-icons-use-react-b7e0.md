# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Icons Use React

These rules are ALWAYS ACTIVE for all React Native mobile application development within the codebase, including all screens, navigators, components, and feature modules.

### Rules

- **R-ICONS-001** SHOULD: Icons SHOULD use react-native-vector-icons/MaterialCommunityIcons for consistent iconography across the application.

### Verify

```bash
# Verify @react-navigation imports are present
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper imports are present
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create usage
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation types are co-located
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- MaterialCommunityIcons from react-native-vector-icons are used consistently across icon implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for React Native mobile application code within scope.
</enforcement>