# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Configuration Values Such

These rules are ALWAYS ACTIVE for all files in `template/src/components/AuthHook/` and related authentication modules that implement credential storage, HTTP communication, and state management for authentication operations.

### Rules

- **R-AUTH-001** MAY: Configuration values such as BASE_URL MAY be imported from a centralized config module.
- **R-AUTH-002** MUST: Use React hooks pattern (useMemo, useEffect) for authentication logic encapsulation.
- **R-AUTH-003** MUST: Implement Redux Toolkit for authentication state management.
- **R-AUTH-004** MUST: Use axios for HTTP requests to authentication endpoints (/auth/local, /auth/local/register).
- **R-AUTH-005** MUST: Use react-native-secure-storage for credential storage on mobile platforms.
- **R-AUTH-006** MUST: Export useAuth hook as the public API contract for authentication operations.
- **R-AUTH-007** SHOULD: Wrap authentication state selectors in useMemo to prevent unnecessary re-renders.
- **R-AUTH-008** SHOULD: Use useEffect for side effects such as persisting tokens to secure storage after successful authentication.
- **R-AUTH-009** SHOULD: Structure authentication requests with consistent payload format (identifier/email and password for login, username/email/password for registration).

### Verify

```bash
# Verify React hooks are imported and used
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit is imported
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
- Configuration values are imported from a centralized config module
- Authentication state selectors are wrapped in useMemo
- Side effects use useEffect for token persistence

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication hook implementations. Violations must be caught during code review and CI pipeline checks before merge.
</enforcement>