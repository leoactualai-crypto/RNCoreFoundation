# Standardize Axios-Based External Authentication Client Boundaries: Authentication Service Boundaries

These rules are ALWAYS ACTIVE for all React and React Native components and hooks that communicate with external authentication services, particularly those implementing user authentication flows including login and registration.

### Rules

- **R-AUTH-001** MUST: Authentication service boundaries MUST be accessed through the BASE_URL configuration variable with standardized endpoint paths (/auth/local, /auth/local/register).
- **R-AUTH-002** MUST: All authentication operations (login, registration, token refresh) that communicate with external authentication services MUST use axios as the HTTP client.
- **R-AUTH-003** MUST: Authentication logic MUST be encapsulated within the useAuth hook to provide a stable public contract that isolates consuming components from changes to the authentication service API or HTTP implementation.
- **R-AUTH-004** MUST: Credential storage MUST integrate with react-native-secure-storage for secure persistence of sensitive user data across application sessions.
- **R-AUTH-005** SHOULD: Axios interceptors SHOULD be configured at application initialization to handle authentication tokens, request/response logging, and consistent error handling across all authentication API calls.
- **R-AUTH-006** SHOULD: TypeScript interfaces SHOULD be defined for authentication request and response payloads to ensure type safety and document the API contract between client and service.
- **R-AUTH-007** SHOULD: Comprehensive error handling SHOULD be implemented in the useAuth hook to distinguish between network errors, authentication failures, and service errors, providing appropriate user feedback for each case.

### Verify

```bash
# Verify axios usage in authentication operations
grep -r "axios.post.*auth/local" template/src/components/AuthHook/ | wc -l

# Verify axios import in useAuth hook
grep -r "import.*axios" template/src/components/AuthHook/useAuth.ts

# Verify react-native-secure-storage integration
grep -r "react-native-secure-storage" template/src/components/AuthHook/useAuth.ts
```

**Accept when:**
- All authentication operations (login, registration) use axios.post with BASE_URL configuration and standardized endpoint paths (/auth/local, /auth/local/register)
- The useAuth hook exports a public contract that abstracts axios implementation details from consuming components
- Credential storage integration with react-native-secure-storage is present in the authentication hook implementation
- No direct authentication API calls exist outside of the designated useAuth hook
- Axios interceptors are configured for consistent error handling and token management

<enforcement>
Claude Code MUST NOT skip or defer verification of these authentication service boundary rules. All authentication operations must be reviewed to ensure they comply with the axios-based pattern and BASE_URL configuration requirement.
</enforcement>