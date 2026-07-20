# Adopt Axios for Direct External Authentication API Calls in React Hooks: Authentication Hooks Integrate

These rules are ALWAYS ACTIVE for authentication hooks and components that communicate with external authentication APIs in React and React Native applications.

### Rules

- **R-AUTH-001** MUST: Use axios for all authentication POST requests to external API endpoints (/auth/local, /auth/local/register).
- **R-AUTH-002** MUST: Configure axios with a centralized BASE_URL constant for authentication endpoint management.
- **R-AUTH-003** MUST: Declare axios as a runtime dependency in package.json with a specific version constraint (e.g., ^1.6.0).
- **R-AUTH-004** SHOULD: Extract repeated axios.post patterns into reusable helper functions to reduce code duplication.
- **R-AUTH-005** SHOULD: Implement error handling wrappers around axios calls to normalize error responses from authentication endpoints.
- **R-AUTH-006** SHOULD: Centralize endpoint definitions in a constants file or API configuration module with typed endpoint builders.
- **R-AUTH-007** MAY: Authentication hooks MAY integrate with React Native Secure Storage for credential persistence.
- **R-AUTH-008** MAY: Alternative HTTP clients (fetch, ky, etc.) may be used if axios introduces bundle size constraints in production builds (requires architectural review and documented justification).

### Verify

```bash
# Verify axios is used for authentication POST requests
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify axios is imported in authentication modules
grep -r "import.*axios" template/src/ | grep -v node_modules

# Verify axios is declared as a dependency
npm list axios 2>/dev/null || echo 'axios not found in dependencies'

# Verify authentication requests include required fields
grep -r "identifier\|password\|username\|email" template/src/components/AuthHook/ | grep -E "(login|register)"
```

**Accept when:**
- All authentication POST requests in useAuth.ts use axios.post with BASE_URL-prefixed endpoints
- Axios is declared as a runtime dependency in package.json with a specific version constraint
- Authentication requests include required fields (identifier/password for login, username/email/password for registration)
- Axios configuration (timeout, headers) is centralized in a dedicated config module
- Error handling is consistently applied across all authentication axios calls

<enforcement>
Claude Code MUST NOT skip or defer verification. All authentication HTTP calls MUST use axios. Violations require architectural review before merge. Existing violations must be tracked as technical debt with prioritized remediation.
</enforcement>