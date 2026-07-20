# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Component Styles Defined

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, and navigation flows within the codebase.

### Rules

- **R-RNMD-001** MUST: Component styles MUST be defined using StyleSheet.create() with typed style objects rather than inline style objects.

### Verify

```bash
# Verify @react-navigation imports across screen components
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper imports for Material Design components
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create() usage for style definitions
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation type definitions are co-located in navigator modules
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- No inline style objects are detected in screen or component implementations.
- All style definitions follow the pattern of StyleSheet.create() with descriptive property names reflecting component structure (container, title, button, etc.).

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All React Native component files MUST comply with R-RNMD-001 before acceptance. Violations result in CI build failures and code review rejection.
</enforcement>