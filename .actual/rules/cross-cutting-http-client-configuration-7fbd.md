# Adopt Axios for Direct External Authentication API Calls in React Hooks: Http Client Configuration

These rules are ALWAYS ACTIVE for all authentication operations (login, registration) in React and React Native applications, custom hooks that expose authentication contracts (useAuth), HTTP POST requests to /auth/local and /auth/local/register endpoints, and integration with Redux Toolkit state management.

### Rules

- **R-HTTP-001** SHOULD: HTTP client configuration SHOULD be centralized in a config module to enable BASE_URL management.

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
- HTTP client configuration is centralized in a config module
- BASE_URL is managed through environment-specific configuration

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication HTTP calls MUST use the centralized axios configuration. Violations flagged during code review must be refactored before merge. Pull requests introducing alternative HTTP clients in authentication flows require architectural review and documented exception.
</enforcement>