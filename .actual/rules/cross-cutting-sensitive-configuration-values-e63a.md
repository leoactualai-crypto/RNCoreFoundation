# Use process.env for Runtime Configuration in Redux Store Setup: Sensitive Configuration Values

These rules are ALWAYS ACTIVE for Redux store initialization and configuration files, particularly `template/src/store/index.ts` and related state management setup code.

### Rules

- **R-CONFIG-001** SHOULD: Sensitive configuration values SHOULD be injected through configuration modules rather than read directly in store setup.
- **R-CONFIG-002** MUST: Store configuration MUST only access `process.env.NODE_ENV` and no other environment variables.
- **R-CONFIG-003** MUST: No API keys, tokens, credentials, database connection strings, service URLs, encryption keys, or signing secrets MUST be read from `process.env` in store initialization.
- **R-CONFIG-004** SHOULD: `process.env.NODE_ENV` access SHOULD be wrapped in a getter function to facilitate testing and future refactoring.
- **R-CONFIG-005** SHOULD: Valid NODE_ENV values SHOULD be defined using TypeScript const assertions or enums to prevent typos.
- **R-CONFIG-006** SHOULD: All environment variables accessed in store setup SHOULD be documented with security classification (safe/sensitive).

### Verify

```bash
# Check for process.env access beyond NODE_ENV
grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV

# Check for sensitive variable patterns (keys, tokens, secrets)
grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/
grep -rn 'process\.env\.[A-Z_]*TOKEN' template/src/store/
grep -rn 'process\.env\.[A-Z_]*SECRET' template/src/store/

# Lint for process.env usage
npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts
```

**Accept when:**
- Store configuration only accesses `process.env.NODE_ENV` and no other environment variables
- No API keys, tokens, credentials, database URLs, or secrets are read from `process.env` in store initialization
- Linting rules flag any new `process.env` access in store configuration files
- All `process.env` access in store setup is documented with security classification
- `NODE_ENV` values are defined using TypeScript const assertions or enums

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for store configuration changes. Code review checklist and static analysis must pass before acceptance.
</enforcement>