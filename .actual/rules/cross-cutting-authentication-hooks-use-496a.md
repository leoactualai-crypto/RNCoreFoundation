# Standardize Authentication Service Boundary with Axios HTTP Client: Authentication Hooks Use

These rules are ALWAYS ACTIVE for all authentication operations in React/React Native applications using axios and custom hooks to interact with authentication service endpoints.

### Rules

- **R-AUTH-001** SHOULD: Authentication hooks SHOULD use React useMemo and useEffect for optimized state management and side effect handling.
- **R-AUTH-002** MUST: All authentication operations (login, registration) MUST use axios.post with explicit /auth/local or /auth/local/register endpoint paths.
- **R-AUTH-003** MUST: The useAuth hook MUST be exported as the public contract and imported by UI components requiring authentication.
- **R-AUTH-004** MUST: BASE_URL configuration MUST be defined in a separate config module and referenced by authentication service boundary code.
- **R-AUTH-005** MUST: Authentication service boundary code MUST not be bypassed; direct authentication endpoint calls outside of designated service boundary modules are prohibited.
- **R-AUTH-006** SHOULD: Error handling within the useAuth hook SHOULD map axios errors to domain-specific authentication error types (InvalidCredentials, NetworkError, ServerError).
- **R-AUTH-007** SHOULD: TypeScript interfaces for authentication request payloads (LoginRequest, RegisterRequest) and response types SHOULD be defined to enforce contract consistency.

### Verify

```bash
# Verify axios.post calls to auth/local endpoints are contained within AuthHook module
grep -r "axios\.post.*auth/local" template/src/components/AuthHook/

# Verify useAuth hook is exported as public contract
grep -r "useAuth" template/src/components/ | grep -c "export"

# Verify BASE_URL configuration is defined in config module
grep -r "BASE_URL" template/src/config/ | grep -v node_modules

# Detect direct authentication endpoint calls outside AuthHook module
grep -r "axios\.post.*auth/local" template/src/ | grep -v "AuthHook" | grep -v node_modules
```

**Accept when:**
- All authentication operations use axios.post with explicit /auth/local or /auth/local/register endpoint paths
- The useAuth hook is exported as the public contract and imported by UI components requiring authentication
- BASE_URL configuration is defined in a separate config module and referenced by authentication service boundary code
- No direct authentication endpoint calls exist outside of designated service boundary modules
- Error handling maps axios errors to domain-specific authentication error types
- TypeScript interfaces are defined for authentication request and response contracts

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication operations MUST conform to the useAuth hook service boundary pattern. CI pipeline MUST fail if grep patterns detect authentication endpoint calls outside of AuthHook module. Pull requests introducing authentication logic bypass MUST be flagged for architectural review.
</enforcement>