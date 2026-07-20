# Standardize React Native with React Navigation and Material Design Components: Icon Implementations Use

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, and UI component libraries within the configured scope.

### Rules

- **R-ICON-001** SHOULD: Icon implementations SHOULD use 'react-native-vector-icons/MaterialCommunityIcons' for consistent iconography across the application.

### Verify

```bash
# Verify react-native-vector-icons/MaterialCommunityIcons is in dependencies
grep -r "react-native-vector-icons" template/src --include='*.tsx' --include='*.ts' --include='*.js' | grep -i "materialcommunityicons" | wc -l

# Verify no alternative icon libraries are used
grep -r "from.*react-native-vector-icons" template/src --include='*.tsx' --include='*.ts' --include='*.js' | grep -v "MaterialCommunityIcons" | wc -l

# Verify icon imports follow consistent pattern
grep -r "MaterialCommunityIcons" template/src --include='*.tsx' --include='*.ts' --include='*.js' | wc -l
```

**Accept when:**
- All icon implementations import from 'react-native-vector-icons/MaterialCommunityIcons'
- No alternative icon libraries (FontAwesome, Feather, Entypo, etc.) are used for primary iconography
- Icon usage is consistent across navigation implementations (stack, drawer, and tab navigators)
- Material Community Icons are used for common UI elements across all screens and components

<enforcement>
Claude Code MUST NOT skip or defer verification of icon library consistency. Icon implementations using non-standard libraries or inconsistent sources MUST be flagged for remediation before approval.
</enforcement>