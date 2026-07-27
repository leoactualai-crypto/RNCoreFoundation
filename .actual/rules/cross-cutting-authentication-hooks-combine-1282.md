# Standardize Axios-Based External Authentication Client Boundaries: Authentication Hooks Combine

These rules are ALWAYS ACTIVE for all React authentication hooks and components that communicate with external authentication services using axios HTTP client, including login, registration, and token refresh operations.

### Rules

- **R-AUTH-001** MAY: Authentication hooks MAY combine multiple external service calls (login, register) within a single hook interface for related operations.

### Verify

```bash
# Verify axios is used for authentication endpoints
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

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands must execute successfully and confirm axios usage, proper imports, and secure storage integration before accepting authentication hook implementations.
</enforcement>