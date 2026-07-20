# Standardize Authentication Service Boundary with Axios HTTP Client: Registration Requests Include

These rules are ALWAYS ACTIVE for all authentication service implementations, custom hooks, and HTTP client configurations in React/React Native applications using axios for authentication operations.

### Rules

- **R-AUTH-001** MUST: Registration requests MUST include username, email, and password fields in the request payload.

### Verify

```bash
# Verify axios.post calls to auth/local endpoints are present
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
- Registration request payloads explicitly include username, email, and password fields

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication service boundary changes require confirmation that registration requests include username, email, and password fields in the request payload.
</enforcement>