# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Secure Credential Storage

These rules are ALWAYS ACTIVE for all authentication hook implementations in `template/src/components/AuthHook/` and related authentication modules that handle credential storage, HTTP communication, and state management.

### Rules

- **R-AUTH-001** MUST: Secure credential storage MUST be implemented using react-native-secure-storage.
- **R-AUTH-002** MUST: Authentication hooks MUST use React hooks pattern (useMemo, useEffect) for state management and side effects.
- **R-AUTH-003** MUST: Redux Toolkit MUST be used for authentication state management in authentication modules.
- **R-AUTH-004** MUST: Axios MUST be used for HTTP POST requests to authentication endpoints (/auth/local, /auth/local/register).
- **R-AUTH-005** MUST: The useAuth hook MUST be exported as the public API contract for authentication operations.
- **R-AUTH-006** SHOULD: Axios base URL SHOULD be configured through a centralized config module to enable environment-specific endpoint configuration.
- **R-AUTH-007** SHOULD: Authentication state selectors SHOULD be wrapped in useMemo to prevent unnecessary re-renders.
- **R-AUTH-008** SHOULD: useEffect SHOULD be used for side effects such as persisting tokens to secure storage after successful authentication.

### Verify

```bash
# Verify React hooks are imported and used in authentication modules
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit is imported and used
grep -r "@reduxjs/toolkit" template/src/components/AuthHook/

# Verify axios is used for authentication endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify react-native-secure-storage is imported for credential storage
grep -r "react-native-secure-storage" template/src/components/AuthHook/

# Verify useAuth hook is exported as public API
grep -r "export.*useAuth" template/src/components/AuthHook/
```

**Accept when:**
- All authentication hook files import React and use hooks pattern (useMemo, useEffect)
- Redux Toolkit is imported and used for state management in authentication modules
- Axios is used for POST requests to /auth/local and /auth/local/register endpoints
- React-native-secure-storage is imported for credential storage
- useAuth hook is exported as the public API contract
- All required dependencies (React, Redux Toolkit, axios, react-native-secure-storage) are present in package.json

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan for required imports. Code review MUST verify authentication hook structure. Integration tests MUST validate authentication flow with mocked axios requests. CI pipeline MUST fail if required dependencies are missing. Code review MUST block merge if authentication hooks don't follow the established pattern. Linting rules MUST flag non-compliant implementations. Architecture review is REQUIRED for any deviation from the standard authentication stack.
</enforcement>