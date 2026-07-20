# Use process.env for Runtime Configuration in Redux Store Setup: Store Configuration Use

These rules are ALWAYS ACTIVE for Redux store configuration files and state management initialization code that accesses runtime environment variables.

### Rules

- **R-STORE-001** MAY: Store configuration MAY use process.env for feature flags and non-sensitive runtime switches.
- **R-STORE-002** MUST: Limit process.env access in store configuration to NODE_ENV and explicitly approved non-sensitive variables only.
- **R-STORE-003** MUST NOT: Access API keys, tokens, authentication credentials, database connection strings, encryption keys, or third-party service credentials from process.env in store initialization.
- **R-STORE-004** SHOULD: Use TypeScript const assertions or enums for valid NODE_ENV values to prevent typos.
- **R-STORE-005** SHOULD: Consider wrapping process.env.NODE_ENV in a getter function to facilitate testing and future refactoring.
- **R-STORE-006** SHOULD: Document all environment variables accessed in store setup with security classification (safe/sensitive).

### Verify

```bash
# Check for process.env access beyond NODE_ENV
grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV

# Check for sensitive environment variable patterns
grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/

# Lint for process.env usage violations
npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts
```

**Accept when:**
- Store configuration only accesses process.env.NODE_ENV and no other environment variables
- No API keys, tokens, or credentials are read from process.env in store initialization
- Linting rules flag any new process.env access in store configuration files
- All environment variables accessed are documented with security classification
- NODE_ENV values are defined as TypeScript const assertions or enums

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for store configuration changes.
</enforcement>