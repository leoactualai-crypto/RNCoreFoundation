# Standardize Authentication Service Boundary with Axios HTTP Client: Authentication Implementations Integrate

These rules are ALWAYS ACTIVE for all authentication operations in React/React Native applications using axios HTTP client, custom authentication hooks, and remote API endpoints.

### Rules

- **R-AUTH-001** MUST: All authentication operations (login, registration) use axios.post with explicit /auth/local or /auth/local/register endpoint paths.
- **R-AUTH-002** MUST: Authentication logic is encapsulated within a custom useAuth hook that serves as the public contract for UI components.
- **R-AUTH-003** MUST: BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code.
- **R-AUTH-004** MUST: HTTP client configuration for authentication service endpoints includes consistent error handling, request/response transformation, and interceptor capabilities.
- **R-AUTH-005** SHOULD: Authentication implementations integrate TypeScript interfaces for request payloads (LoginRequest, RegisterRequest) and response types to enforce contract consistency.
- **R-AUTH-006** SHOULD: Error handling within the useAuth hook maps axios errors to domain-specific authentication error types (InvalidCredentials, NetworkError, ServerError).
- **R-AUTH-007** MAY: Authentication implementations MAY integrate with react-native-secure-storage for credential persistence.

### Verify

```bash
# Verify all authentication operations use axios.post with explicit endpoint paths
grep -r "axios\.post.*auth/local" template/src/components/AuthHook/

# Verify useAuth hook is exported as public contract
grep -r "useAuth" template/src/components/ | grep -c "export"

# Verify BASE_URL configuration is defined in separate config module
grep -r "BASE_URL" template/src/config/ | grep -v node_modules
```

**Accept when:**
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code
- Error handling maps axios errors to domain-specific authentication error types
- Integration tests validate authentication flows through useAuth hook contract

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication service boundary changes require confirmation that rules R-AUTH-001 through R-AUTH-004 are satisfied before merge.
</enforcement>