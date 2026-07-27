# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Http Requests Authentication

These rules are ALWAYS ACTIVE for all authentication hook implementations in `template/src/components/AuthHook/` and related authentication modules that handle HTTP requests to backend authentication endpoints.

### Rules

- **R-AUTH-001** MUST: HTTP requests to authentication endpoints MUST use axios library with POST method to BASE_URL/auth/local for login and BASE_URL/auth/local/register for registration
- **R-AUTH-002** MUST: Authentication hooks MUST import and use React hooks pattern (useMemo, useEffect) for state management and side effects
- **R-AUTH-003** MUST: Redux Toolkit MUST be imported and used for authentication state management in authentication modules
- **R-AUTH-004** MUST: React-native-secure-storage MUST be imported and used for secure credential storage
- **R-AUTH-005** MUST: useAuth hook MUST be exported as the public API contract for authentication operations
- **R-AUTH-006** MUST: Axios base URL MUST be configured through centralized config module (../../config) for environment-specific endpoint configuration
- **R-AUTH-007** MUST: Authentication requests MUST use consistent payload format: identifier/email and password for login, username/email/password for registration
- **R-AUTH-008** SHOULD: Authentication state selectors SHOULD be wrapped in useMemo to prevent unnecessary re-renders
- **R-AUTH-009** SHOULD: useEffect SHOULD be used for side effects such as persisting tokens to secure storage after successful authentication

### Verify

```bash
# Verify React hooks imports in authentication modules
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit imports
grep -r "@reduxjs/toolkit" template/src/components/AuthHook/

# Verify axios POST requests to auth endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify react-native-secure-storage imports
grep -r "react-native-secure-storage" template/src/components/AuthHook/

# Verify useAuth hook export
grep -r "export.*useAuth" template/src/components/AuthHook/
```

**Accept when:**
- All authentication hook files import React and use hooks pattern (useMemo, useEffect)
- Redux Toolkit is imported and used for state management in authentication modules
- Axios is used for POST requests to /auth/local and /auth/local/register endpoints
- React-native-secure-storage is imported for credential storage
- useAuth hook is exported as the public API contract
- Axios base URL is configured through centralized config module
- Authentication requests follow consistent payload format
- State selectors are wrapped in useMemo where applicable
- Side effects use useEffect for token persistence

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All R-AUTH rules marked MUST are non-negotiable and MUST be verified before accepting authentication hook implementations. Violations require architecture review and documented exception approval from tech lead and security team.
</enforcement>