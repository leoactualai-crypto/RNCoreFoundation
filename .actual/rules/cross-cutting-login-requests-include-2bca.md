# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Login Requests Include

These rules are ALWAYS ACTIVE for all authentication hook implementations in `template/src/components/AuthHook/` and related authentication modules that handle login and registration operations.

### Rules

- **R-AUTH-001** SHOULD: Login requests SHOULD include identifier and password fields in the request body.

### Verify

```bash
# Verify React hooks are imported in authentication modules
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit is imported and used
grep -r "@reduxjs/toolkit" template/src/components/AuthHook/

# Verify axios is used for authentication endpoints
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify secure storage is imported
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
- Login request payloads include both identifier (or email) and password fields

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools, code review checklists, integration tests, and dependency analysis MUST confirm compliance before authentication modules are merged.
</enforcement>