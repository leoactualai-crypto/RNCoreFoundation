# Standardize Axios-Based External Authentication Client Boundaries: Authentication State Management

These rules are ALWAYS ACTIVE for all React and React Native authentication hooks, components, and services that communicate with external authentication endpoints at BASE_URL/auth.

### Rules

- **R-AUTH-001** MUST: All authentication operations (login, registration, token refresh) that communicate with external authentication services MUST use axios as the HTTP client.
- **R-AUTH-002** SHOULD: Authentication state management SHOULD integrate with @reduxjs/toolkit for global state coordination across the application.
- **R-AUTH-003** MUST: Credential storage MUST use react-native-secure-storage for secure persistence of authentication tokens and sensitive user data.
- **R-AUTH-004** MUST: Authentication logic MUST be encapsulated within the useAuth hook to provide a stable public contract that isolates consuming components from changes to the authentication service API.
- **R-AUTH-005** MUST: All axios HTTP client calls for authentication MUST be configured with interceptors for consistent error handling, request/response logging, and token management.
- **R-AUTH-006** MUST: TypeScript interfaces MUST be defined for all authentication request and response payloads to ensure type safety and document the API contract between client and service.
- **R-AUTH-007** MUST: Error handling in the useAuth hook MUST distinguish between network errors, authentication failures, and service errors, providing appropriate user feedback for each case.
- **R-AUTH-008** SHOULD: Axios configuration and BASE_URL management SHOULD be extracted into a separate configuration module to facilitate environment-specific configuration and testing.

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
- Axios interceptors are configured for error handling and token management
- TypeScript interfaces are defined for authentication API payloads
- Error handling distinguishes between network errors, authentication failures, and service errors

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All authentication client boundaries MUST conform to the axios-based pattern with @reduxjs/toolkit integration and react-native-secure-storage credential management.
</enforcement>