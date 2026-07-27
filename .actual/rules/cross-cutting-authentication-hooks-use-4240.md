# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Authentication Hooks Use

These rules are ALWAYS ACTIVE for all authentication hook files in `template/src/components/AuthHook/` and related authentication modules that implement login, registration, and credential storage operations.

### Rules

- **R-AUTH-001** MUST: Authentication hooks MUST use React as the UI framework with hooks pattern (useMemo, useEffect) for state and side effect management.
- **R-AUTH-002** MUST: Redux Toolkit MUST be imported and used for authentication state management in authentication modules.
- **R-AUTH-003** MUST: Axios MUST be used for POST requests to authentication endpoints (/auth/local and /auth/local/register).
- **R-AUTH-004** MUST: React-native-secure-storage MUST be imported and used for secure credential storage.
- **R-AUTH-005** MUST: The useAuth hook MUST be exported as the public API contract for authentication operations.
- **R-AUTH-006** SHOULD: Axios base URL SHOULD be configured through a centralized config module to enable environment-specific endpoint configuration.
- **R-AUTH-007** SHOULD: Authentication state selectors SHOULD be wrapped in useMemo to prevent unnecessary re-renders.
- **R-AUTH-008** SHOULD: useEffect SHOULD be used for side effects such as persisting tokens to secure storage after successful authentication.

### Verify

```bash
# Verify React hooks imports
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit imports
grep -r "@reduxjs/toolkit" template/src/components/AuthHook/

# Verify axios authentication endpoint usage
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
- All required dependencies are present in package.json
- Authentication hooks follow the established pattern with consistent payload structure (identifier/email and password for login, username/email/password for registration)

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan for required imports. Code review MUST verify authentication hook structure and public API contracts. Integration tests MUST validate authentication flow with mocked axios requests. CI pipeline MUST fail if required dependencies are missing. Code review MUST block merge if authentication hooks don't follow the established pattern. Linting rules MUST flag non-compliant implementations. Architecture review is REQUIRED for any deviation from the standard authentication stack.
</enforcement>