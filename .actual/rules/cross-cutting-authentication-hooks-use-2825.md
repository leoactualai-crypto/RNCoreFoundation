# Adopt Axios for Direct External Authentication API Calls in React Hooks: Authentication Hooks Use

These rules are ALWAYS ACTIVE for all authentication hooks and React components that communicate with external authentication endpoints (login, registration) via HTTP POST requests.

### Rules

- **R-AUTH-001** MUST: Authentication hooks MUST use axios for HTTP POST requests to external authentication endpoints.

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
- Axios is configured with centralized defaults (timeout, headers) in a dedicated config module
- Error handling is consistently applied across all authentication axios calls

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication HTTP calls must be reviewed for axios compliance before merge. Violations require architectural review and documented exceptions.
</enforcement>