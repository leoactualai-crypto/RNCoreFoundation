# Standardize Authentication Service Boundary with Axios HTTP Client: Authentication Service Boundaries

These rules are ALWAYS ACTIVE for all authentication operations in React/React Native applications using axios HTTP client with custom hooks that expose authentication functionality to UI components.

### Rules

- **R-AUTH-001** MUST: Authentication service boundaries MUST be defined using axios HTTP client with explicit POST requests to /auth/local and /auth/local/register endpoints

### Verify

```bash
# Verify axios.post calls to auth/local endpoints exist in AuthHook module
grep -r "axios\.post.*auth/local" template/src/components/AuthHook/

# Verify useAuth hook is exported as public contract
grep -r "useAuth" template/src/components/ | grep -c "export"

# Verify BASE_URL configuration is defined separately
grep -r "BASE_URL" template/src/config/ | grep -v node_modules
```

**Accept when:**
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication service boundary implementations MUST pass the verify commands above before acceptance.
</enforcement>