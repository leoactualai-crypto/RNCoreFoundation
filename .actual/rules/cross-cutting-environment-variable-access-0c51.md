# Use process.env for Runtime Configuration in Redux Store Setup: Environment Variable Access

These rules are ALWAYS ACTIVE for Redux store configuration files and any code that initializes state management infrastructure, particularly template/src/store/index.ts and related store setup modules.

### Rules

- **R-ENV-001** SHOULD: Environment variable access in store configuration SHOULD be limited to non-sensitive runtime mode indicators (NODE_ENV, DEBUG flags).
- **R-ENV-002** MUST: Store configuration MUST NOT access API keys, tokens, authentication credentials, database connection strings, service URLs, encryption keys, signing secrets, or third-party service credentials from process.env.
- **R-ENV-003** SHOULD: All environment variables accessed in store setup SHOULD be documented with security classification (safe/sensitive).
- **R-ENV-004** SHOULD: process.env.NODE_ENV access SHOULD be wrapped in a getter function or const assertion to facilitate testing and future refactoring.
- **R-ENV-005** SHOULD: TypeScript const assertions or enums SHOULD be used for valid NODE_ENV values to prevent typos.

### Verify

```bash
# Check for process.env access beyond NODE_ENV
grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV

# Check for sensitive environment variable patterns
grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/

# Lint for process.env usage
npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts
```

**Accept when:**
- Store configuration only accesses process.env.NODE_ENV and no other environment variables
- No API keys, tokens, or credentials are read from process.env in store initialization
- Linting rules flag any new process.env access in store configuration files
- All environment variables accessed in store setup are documented with security classification

<enforcement>
Clause Code MUST NOT skip or defer verification. All process.env access in store configuration must be reviewed against these rules before approval.
</enforcement>