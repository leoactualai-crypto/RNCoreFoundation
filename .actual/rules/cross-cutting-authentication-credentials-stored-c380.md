# Standardize Axios-Based External Authentication Client Boundaries: Authentication Credentials Stored

These rules are ALWAYS ACTIVE for all React and React Native authentication components, hooks, and services that communicate with external authentication endpoints and manage user credentials.

### Rules

- **R-AUTH-001** SHOULD: Authentication credentials SHOULD be stored using react-native-secure-storage for persistent secure storage across sessions.
- **R-AUTH-002** MUST: All authentication operations (login, registration, token refresh) that communicate with external authentication services MUST use axios as the HTTP client.
- **R-AUTH-003** MUST: Authentication logic MUST be encapsulated within the useAuth hook to provide a stable public contract that isolates consuming components from changes to the authentication service API.
- **R-AUTH-004** SHOULD: Axios interceptors SHOULD be configured at application initialization to handle authentication tokens, request/response logging, and consistent error handling across all authentication API calls.
- **R-AUTH-005** SHOULD: TypeScript interfaces SHOULD be defined for authentication request and response payloads to ensure type safety and document the API contract between client and service.
- **R-AUTH-006** SHOULD: Comprehensive error handling SHOULD be implemented in the useAuth hook to distinguish between network errors, authentication failures, and service errors.

### Verify

```bash
# Verify axios is used for authentication endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/ | wc -l

# Verify axios is imported in useAuth
grep -r "import.*axios" template/src/components/AuthHook/useAuth.ts

# Verify react-native-secure-storage is integrated
grep -r "react-native-secure-storage" template/src/components/AuthHook/useAuth.ts
```

**Accept when:**
- All authentication operations (login, registration) use axios.post with BASE_URL configuration and standardized endpoint paths
- The useAuth hook exports a public contract that abstracts axios implementation details from consuming components
- Credential storage integration with react-native-secure-storage is present in the authentication hook implementation
- Axios interceptors are configured for consistent error handling and token management
- TypeScript interfaces are defined for authentication API contracts
- Error handling distinguishes between network errors, authentication failures, and service errors

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication operations MUST use the established axios-based pattern through the useAuth hook. Pull requests introducing authentication logic outside this pattern or using alternative HTTP clients MUST be flagged during code review.
</enforcement>