# Standardize Axios-Based External Authentication Client Boundaries: External Authentication Calls

These rules are ALWAYS ACTIVE for all React and React Native components, hooks, and services that implement external authentication API communication, including login, registration, token management, and credential storage operations.

### Rules

- **R-AUTH-001** MUST: External authentication API calls MUST use axios as the HTTP client library for all authentication operations including login, registration, and token management.

### Verify

```bash
# Verify axios is used for authentication endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/ | wc -l

# Verify axios import in useAuth hook
grep -r "import.*axios" template/src/components/AuthHook/useAuth.ts

# Verify react-native-secure-storage integration
grep -r "react-native-secure-storage" template/src/components/AuthHook/useAuth.ts
```

**Accept when:**
- All authentication operations (login, registration) use axios.post with BASE_URL configuration and standardized endpoint paths
- The useAuth hook exports a public contract that abstracts axios implementation details from consuming components
- Credential storage integration with react-native-secure-storage is present in the authentication hook implementation

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication API calls must be validated against R-AUTH-001 to ensure axios is the exclusive HTTP client for external authentication boundaries.
</enforcement>