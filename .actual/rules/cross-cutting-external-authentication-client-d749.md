# Standardize Axios-Based External Authentication Client Boundaries: External Authentication Client

These rules are ALWAYS ACTIVE for all React and React Native authentication hooks and components that communicate with external authentication services using axios HTTP clients.

### Rules

- **R-AUTH-001** SHOULD: External authentication client boundaries SHOULD use React hooks (useMemo, useEffect) to coordinate UI lifecycle with asynchronous service calls.

### Verify

```bash
# Verify axios is used for authentication operations
grep -r "axios.post.*auth/local" template/src/components/AuthHook/ | wc -l

# Verify axios is imported in useAuth hook
grep -r "import.*axios" template/src/components/AuthHook/useAuth.ts

# Verify react-native-secure-storage integration
grep -r "react-native-secure-storage" template/src/components/AuthHook/useAuth.ts
```

**Accept when:**
- All authentication operations (login, registration) use axios.post with BASE_URL configuration and standardized endpoint paths
- The useAuth hook exports a public contract that abstracts axios implementation details from consuming components
- Credential storage integration with react-native-secure-storage is present in the authentication hook implementation
- axios interceptors are configured at application initialization for consistent error handling and token management
- TypeScript interfaces are defined for authentication request and response payloads
- Comprehensive error handling distinguishes between network errors, authentication failures, and service errors

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All authentication client boundaries must be reviewed against R-AUTH-001 during code review and static analysis.
</enforcement>