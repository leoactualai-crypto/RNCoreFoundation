# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: State Management Authentication

These rules are ALWAYS ACTIVE for all files in `template/src/components/AuthHook/` and related authentication modules that implement state management for authentication operations.

### Rules

- **R-AUTH-001** MUST: State management for authentication MUST be implemented using @reduxjs/toolkit.
- **R-AUTH-002** MUST: Authentication hooks MUST import React hooks (useMemo, useEffect) from the React library.
- **R-AUTH-003** MUST: HTTP requests to authentication endpoints MUST use axios as the HTTP client.
- **R-AUTH-004** MUST: Sensitive authentication credentials MUST be stored using react-native-secure-storage for mobile platform compatibility.
- **R-AUTH-005** MUST: The useAuth hook MUST be exported as the public API contract for authentication operations.
- **R-AUTH-006** SHOULD: Axios base URL SHOULD be configured through a centralized config module to enable environment-specific endpoint configuration.
- **R-AUTH-007** SHOULD: Authentication state selectors SHOULD be wrapped in useMemo to prevent unnecessary re-renders.
- **R-AUTH-008** SHOULD: useEffect SHOULD be used for side effects such as persisting tokens to secure storage after successful authentication.

### Verify

```bash
# Verify React hooks are imported in authentication modules
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit is imported and used
grep -r "@reduxjs/toolkit" template/src/components/AuthHook/

# Verify axios is used for authentication endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify react-native-secure-storage is imported
grep -r "react-native-secure-storage" template/src/components/AuthHook/

# Verify useAuth hook is exported
grep -r "export.*useAuth" template/src/components/AuthHook/
```

**Accept when:**
- All authentication hook files import React and use hooks pattern (useMemo, useEffect)
- Redux Toolkit is imported and used for state management in authentication modules
- Axios is used for POST requests to /auth/local and /auth/local/register endpoints
- React-native-secure-storage is imported for credential storage
- useAuth hook is exported as the public API contract
- Axios base URL is configured through centralized config module
- Authentication state selectors are wrapped in useMemo
- useEffect is used for token persistence side effects

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All R-AUTH rules marked MUST are non-negotiable for authentication implementations. Violations require architecture review and documented exception approval from tech lead and security team.
</enforcement>