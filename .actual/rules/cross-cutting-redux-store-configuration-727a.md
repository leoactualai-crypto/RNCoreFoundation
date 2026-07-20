# Use process.env for Runtime Configuration in Redux Store Setup: Redux Store Configuration

These rules are ALWAYS ACTIVE for Redux store configuration files, particularly `template/src/store/index.ts` and related state management initialization code that determines development vs production behavior through environment-aware middleware and tooling setup.

### Rules

- **R-REDUX-001** MUST: Redux store configuration MUST read NODE_ENV from process.env to determine development vs production behavior.
- **R-REDUX-002** MUST: Store configuration MUST limit process.env access to NODE_ENV and explicitly approved non-sensitive variables only.
- **R-REDUX-003** MUST: No API keys, tokens, credentials, database connection strings, encryption keys, or third-party service credentials SHALL be read from process.env in store initialization.
- **R-REDUX-004** SHOULD: Use TypeScript const assertions or enums for valid NODE_ENV values to prevent typos and improve type safety.
- **R-REDUX-005** SHOULD: Consider wrapping process.env.NODE_ENV in a getter function to facilitate testing and future refactoring.
- **R-REDUX-006** SHOULD: Document all environment variables accessed in store setup with security classification (safe/sensitive).

### Verify

```bash
# Check for process.env access beyond NODE_ENV in store configuration
grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV

# Check for sensitive environment variable patterns in store directory
grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/

# Lint store configuration for unauthorized process.env access
npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts
```

**Accept when:**
- Store configuration only accesses process.env.NODE_ENV and no other environment variables
- No API keys, tokens, or credentials are read from process.env in store initialization
- Linting rules flag any new process.env access in store configuration files
- All environment variable access is documented with security classification
- TypeScript enums or const assertions are used for NODE_ENV value validation

<enforcement>
Clause Code MUST NOT skip or defer verification. All three verify commands MUST pass before accepting changes to Redux store configuration. Security team MUST be notified if any sensitive environment variables are accessed. Pull requests MUST be blocked until process.env access is justified and approved.
</enforcement>