# Standardize Authentication Service Boundary with Axios HTTP Client: Base Url Configuration

These rules are ALWAYS ACTIVE for all authentication service implementations, custom hooks, and HTTP client configurations in React/React Native applications using axios for authentication operations.

### Rules

- **R-AUTH-001** SHOULD: Base URL configuration SHOULD be externalized to a config module to support environment-specific service endpoints.
- **R-AUTH-002** MUST: All authentication operations (login, registration) MUST use axios.post with explicit /auth/local or /auth/local/register endpoint paths through a centralized service boundary.
- **R-AUTH-003** MUST: Authentication logic MUST be encapsulated within a custom useAuth hook that provides a stable public API contract for UI components.
- **R-AUTH-004** SHOULD: TypeScript interfaces SHOULD be defined for authentication request payloads (LoginRequest, RegisterRequest) and response types to enforce contract consistency.
- **R-AUTH-005** SHOULD: Error handling within the useAuth hook SHOULD map axios errors to domain-specific authentication error types (InvalidCredentials, NetworkError, ServerError).
- **R-AUTH-006** MUST: Direct axios calls to authentication endpoints MUST NOT occur outside of designated service boundary modules.

### Verify

```bash
# Verify all authentication operations use axios.post with explicit endpoint paths
grep -r "axios\.post.*auth/local" template/src/components/AuthHook/

# Verify useAuth hook is exported as public contract
grep -r "useAuth" template/src/components/ | grep -c "export"

# Verify BASE_URL configuration is defined in config module
grep -r "BASE_URL" template/src/config/ | grep -v node_modules

# Detect direct authentication endpoint calls outside AuthHook module
grep -r "axios\.post.*auth/local" template/src/ | grep -v "AuthHook" | grep -v "node_modules"
```

**Accept when:**
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code
- No direct authentication endpoint calls exist outside of designated service boundary modules
- TypeScript interfaces are defined for authentication request and response payloads
- Error handling maps axios errors to domain-specific authentication error types

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis rules detecting direct authentication endpoint calls outside of designated service boundary modules MUST fail the CI pipeline. Pull requests introducing authentication logic bypass MUST be flagged for architectural review.
</enforcement>