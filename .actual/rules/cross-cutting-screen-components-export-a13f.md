# Standardize React Native with React Navigation and Material Design for Mobile UI Architecture: Screen Components Export

These rules are ALWAYS ACTIVE for all React Native mobile application screens, navigators, and UI components within the codebase.

### Rules

- **R-RN-001** SHOULD: Screen components SHOULD export typed navigation contracts that define their public API surface for navigation and parameter passing.

### Verify

```bash
# Verify @react-navigation imports across screen components
grep -r "from '@react-navigation/" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify react-native-paper usage for Material Design
grep -r "from 'react-native-paper'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify StyleSheet.create() usage for optimized styling
grep -r "StyleSheet.create" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l

# Verify navigation type definitions in navigator modules
find template/src/navigators -name 'types.ts' -o -name '*.tsx' | xargs grep -l 'ParamList' | wc -l
```

**Accept when:**
- All screen components import navigation dependencies from @react-navigation scoped packages and define typed navigation parameter lists.
- UI components consistently use react-native-paper components for Material Design elements and StyleSheet.create() for style definitions.
- Navigator modules contain co-located types.ts files defining navigation contracts, and verification commands return counts consistent with codebase size (29+ files).
- TypeScript compilation succeeds with no navigation parameter type errors.
- No inline styles or alternative UI library usage detected without documented exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST execute successfully before accepting changes to screen components, navigators, or UI styling patterns.
</enforcement>