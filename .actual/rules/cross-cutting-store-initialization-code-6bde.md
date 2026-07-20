# Use process.env for Runtime Configuration in Redux Store Setup: Store Initialization Code

These rules are ALWAYS ACTIVE for Redux store initialization and configuration code, particularly `template/src/store/index.ts` and related state management setup files.

### Rules

- **R-STORE-001** MUST_NOT: Store initialization code MUST NOT access process.env variables containing secrets or credentials directly.

### Verify

```bash
# Check for process.env access other than NODE_ENV in store configuration
grep -n 'process\.env' template/src/store/index.ts | grep -v NODE_ENV

# Check for sensitive environment variable patterns (API keys, tokens, etc.)
grep -rn 'process\.env\.[A-Z_]*KEY' template/src/store/

# Lint store configuration for process.env usage
npm run lint -- --rule 'no-process-env: error' template/src/store/index.ts
```

**Accept when:**
- Store configuration only accesses process.env.NODE_ENV and no other environment variables
- No API keys, tokens, or credentials are read from process.env in store initialization
- Linting rules flag any new process.env access in store configuration files
- All environment variables accessed in store setup are documented with security classification

<enforcement>
Clause R-STORE-001 verification is mandatory. Code review and static analysis MUST confirm that store initialization does not access sensitive environment variables before changes are merged.
</enforcement>