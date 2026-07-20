# Standardize Authentication Service Boundary with Axios HTTP Client: Authentication Service Interactions

These rules are ALWAYS ACTIVE for all authentication service interactions in React/React Native applications using axios HTTP client and custom React hooks.

### Rules

- **R-AUTH-001** MUST: Authentication service interactions MUST be encapsulated within custom React hooks that expose a public useAuth contract.

### Verify

```bash
# Verify axios.post calls to auth/local endpoints are within AuthHook module
grep -r "axios\.post.*auth/local" template/src/components/AuthHook/

# Verify useAuth hook is exported as public contract
grep -r "useAuth" template/src/components/ | grep -c "export"

# Verify BASE_URL configuration is externalized
grep -r "BASE_URL" template/src/config/ | grep -v node_modules
```

**Accept when:**
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication service interactions MUST conform to the useAuth hook encapsulation pattern with axios HTTP client.
</enforcement>