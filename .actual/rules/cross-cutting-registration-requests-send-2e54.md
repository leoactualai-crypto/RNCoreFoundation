# Adopt Axios for Direct External Authentication API Calls in React Hooks: Registration Requests Send

These rules are ALWAYS ACTIVE for all authentication operations (login, registration) in React and React Native applications, custom hooks that expose authentication contracts (useAuth), and HTTP POST requests to /auth/local and /auth/local/register endpoints.

### Rules

- **R-AUTH-001** MUST: Registration requests MUST send username, email, and password fields in the request body.

### Verify

```bash
# Verify axios is used for authentication POST requests
grep -r "axios.post.*auth/local" template/src/components/AuthHook/

# Verify axios is imported in authentication modules
grep -r "import.*axios" template/src/ | grep -v node_modules

# Verify axios is declared as a dependency
npm list axios 2>/dev/null || echo 'axios not found in dependencies'

# Verify registration requests include required fields
grep -A 5 "auth/local/register" template/src/ | grep -E "(username|email|password)"
```

**Accept when:**
- All authentication POST requests in useAuth.ts use axios.post with BASE_URL-prefixed endpoints
- Axios is declared as a runtime dependency in package.json
- Authentication requests include required fields (username, email, password for registration)
- Registration request payloads contain all three required fields in the request body

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for authentication code paths.
</enforcement>