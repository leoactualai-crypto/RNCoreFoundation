# Standardize Authentication Service Boundary with Axios HTTP Client: Login Requests Include

These rules are ALWAYS ACTIVE for all authentication operations in React/React Native applications using axios HTTP client and the useAuth custom hook pattern.

### Rules

- **R-AUTH-001** MUST: Login requests MUST include identifier and password fields in the request payload

### Verify

```bash
# Verify axios.post calls to auth/local endpoints exist
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
- Login request payloads contain both identifier (or email) and password fields

<enforcement>
Clause Code MUST NOT skip or defer verification. Authentication service boundary violations detected by grep patterns or static analysis MUST block CI pipeline and require architectural review board approval before merge.
</enforcement>