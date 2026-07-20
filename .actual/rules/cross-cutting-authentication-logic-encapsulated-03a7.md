# Adopt Axios for Direct External Authentication API Calls in React Hooks: Authentication Logic Encapsulated

These rules are ALWAYS ACTIVE for all authentication operations in React and React Native applications that communicate with external authentication APIs, specifically custom hooks that expose authentication contracts (useAuth) and HTTP POST requests to /auth/local and /auth/local/register endpoints.

### Rules

- **R-AUTH-001** MUST: Authentication logic MUST be encapsulated in custom React hooks (e.g., useAuth) that expose public contracts.
- **R-AUTH-002** MUST: All authentication POST requests to /auth/local and /auth/local/register endpoints MUST use axios as the HTTP client.
- **R-AUTH-003** MUST: Axios MUST be declared as a runtime dependency in package.json with a specific version constraint (e.g., ^1.6.0).
- **R-AUTH-004** SHOULD: Axios defaults (timeout, headers) SHOULD be configured in a centralized config module imported by authentication hooks.
- **R-AUTH-005** SHOULD: Error handling SHOULD be wrapped around axios calls to normalize error responses from authentication endpoints.
- **R-AUTH-006** SHOULD: Repeated axios.post patterns SHOULD be extracted into a createAuthRequest helper function to reduce duplication.
- **R-AUTH-007** SHOULD: Endpoint definitions SHOULD be centralized in a constants file or API configuration module with typed endpoint builders.
- **R-AUTH-008** MAY: Alternative HTTP clients (fetch, ky, etc.) MAY be used if axios introduces bundle size constraints in production builds (EXC-001).

### Verify

```bash
# Verify axios is used for authentication POST requests
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify axios is imported in authentication modules
grep -r "import.*axios" template/src/ | grep -v node_modules

# Verify axios is declared as a dependency
npm list axios 2>/dev/null || echo 'axios not found in dependencies'
```

**Accept when:**
- All authentication POST requests in useAuth.ts use axios.post with BASE_URL-prefixed endpoints
- Axios is declared as a runtime dependency in package.json
- Authentication requests include required fields (identifier/password for login, username/email/password for registration)
- Axios configuration is centralized in a dedicated config module
- Error handling is consistently applied across all authentication axios calls

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication API integration. Violations flagged during code review must be refactored before merge. Existing violations should be tracked as technical debt items with prioritized remediation. Exceptions require architectural review board approval with documented justification and performance analysis.
</enforcement>