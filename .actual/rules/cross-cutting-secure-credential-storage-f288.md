# Standardize React Native with React Navigation and Material Design Components: Secure Credential Storage

These rules are ALWAYS ACTIVE for all React Native mobile application screens, components, navigation implementations, UI component libraries, state management patterns, and authentication/secure storage implementations within the configured scope.

### Rules

- **R-CRED-001** MUST: Secure credential storage MUST use 'react-native-secure-storage' for persisting authentication tokens and sensitive data.

### Verify

```bash
# Verify react-native-secure-storage is present in dependencies
grep -r "react-native-secure-storage" template/src --include='*.tsx' --include='*.ts' --include='*.js' | head -5

# Verify authentication flows use secure storage patterns
grep -r "from 'react-native-secure-storage'" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify no plaintext token storage in AsyncStorage or local state
grep -r "AsyncStorage.*token\|AsyncStorage.*password\|AsyncStorage.*credential" template/src --include='*.tsx' --include='*.ts' | wc -l

# Verify package.json includes react-native-secure-storage
find template/src -name 'package.json' -exec grep -l 'react-native-secure-storage' {} \;
```

**Accept when:**
- react-native-secure-storage is listed as a dependency in package.json
- All authentication token persistence uses react-native-secure-storage APIs
- No sensitive credentials are stored in AsyncStorage, Redux state, or local component state
- Integration tests verify secure storage behavior on both Android and iOS platforms
- Credential retrieval and storage operations are centralized in authentication service modules

<enforcement>
Claude Code MUST NOT skip or defer verification of secure credential storage patterns. Violations of R-CRED-001 represent security risks and MUST be flagged during code review.
</enforcement>