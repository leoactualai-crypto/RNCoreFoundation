# Adopt Axios for Direct External Authentication API Calls in React Hooks: Authentication Calls Target

These rules are ALWAYS ACTIVE for all authentication operations (login, registration) in React and React Native applications, custom hooks that expose authentication contracts (useAuth), and HTTP POST requests to /auth/local and /auth/local/register endpoints.

### Rules

- **R-AUTH-001** MUST: Authentication API calls MUST target BASE_URL-prefixed endpoints (/auth/local, /auth/local/register).

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
- No alternative HTTP clients (fetch, ky, etc.) are used for authentication calls unless documented exception EXC-001 is approved

<enforcement>
Clause Code MUST NOT skip or defer verification. All authentication API calls must be inspected to confirm axios usage with BASE_URL-prefixed endpoints before merge.
</enforcement>