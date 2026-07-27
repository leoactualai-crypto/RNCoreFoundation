# Adopt React with Redux Toolkit and Axios for Authentication Hook Implementation: Authentication Hooks Expose

These rules are ALWAYS ACTIVE for all authentication hook files in `template/src/components/AuthHook/` and related authentication modules that implement the public authentication contract.

### Rules

- **R-AUTH-001** MUST: Authentication hooks MUST expose useAuth as the public contract interface.

### Verify

```bash
# Verify React hooks are imported and used in authentication modules
grep -r "import.*react.*from 'react'" template/src/components/AuthHook/

# Verify Redux Toolkit is imported for state management
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

<enforcement>
Claude Code MUST NOT skip or defer verification. All five verify commands must pass before accepting authentication hook implementations.
</enforcement>