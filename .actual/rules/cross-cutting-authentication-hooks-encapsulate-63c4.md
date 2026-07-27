# Standardize Axios-Based External Authentication Client Boundaries: Authentication Hooks Encapsulate

These rules are ALWAYS ACTIVE for all React authentication hooks and components that communicate with external authentication services using axios HTTP clients.

### Rules

- **R-AUTH-001** MUST: Authentication hooks MUST encapsulate external client calls and expose a public contract (useAuth) that abstracts HTTP implementation details from consuming components.

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

<enforcement>
Claude Code MUST NOT skip or defer verification. All three verify commands must execute successfully and return expected results before accepting authentication hook implementations.
</enforcement>